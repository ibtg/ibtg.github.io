---
layout: post
title: 'git config 명령어 정리'
subtitle: 'git config'
categories: development
tags: git
comments: true
---

- git config에 대해서 공부하고 정리한 글입니다

---

- git에 관련된 모든 환경설정이 .gitconfig라는 파일 안에 저장된다

```bash
$ git config --list
# 모든 설정 확인 가능


$ git config --global -e
# 파일로 열어보고 싶은 경우, 터미널로 확인가능
# 텍스트 에디터 연결되어 있으면 텍스트 에디터에서 확인가능

$ code .
# 텍스트 에디터가 연결되어 있다면 연결된 텍스트 에디터를 바로 열 수 있다
```

- code를 에디터와 연동해서 사용하는 방법은 `command + shift + p`를 눌러 `Install 'code' command IN PATH` 를 선택해준다

```bash

$ git config --global core.editor "code"
# .gitconfig 파일이 열리는 동시에 
# 다른 명령어를 수행할 수 있도록 터미널이 계속해서 활성화된다


$ git config --global core.editor "code --wait"
# 하지만 다음과 같이 설정해주면 파일이 열린 동안에는
# 다른 명령어를 수행할 수 없도록 터미널이 wait 된다


# 다음과 같은 설정을 추가해준다
[user]
	email = "userId@email.com"
	name = "userName"
[push]
	default = current
[pull]
	rebase = true

# push를 할때 default를 current으로 설정해서 로컬에 있는 브랜치 이름이 항상 리무트와 동일하다고 간주한다

# 따라서 push를 할때 일일이 'git push --set-upstream origin master' 옵션을 작성하지 않아도 된다


# pull 명령어는 merge와 rebase 옵션을 선택해서 동작할 수 있는데 우리는 rebase를 이용한다
```

- 사용자에 관련된 정보 설정

```bash
$ git config --global user.name "userName"
$ git config --global user.email "userId@email.com"
```

- `git config` 명령어를 사용해서 설정한 속성을 확인할 수 있다

```bash
$ git config user.name
$ git config user.email

git config --list
# 모든 설정을 텍스트에디터에서 확인할 수 있다

```

- repository 마다 다른 `user.name`과 `user.email`을 설정하고 싶은 경우 `--local` 옵션을 사용한다

```bash
git config --local user.name "userName"
git config --local user.email "userId@email.com"

```


- `git config` 명령어를 사용해서 단축키 설정을 할 수도 있다

```bash

$ git config --global -e
# 다음과 같은 명령어를 사용하면 터미널에서 설정을 확인할 수 있다
# 텍스트에디터 열고 다음과 같이 설정해준다

[alias]

s = status

co = checkout
ca = !git add -A && git commit -m
cad = !git add -A && git commit -m "."

hist = log --graph --all --pretty=format:'%C(yellow)[%ad]%C(reset) %C(green)[%h]%C(reset) | %C(white)%s %C(bold red){{%an}}%C(reset) %C(blue)%d%C(reset)' --date=short

c = commit
```

- alias를 설정하면 다음과 같이 단축키를 사용할 수 있다

```bash
$ git cad
# 빠르게 commit을 .(dot)을 이용해서 할 수 있다

$ git ca "커밋 메시지"
# 모든 파일을 staging area에 옮긴 다음에 커밋을 한다


$ gh
  # 다음과 같이 단축키를 사용할 수도 있다

$ gsa
# 다음과 같이 단축키를 사용할 수도 있다
$ gsl
# 다음과 같이 단축키를 사용할 수도 있다
$ gss
# 다음과 같이 단축키를 사용할 수도 있다

```

- 운영체제마다 에디터에서 줄바꾸말 때 들어가는 문자열이 달라진다

- 윈도우는 `carriage-return(\r)`과 `line feed(\n)`가 동시에 들어가는 반면에 mac에서는 `line feed(\n)` 하나만 들어가게 된다

- 예를들어 윈도우는 `text\r\n`, mac은 `text\n`

- 이러한 차이점 때문에 git repository를 다양한 운영체제에서 쓰는 경우에는 내가 수정하지 않았음에도 불구하고 줄바꿈 문자열이 달라저서 git history나 git blame을 보는데 문제가 있을 수 있다

- 이것을 수정할 수 있는 속성이 바로 autocrlf 설정

- 윈도우에서 true로 설정하게 되면 git에 저장할 때는 `carriage-return(\r)`을 삭제하게 되고, 다시 git에서 윈도우로 가져올 때는 자동으로 `carriage-return(\r)`을 붙쳐준다

- mac에서는 input으로 설정하게 되면 git에서 받아올 때는 별다른 수정이 일어나지 않지만 저장할 때는 `carriage-return(\r)`을 삭제한다

- mac에서는 `carriage-return(\r)`이 붙지 않음에도 불구하고 이렇게 처리를 해주는 것은 mac에서 이메일을
  받아온 텍스트를 복하새서 붙여넣을 때 실수로 `carriage-return(\r)`이 들어갈 수있기 때문이다

```bash
$ git config --global core.autocrlf
```

- `alias` 명령어를 사용하면 기존의 명령어를 줄여서 사용할 수 있다

```bash
# 아래와 같은 방법으로 git status를 git st로 줄여서 쓸 수 있다
$ git config --global alias.st status

# git 관련된 명령어 볼 수 있다
$ git config --h
```

---

- git 옵션에는 지역옵션, 전역옵션, 시스템 환경 옵션이 있다

- git config 명령을 사용해서 옵션을 보거나 값을 바꿀 수 있다

- 우선 순위 : 지역 옵션 > 전역 옵션 > 시스템 환경 옵션

- 일반적으로 개인 PC에서는 전역 옵션을 많이 하용하지만 공용 PC처럼 여러 사람이 사용하거나 git을 잠깐만 써야할 경우 지역 옵션을 사용한다

- 시스템 환경 옵션

  - PC 전체의 사용자를 위한 옵션

  - git이나 소스트리 설치시에 몇가지 값들이 지정되는 곳

- 전역 옵션

  - 현재 사용자를 위한 옵션

* 지역 옵션

  - 현재 Git 저장소에서만 유효한 옵션

  ```bash
  git config --global <옵션명>
  #지정한 전역 옵션의 내용을 살펴본다

  git config --global <옵션명> <새로운 값>
  #지정한 전역 옵션의 값을 새로 설정한다

  git config --global unset <옵션명>
  #지정한 전역 옵션을 삭제한다

  git config --local <옵션명>
  #지정한 지역 옵션의 내용을 살펴본다

  git config --local <옵션명> <새로운 값>
  #지정한 지역 옵션의 값을 새로 설정한다

  git config --local unset <옵션명>
  #지정한 지역 옵션을 삭제한다

  git config --system <옵션명>
  #지정한 시스템 옵션의 내용을 살펴본다

  git config --system <옵션명> <값>
  #지정한 시스템 옵션의 값을 새로 설정한다

  git config --system unset <옵션명> <값>
  #지정한 시스템 옵션의 값 삭제한다

  git config --list
  #현재 프로젝트의 모든 옵션을 살펴본다

  $ git config --global user.name
  #현재 user 확인

  $ git config --global user.name "new"
  #현재 이름을 new로 지정

  # 현재 작업 PC가 공용이거나 프로젝트마다 값을 따로 설정하고 싶은 경우
  # --global 대신 지역 옵션을 설정한다

  #git 기본 에디터 확인
  $ git config core.edigot

  $ git config --global core.editor

  $ git config --system core.editor
  ```

  ---

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
- [git docs - config](https://git-scm.com/docs/git-config)
