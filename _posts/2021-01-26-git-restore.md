---
layout: post
title: 'git restore 명령어 정리'
subtitle: 'git restore'
categories: development
tags: git
comments: true
---

- git restore 명령어에 대해서 공부하고 정리한 글입니다

---

### restore

- 다음과 같은 경우에 커밋을 취소할 수 있다

  - 너무 커밋을 성급하게 해서 모든 수정사항을 커밋하지 못한 경우

  - 커밋 메시지가 마음에 들지 않는 경우

  - 커밋을 너무 많이 해서 여러개로 나누어서 커밋을 다시하고 싶거나 또는 반대로 커밋을 너무 조금해서 하나로 묶고 싶은 경우

  - 내가 실수로 버그를 도입해서 버그가 도입된 커밋을 삭제하거나 돌리고 싶은 경우

- 다만 이 때 git history를 수정하는 경우에는 내 local 에 있는 것들만 변경해야 한다

- push 명령어를 통해 이미 서버에 올려두고 다른 개발자와 이미 공유된 history라면 절대 수정하면 안된다

- 아래와 같은 커밋있다고 하자

- WIP는 Working In Process의 줄임말로 아직 일이 진행죽인데 commit이 되었다

- 그리고 무슨 일을 하고 있는지도 작성해야 되는데 내용이 빠져있다

- 그리고 커밋메시지가 `.`인것을 통해 굉장히 의미 없는 커밋이 생긴 것을 알 수 있다

```bash
* Forth Commit (HEAD -> master)
* .
* Third Commit
* WIP
* Sceond Commit
* Initialise Commit

```

- 로컬에서 작업하고 있는 stating area나 working directory에서 작업하는 내용을 초기화할 수 있는 방법

  - 워킹 디렉토리에 있는 파일들을 초기화 하고 싶다면 `git restore` 명령어 사용한다

  - 이전에는 `git checkout —"파일이름"` 도 사용했지만 checkout으로 할 수 있는 동작들이 너무 많기 때문에 좀 더 범위를 좁혀 직관적으로 수행하는 작업을 알 수 있도록 `git resotre`을 사용한다

  - `git checkout "브랜치이름"` 대신 `git switch "브랜치이름" `을 사용하는 것과 같다

```bash
$ echo add >> file1.txt
# 파일 수정

$ git status
# 확인

$ git restore file1.txt
# 파일 초기화

$ git restore .
# 워킹 디렉토리에 있는 모든 파일 초기화
```

- 수정한 파일이 staging area에 있는 경우

```bash
$ echo add >> file1.txt

$ git add file1.txt
# staging area에 추가

$ git restore --staged file1.txt

$ git status
# 다시 워킹 디렉토리로 옮겨져 온 것을 확인할 수 있다

$ git restore .
# 워킹 디렉토리에 있는 모든 파일 초기화

$ git status
# 초기화된 것 확인할 수 있다
```

- `git reset` 명령어를 사용하는 방법도 있다

- reset 옵션을 사용하면 내가 가고자 하는 포인터를 가르킬 수 있다

```bash
$ git reset HEAD .
# 헤드 버전으로 가리키는 모든 파일에 대해 적용
# stating area에 있는 모든 요소들이 다시 워킹 디렉토리로 옮겨진다

$ git status
# unstaged 된 것 확인 가능
```

- 아래와 같은 커밋이 있을 때 어떤 커밋으로부터 어떠한 파일을 초기화할지 설정할 수 있다

```bash
* Forth Commit (HEAD -> master)
* .
* Third Commit
* WIP
* Sceond Commit
* Initialise Commit
```

```bash
$ git restore --source=HEAD~2 file3.txt
# HEAD 에서 두번째 떨어진 곳에서 file3.txt을 restore하겠다

$ git restore --source="해쉬코드" file3.txt
# 해쉬코드 이름을 작성해도 된다

$ git status
# 파일 삭제된 것 확인할 수 있다
```
