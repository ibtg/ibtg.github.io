---
layout: post
title: 'git status 명령어 정리'
subtitle: 'git status'
categories: development
tags: git
comments: true
---

- git의 status(상태)에 대해서 공부하고 정리한 글입니다

---

- Git은 `[.git]` 폴더에 버전 관리한 데이터(어디까지 작업했는지에 대한 정보)와 이를 올릴 원격 저장소 주소등 필요한 정보를 저장한다

```bash
$ git init
# [.git] 폴더 생성된다
```

---

- `origin`은 우리가 연결한 GitHub 원격 저장소의 닉네임이다
- `master`는 우리가 커밋을 올리는 '줄기'의 이름

- 커밋을 출력해보면 줄줄이 하나의 줄기로 이어져 있는 것을 확인할 수 있는데 따로 줄기를 생성하지 않으면 기본 줄기인 `master`에 커밋을 올린다

- `[master]`는 내 로컬 저장소의 버전

- `[origin/master]`는 GitHub의 원격 저장소의 버전을 가리킨다

```bash
$ git remote add origin "원격 저장소 주소"
# origin이라는 이름으로 원격 저장소 추가
```

---

- add

  - 하나의 버전을 만들기 위해 변경사항을 선택하는 과정

- commit

  - 그렇게 선택한 변경사항을 하나로 묶어 버전으로 만든 것이

- Git은 커밋에 바뀐 것만 저장하는 것이 아니라 전체 코드를 저장한다.

- 차이점만 저장하는 방식이 용량도 작고 빠를 것 같지만 실제로는 그렇지 않다

- 생각해 보면 차이점을 저장하는 방식을 사용하는 경우에는 버전을 보여줄 때 맨 처음 파일이 만들어 졌을 때 까지 거슬러 올라가면서 바뀐 점을 모두 반영하는 계산을 해야 한다

- 즉, 100번 버전이 바뀐 경우에는 새롭게 버전이 만들어지는 경우에는, 이전 버전들과 비교하는 100번의 연산이 필요하다.

- 하지만 Git은 앞에서 바뀐 커밋과 비교하는 연산 한번이면 충분하다

---

- 워킹 디렉토리(working Directory)

  - 일반적으로 사용자가 파일과 하위 폴더를 만들고 작업 결과물을 저장하는 곳

  - 일반적인 작업 폴더를 git 용어로 워킹 디렉토리(또는 워킹트리)라고 한다

  - 작업 폴더에서 [.git]폴더 (로컬저장소)를 뺀 나머지 부분을 말한다

- 로컬 저장소(local repository)

  - git init 명령어로 생성되는 [.git]폴더

  - 커밋, 커밋을 구성하는 객체, 스테이지가 모두 이 폴더에 저장된다

- 원격 저장소(remote repository)

  - 로컬 저장소를 업로드하는 곳

- Git 저장소

  - git 명령어로 관리할 수 있는 폴더 전체를 일반적으로 git 프로젝트 혹은 git 저장소라고 한다

  - 일반적으로는 워킹트리 + 로컬저장소 느낌으로 사용한다

  - 공식문서에서는 로컬저장소와 git 저장소를 같은 뜻으로 사용하고 있다

  - 즉, 워킹 디렉토리에서 작업을 하고, `add`명령어를 통해서 staging area에 올리고 `commit`명령어를 사용해서 Git 저장소에 버전을 추가한다

---

- git status는 git 저장소의 상태를 알려주는 명령어

```bash

# Git 저장소의 상태를 알려주는 명령어
# git status는 Git 저장소(정확하게는 워킹트리)에서만 정상적으로 수행된다
$ git status


# git status 명령 보다 짧게 요약해서 상태를 보여주는 명령어로,
# 변경된 파일이 많을 때 유용하다
$ git status -s

# 아래와 같은 방법으로 git status를 git st로 줄여서 쓸 수 있다
$ git config --global alias.st status

# git 관련된 명령어 볼 수 있다
$ git config --h

```

- git status 명령어는 git 저장소(정확하게는 워킹트리)에서만 정상적으로 수행되는 명령어이다

