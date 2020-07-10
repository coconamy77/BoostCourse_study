# git 명령어

## 기존 프로젝트를 기존 repository에 push하기

##### .git 생성
```
$ git init
```

##### 만일 원하는 branch명이 있을 경우 branch checkout
```bash
$ git checkout -b <<branch name>>
$ git add .
$ git commit -m "clone project"
$ git remote add origin <<repository url>>
$ git push origin <<branch name>>
```



## git add 취소하기

```bash
//파일들의 상태 확인
$ git status 
//git add 취소, 뒤에 파일명이 없으면 전체 취소
$ git reset <<branch>> ReadMe.md
```



## git commit 취소하기

```bash
//commit 목록확인
$ git log

// [방법 1] commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
$ git reset --soft HEAD^
// [방법 2] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에 보존
$ git reset --mixed HEAD^ // 기본 옵션
$ git reset HEAD^ // 위와 동일
$ git reset HEAD~2 // 마지막 2개의 commit을 취소
// [방법 3] commit을 취소하고 해당 파일들은 unstaged 상태로 워킹 디렉터리에서 삭제
$ git reset --hard HEAD^

// 워킹 디렉터리를 원격 저장소의 마지막 commit 상태로 되돌린다.
$ git reset --hard HEAD
```











참고 :

[하루하나](https://cpdev.tistory.com/60)

[https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)