### 기존 프로젝트를 기존 repository에 push하기
##### .git 생성
```
$ git init
```

##### 만일 원하는 branch명이 있을 경우 branch checkout
```
$ git checkout -b <<branch name>>
$ git add .
$ git commit -m "clone project"
$ git remote add origin <<repository url>>
$ git push origin <<branch name>>
```

참고:[하루하나](https://cpdev.tistory.com/60)