- git init을 통해서 [.git] 폴더가 있는 상태에서만 정상적으로 수행된다

```bash
# 현재 폴더에 Git 저장소를 생성한다. 현재 폴더에는 [.git]이라는 폴더가 생성된다
# 이 폴더가 로컬저장소이다
$ git init

$ ls -a # [.git] 폴더 확인 가능

$ git status # 워킹 트리 상태 확인


```

- git status 명령어가 에러 없이 동작되는 경우도 있다

- 이 경우는 새로 만든 폴더가 git 프로젝트의 하위 폴더라는 의미

- 즉, 현재 폴더의 상위 폴더 중 어떤 폴더가 Git 저장소로 이미 초기화 되어 있다는 뜻

- 예를 들어 윈도우 환경에서, 내문서 - Documents 폴더가 git 저장소 인 경우

- 실수로 git push 명령어를 사용하면 내 문서 내의 자료가 통채로 github로 공개 될 수 있다

- 따라서 이러한 경우에는 이미 생성된 [.git] 폴더를 삭제한다

---

- Git으로 관리하는 파일의 4가지 상태
  - untracked
  - tracked
    - unmodified
    - modified
    - staged

1. 추적안됨(untracked)

   - 한번도 커밋되지 않은 파일

   - 워킹 디렉토리의 파일은 tracked와 untracked로 나눌 수 있다

   - tracked 는 git이 tracking 하고 있는 파일이고 untracked는 새로 만든 파일처럼 파일에 대한 정보가 아직 없고 git이 tracking 하지 않은 파일이다

2. 스테이지 됨(staged)

   - add 명령어를 통해 스테이지에 파일을 올린 상태

   - 로컬저장소(.git 폴더)의 stage에 파일이 올라간다

3. 수정없음(unmodified)

   - commit 명령어를 통해서 버전으로 만들면 파일 상태가 '스테이지됨(staged)'에서 '수정없음(modified)'로 변경된다

   - '수정없음(modified)' 상태인 파일은 수정할 수 없다

   - push 명령어를 통해서 원격 저장소에 올린다

4. 수정됨(modified)

   - 커밋된 파일을 수정한 경우 modified로 파일 상태가 변한다

   - git이 tracking 하고 있는 파일 중에서도 수정이 되었는지 유무에 따라 unmodified , modified로 나눌 수 있고 파일 상태가 'modified'인 파일은 변경사항이 없기 때문에 스테이지로 올릴 수 없다

   - 하지만 untracked 또는 modified 파일을 스테이지에 올리면 수정 없음 파일은 add 하지 않았지만 변경사항이 없기 때문에 같이 스테이지에 올라가게 된다.

   - 그리고 commit을 해서 새로운 버전을 만들면 git은 계산을 통해 앞 commit과 비교해서 어떠한 부분이 달라졌는지 알아낼 수 있다

```bash
# 모든 파일(a.txt, b.txt, c.txt)을 staging area에 추가
$ git add *

# a.txt라는 파일 삭제하고 git add * 하면 삭제된 a.txt라는 파일은 워킹 디렉토리에 없었기 때문에
# git staging area에 a라는 파일이 삭제된 상태가 추가 되지 않은 것을 확인할 수 있다
$ rm a.txt
$ git add *


# 하지만 아래와 같이 . 로 add 명령어를 수행하면 a.txt 파일이 삭제된 상태를 포함해서
# 모든 파일들이 staing area에 추가한다
$ git add .

# css파일만 추가하고 싶을 때
$ git add *.css

# staging area에 있는 파일들을 다시 unstage할 수 있다
$ git rm --cached "파일이름"

# state area에 있는 모든 파일을 unstage 한다
$ git rm --cached *


# git과 github에 추가하고 싶지 않은 파일들 git ignore에 추가한다

$ echo *.log > .gitignore

# 또한 아래처럼 파일을 열어서 아래와 같이 추가하고 싶지 않은 파일을 타이핑 할 수도 있다
# *.log
# build/
# build/*.log

# mac
open .gitignore

# window
stat .gitignore

```

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
