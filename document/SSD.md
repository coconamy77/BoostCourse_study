# 개발자를 위한 SSD : 페이지 & 블록 & FTL(Flash Translation Layer)

참조 : [개발자를 위한 SSD from kakaoTech](http://tech.kakao.com/2016/07/15/coding-for-ssd-part-3/)

## 기본 오퍼레이션

### 읽기 & 쓰기 & 삭제(Erase)

- NAND 플래시 메모리의 구성 특성상, 특정 셀을 단독으로 읽고 쓰는 작업은 불가능하다.메모리는 그룹핑되어 있으며, 아주 특별한 방법으로만 접근할 수 있다.

- ##### 읽기는 페이지 사이즈 단위로 실행(Reads are aligned on page size)

  - 한번에 하나의 페이지보다 작은 크기의 데이터를 읽을 수는 없다.

- ##### 쓰기는 페이지 사이즈 단위로 실행 (Writes are aligned on page size)

  - 쓰기를 실행할 때에도 SSD는 페이지 단위로, 하나의 페이지 또는 여러 개의 페이지로 실행된다.
  - 필요 이상으로 쓰기가 발생하는 것(Write Overhead)을 “Write Amplication”이라고 한다.

- ##### 페이지는 덮어 쓰기(Overwrite)될 수 없다 (Pages cannot be overwritten)

  - NAND 플레시 메모리의 페이지는 반드시 “free” 상태일때에만 쓰기를 할 수 있다.
  - 데이터가 변경되면, 페이지의 내용은 내부 레지스터로 복사된 후 레지스터에서 변경되어 새로운 “free” 상태의 페이지로 기록되는 것이다.이를 “Read-Modify-Write”라고 한다.
  - 변경된 데이터가 새로운 페이지에 완전히 기록되면, 원본 페이지는 “stale”로 마킹되고 삭제(Erase)되기 전까지 그 상태로 남게 된다.

- ##### 삭제(Erase)는 블록 사이즈 단위로 실행 (Erases are aligned on block size)

  - 페이지는 덮어 쓰기가 불가능하기 때문에 한번 “stale” 상태로 된 페이지는 반드시 삭제(Erase)하는 작업을 거쳐서 “free” 상태로 전이할 수 있다.
  - 삭제 명령은 SSD 컨트롤러가 “free” 공간이 필요할 때자동적으로 내부 명령을 실행해서 Garbage-collection을 실행할 때 사용된다.

### Write amplification

- 쓰기는 페이지 사이즈에 맞춰서(Aligned) 실행되므로, 페이지 사이즈에 일치하지 않는 모든 쓰기는필요 이상의 부가적인 쓰기(Write amplification)를 필요로 한다.

- ##### 페이지 사이즈보다 작은 데이터 쓰기는 피하자

- ##### 쓰기 맞춤 (Align writes)을 하자

- ##### 작은 데이터의 쓰기는 버퍼링 (Buffer small writes)

  - 스루풋을 최대화하기 위해서, 가능하면 작은 쓰기는 메모리에 버퍼링했다가 버퍼가 가득 차면 단일 쓰기로 최대한 많은 데이터를 기록하도록 하자.

### Wear leveling

- 프로그램-삭제(P/E Cycles) 회수가 제한되어 있으므로 NAND 플래시 셀은 제한된 수명을 가지게 된다. 그래서 하나의 셀을 계속해서 사용하다 보면 그 셀은 노화되어 사용할 수 없게 되고, 그럼 전체적인 ssd의 용량이 줄어드는 것이다.
- SSD 컨트롤러의 중요한 역할 중 하나는, SSD의 전체 블록에 대해서 P/E cycle이 골고루 분산되도록 쓰기(“Wear leveling”)를 실행하는 것이다.

### Garbage Collection

- SSD 컨트롤러의 Garbage-collection 프로세스는 “stale” 상태의 페이지들이 삭제(Erase)되어 새로운 쓰기 데이터를 저장할 수 있도록 해주는 과정이다.

- background gc(Idle Collection)

  - SSD 컨트롤러가 한가한 시간에 주기적으로 “stale” 페이지를 “free” 상태로 만들어 준다.

- Parallel Garbage-collection

  - 호스트의 쓰기 요청과 동시에 Garbage-collection을 실행하는 방식이다.

- ##### 콜드 데이터와 핫 데이터 분리

  - 콜드 데이터(정적 데이터): 변경이 뜸한 데이터
  - 핫 데이터(동적 데이터): 변경이 빈번한 데이터
  - 두 데이터가 한 페이지 안에 있다면 write Amplification이 심해지므로, 두 데이터를 분리 하고 wear leveling을 위해 주기적으로 swap해준다.
  - 이는 Garbage-collection이 좀 더 효율적으로 작동하도록 해준다.

- ##### 핫 데이터 버퍼링

  - 매우 빈번하게 변경되는 핫 데이터는 최대한 버퍼링되었다가 SSD에 덜 자주 업데이트되도록 하는 것이 좋다.

- ##### 불필요한 데이터는 한번에 많이 삭제

  - 데이터가 더 이상 필요치 않거나 삭제해야 할 때에는 최대한 모아서 한번(단일 오퍼레이션)에 삭제하는 것이 좋다.
  - 단일 오퍼레이션으로 삭제하는 것이 Garbage-collection 처리가 한번에 큰 영역을 처리하도록 해주며 그와 동시에 내부 프레그멘테이션을 최소화시켜 줄 것이다.