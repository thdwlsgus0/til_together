# Git 명령어 정리

### 생성하기

* 원격 Git 저장소 복제
```
git clone <URL>
```

### Branch 작업하기

* branch 생성
```
git branch -b <branchname>
```

 Tip. checkout 명령에 -b 옵션을 넣으면 브랜치 작성과 체크아웃을 한꺼번에 실행할 수 있습니다.


* branch 이동
```
git checkout <branchname>
```

* 내 디렉토리 만들기
```
mkdir 디렉토리명
```

* 디렉토리 이동
```
cd 디렉토리명/
```

* 디렉토리에서 파일 만들기


*  staging area에 추가
```
git add * 
```
```
git add <filename> 
```

* commit
```
git commit -m <branchname>
```

* commit한 이력이 repository로 저장
```
git push origin 브랜치 이름
```

* 원격저장소 변경 사항(이력)을 받아오기
```
 git pull origin master
```


### 살펴보기

* 파일 상태 확인
```
git status
```