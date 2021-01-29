---
layout: post
title: 'git rebase 명령어 정리'
subtitle: 'git rebase'
categories: development
tags: git
comments: true
---

- git rebase에 대해서 정리한 글입니다

---

### rebase

- rebase를 사용할 수 있는 상황으로는 다음과 같은 상황이 있다

- 풀 리퀘스트를 보냈을 때 충돌이 났을 때 해결 하는 방법

- 현재 커밋과 병합하고 싶은 커밋을 미리 내 브랜치에서 병합해서 병합 커밋을 만들고 이를 풀 리퀘스트로 보낸다

- 다음과 같이 B 원격 저장소의 1번 커밋(upstream)과 A 원본 저장소의 2번 커밋(master)을 합치려고 할 때 충돌이 나는 경우

- upstream은 원본저장소를 지칭하는 관용적 닉네임이다

 <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-rebase1.png?raw=true">

- 커밋1과 커밋2의 충돌을 해결해서 만들어진 커밋3(병합커밋)은 커밋 2와 문제 없이 병합할 수 있다

- 하지만 나의 풀 리퀘스트에 불필요한 병합커밋(3번 커밋)의 이력이 남아 있다

- 이렇게 나의 풀 리퀘스트에 불필요한 병합커밋(3번 커밋)의 이력이 남는 것을 해결하는 방법은 묵은 커밋을 방금 한 커밋처럼 이력을 조작하는 것이다

- 이 때 사용할 수 있는 것이 rebase이다

---

- 또 다른 상황으로는 다음과 같이 3-way merge가 발생하는 상황이 있다

- 3-way merge를 하면 병합 커밋이 생성되기 때문에 트리가 다소 지저분해진다는 단점이 있다

- 아래 그림을 보면 0번 커밋에서 브랜치를 만들고 커밋을 만들었고 0번 커밋에도 새로운 커밋이 추가되어 있다

 <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-rebase2.png?raw=true">

- 따라서 master 브랜치와 feature A 브랜치를 merge 하려면 충돌이 발생한다

- 그런데 featue A 브랜치의 커밋의 베이스르 1번 커밋으로 만든다면 즉, featue A 브랜치를 master 브랜치의 최신 버전으로 만든다면 아무 문제 없이 Fast-forward Merge가 된다

- 이렇게 커밋의 베이스를 똑 떼서 다른 곳으로 붙여서 다시 베이스를 잡는 것을 `리베이스(rebase)`라고 한다

 <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-rebase3.png?raw=true">

- rebase의 원리는 다음과 같다

  1. HEAD와 대상 브랜치의 공통 조상을 찾는다(아래의 C2)

  2. 공통 조상 이후에 생성한 커밋들 (C4, C5 커밋)을 대상 브랜치의 뒤로 재배치한다

- 이렇게 rebase를 하면 병합 커밋을 만들지 않고 깔끔하게 브랜치를 합칠 수 있다

- 간단히 정리하면 한 달 전의 코드를 기준으로 만들었던 브랜치를, 마치 최신 코드를 기준으로 만든 것 처럼 이력을 조작하는 것이다.

- 즉, rebase는 최신 커밋을 베이스로 새롭게 이력을 조작하는 것이다

- 만약 충돌이 있다면 이력을 조작하는 중간에 고쳐주면 된다

- 1번 커밋을 베이스로 삼고 rebase를 하면 아래와 결과를 얻게 된다

- 중간에 충돌이 일어나도 해결한 다음 소스트리 기준 `[액션 - 재배치 계속]`을 클릭해서 리베이스를 계속 진행한다

- 리베이스는 커밋을 하나씩 비교하면서 충돌이 있는지 확인하기 때문에 계속 같은 곳을 수정했다면 `[재배치 계속]`을 누를 때 마다 충돌이 여러번 날 수 있다

* 이후 푸시를 해서 로컬 저장소의 이력을 원격저장소에 반영해야 한다

* 하지만 리베이스는 이력을 조작하기 때문에 일반 푸시로는 수행할 수 없다

* 따라서 리베이스를 사용하면 `강제 푸시`를 사용해서 원격 저장소에 반영해야 한다. 그리고 이 때 다른 개발자가 이 변경사항을 사용하고 있지 않아야 한다

* 이렇게 이력을 조작하고 푸시하는 것은 다른 개발자에게 위험한 행위기 때문에 무조건 혼자 사용하는 브랜치에서만 사용해야 한다

- 즉, 나 혼자만 feture A 브랜치를 사용하는 경우에는 상관없지만 다른 개발자와 함께 featuer A에서 작업을 하고 있다면 rebase를 하는 것이 위험한데 아래 예시를 통해 확인할 수 있다

```bash

# rebase 전

# feature 브랜치는 HEAD이므로 `*`이 붙어 있다
C1 <-- C2 <--  C3 (master)
        ↑
        -----C4 <-- C5(*feature A)

# rebase 후
C1 <-- C2 <-- C3(master)
                  ↑
                  -----C4' <-- C5'(*feature A)
```

- 위의 예시는 rebase를 하기 전과 후의 상황을 보여준다

- git rebase master 명령을 수행하면 공통 조상인 C2 이후 커밋인 C4와 C5를 master 브랜치의 최신 커밋인 C3 뒤쪽으로 재배치를 수행한다

- 그림에서 볼 수 있듯이 재배치된 C4, C5 커밋은 C4', C5' 커밋으로 표현되는데 그 이유는 커밋은 원래의 커밋과 다른 커밋이기 때문이다

- rebase 전과 후에 커밋 체크섬을 확인해보면 값이 달라진 것을 확인할 수 있다

- 겉으로는 똑같아 보이지만 실제로는 다른 커밋이 rebase 후에 생긴다

- 따라서 만약 다른 개발자가 동일한 branch에서 작업을 하고 있는 경우 내가 rebase하고 push 해서 변경된 정보를
  업데이트 하게 되면 다른 개발자가 가지고 있는 feature A의 C4, C5과 전혀 다른 커밋이기 때문에 merge conflict가 발생할 수 있다

- 따라서 다른 개발자와 함께 branch위에서 작업을 하고 있고 이미 history가 서버에 업로드 되어있는 경우라면 업로드된 history는 절대 rebase하면 안된다

- 나 혼자만 사용하는 brancn 또는 서버에 업데이트 되지 않는 나의 로컬에 있는 commit에 대해서는 자유롭게 rebase해도 된다

* 보통 기능 구현을 하는 브랜치는 혼자만 쓰니까 편하게 rebase 해도 된다

- 여러 git 가이드에서 원격 저장소에 존재하는 브랜치에 대해서는 rebase를 하지 말 것을 권장한다

```bash
$ git checkout feature1
# rebase하고 싶은 branch로 이동

$ git rebase master
# HEAD 브랜치의 커밋을 master로 재배치

# 충돌로 인해 rebase는 실패한다. 충돌을 수정한다

$ git status
# 충돌 대상 확인 및 수동으로 충돌해결

$ git add file1.txt
# 변경 사항 스테이징 및 상태 확인

$ git rebase --continue
# 리베이스 계속 진행
# merge는 마지막 단계에서 git commit 명령을 사용하지만
# rebaes는 git rebase --continue 명령을 사용한다

$ git log --oneline --all --graph -n2

$ git checkout master

$ git merge feature1 # 한 줄이 되었기 때문에 빨리감기 병합을 한다

```

---

### rebase --onto

- `rebase --onto` 옵션은 여러가지 브랜치를 만들고 그 브랜치 위에 또 다른 브랜치를 만드는 방식으로 체이닝해서 브랜치를 만드는 경우에 유용하게 사용할 수 있다

- 아래 그림은 master 브랜치에서 service를 위한 service 브랜치를 만든다음 service 를 사용하면서 필요한 UI를 위한 UI 브랜치를 만든 경우이다

 <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-rebase4.png?raw=true">

- 이러한 상황에서 만약 service 브랜치 더 이상 필요하지 않고 UI 브랜치만 필요해서 masger 브랜치에 merge하고 싶은 경우 `rebase --onto` 옵션을 사용할 수 있다

```bash

$ git rebase --onto master service UI

# service 브랜치에서 파생된 UI 브랜치만 master 브랜치에 rebase한다
# master 브랜치에서 명령어 실행

```

 <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-rebase5.png?raw=true">

```bash

$ git checkout master
# UI 브랜치를 master 브랜치로 rebase 했기 때문에 fast-forward가능하다

$ git merge UI
```

---

### interactive rebasing

- 기존에 존재하는 history를 간편하게 변경하는 방법

- 기존의 history를 변경하는 것이기 때문에 서버에 업로드 된 경우 말고 혼자서 작업하는 경우에 유용하게 사용할 수 있는 방법이다

- [`git ammend`](https://ibtg.github.io/development/2020/08/06/git-amend/) 명령어를 사용하면 최신 커밋의 타이틀이나 수정 사항을 업데이트 할 수 있었다

- 이 때 최신 커밋이 아니라 예전 커밋을 업데이트 하고 싶은 경우 `interative rebasing`을 사용할 수 있다

- 예를들어 아래와 같은 커밋에서 C3 에서 rebasing 한다면 rebasing 하는 순간 , C4부터 C3 하나만 수정하더라도 C3 이후의 모든 커밋들이 업데이트 되어야 하기 때문에 C3를 포함해서 C3 이후의 커밋들이 모든 새로운 커밋이 되면서 history가 업데이트 된다

```bash

# interative rebasing 전

C1 <-- C2 <-- C3 <--C4 <-- C5 <-- C6 <-- C7

# interative rebasing 후
C1 <-- C2 <-- C3* <--C4* <-- C5* <-- C6* <-- C7*

```

- 그리고 C3 커밋의 커밋 메시지를 다른 커밋 메시지로 변경하고 싶은 경우 C3 커밋이 가리키는 C3 이전의 해쉬코드 부터 시작해야 한다

- 즉 rebase를 할 때 C2 커밋의 해쉬코드를 사용한다

```bash
$ git rebase -i "C2 commit hashcode"
# 텍스트 에디터에서 C2 이후 모든 커밋과 commands 확인할 수 있다

# p, pick : 커밋을 사용하라는 것
# r, reword : 커밋을 사용하지만 메시지는 변경한다
# e, edit : 커밋을 사용하지만 안의 변경사항을 바꾸겠다
# s, squash : merge 를 통해 커밋을 하나로 묶어주는 것처럼 여러 commit을 하나로 묶어준다
# f, fixup : sqush와 비슷하지만 메시지를 남기지 않는 것
# e, exec : 이 커밋부터 shell 명령어를 직접적으로 이용하고 싶을 때 사용한다
# b, break: stop here, 여기서 멈춘다
# d, drop: 해당하는 커밋을 history에 남기지 않고 제거하고 싶을 때


# 위 명령어를 사용해서 C3의 커밋메시지를 변경할 수 있다
pick "C3 commit hashcode" "커밋 메시지"

# 이전의 pick 대신 사용한다 r(reword)를 사용한다
r "C3 commit hashcode" "커밋 메시지"

# 다시 커밋메시지를 수정할 수 있는 창이 생긴다.
# 여기서 새로운 커밋메시지로 수정하고 저장하면 rebase가 끝나있다

$ git log
# 변경된 것 확인

```

---

- 3-way병합은 기존 커밋의 변경 없이 새로운 병합 커밋을 하나 생성한다

- 따라서 충돌도 한 번만 발생한다

- 충돌 수정 완료 후 git commit 명령을 수행하면 merge 작업이 완료된다

- 그러나 rebase는 재배치 대상 커밋이 여러 개일 경우 여러 번 충돌이 발생할 수 있다

- 또한 기존의 커밋을 하나씩 단계별로 수정하기 때문에 git rebase —continue 명령으로 충돌로 인해 중단된 rebase를 재개하게 된다

- 여러 커밋에 충돌이 발생했다면 충돌을 해결할 때 마다 git rebase —continue 명령을 매번 입력해야 한다

- 3-way 병합

  - 특징 : 머지 커밋 생성

  - 장점 : 한번만 충돌 발생

  - 단점 : 트리가 약간 지저분 해짐

- rebase

  - 특징 : 현재 커밋들을 수정하면서 대상 브랜치 위로 재배치함

  - 장점 : 깔끔한 히스토리

  - 단점 : 여러번 충돌이 발생할 수 있음

---

<img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-rebase6.png?raw=true">

- 위의 예시처럼 master 브랜치만 사용했는데도 뻗어나오는 가지가 생기는 경우가 있다

- 보통 이런 상황은 두 대의 PC에서 한 브랜치에 작업을 하는 경우 생긴다

- 한 PC에서 커밋을 생성하고 push를 했는데 다른 PC에서 pull을 하지 않고 커밋을 하게 되면 이전 커밋을 부모로한 커밋이 생긴다.

- 그 상황에서 뒤늦게 pull을 하면 자동으로 3-way 병합이 되기 때문에 위 그림처럼 모양이 생기게 된다

- 불필요하게 병합 커밋이 생긴 상황이므로, 이를 해결하는 방법으로는 reset —hard로 병합 커밋을 되돌리고 rebase를 사용하는 것이다

- 우선 아래처럼 상황을 만들어 준다

```bash
# 보통 커밋 만들기

$ echo "master1" > master1.txt

$ git add master1.txt

$ git commit -m "master 커밋 1"

$ git push origin master

$ git log --oneline -n1

$ ls # 작업 디렉토리 상태 확인
```

```bash
# 가지 커밋 만들기

$ git reset --hard HEAD~ # HEAD를 한 단계 되돌리기

$ echo "master2" > master2.txt

$ git add .
# 스테이지에 추가
# . 은 모든 것(all), 즉 working directory 전체를 의미하는데 all은 권장하지 않는다
# 중요한 설정 파일까지 add할 필요가 없기 때문이다

$ git commit -m "master 2 커밋"

$ git log --oneline --graph --all -n3
# 로그를 확인해보면 master1 커밋과 master2 커밋 모두 같은 커밋을 부모로 가지기 때문에
# 가지가 생긴 것을 확인할 수 있다

$ git pull # 충돌 생긴다
# pull은 fetch + merge이므로 가지를 병합하기 위해 병합커밋이 생기고 히스토리가 지저분해진다

$ git log --oneline --graph --all -n4 # 병합 커밋 생성 확인

# 병합커밋이 생기는 빈도는 그리 놓지 않다
# 따라서 pull을 사용하고 그 때 병합 커밋이 생성되면
# hard reset을 이용해 되돌리고 rebase를 하면 된다
```

```bash
# rebase로 가지 없애기

$ git reset --hard -HEAD~
# 마지막 커밋은 병합 커밋이므로 병합되기 전 커밋으로 돌아간다
# 이 커밋은 튀어나온 가지 커밋이므로 재배치 해야 한다

$ git rebase origin/master
# 이 명령어를 수행하면 로컬 amster 브랜치의 가지 커밋이
# origin/master 브랜치 위로 재배치 된다

$ git log --oneline --all --graph -n3

$ git push
```

---

- rebase를 사용할 때, 원격 저장소에 푸시한 브랜치는 rebase하지 않는 것이 원칙이다

- 예를 들어 C1 커밋을 원격에 푸시하고 rebase를 하게 되면 원격에서 C1이 존재하고 로컬에는 다른 커밋인 C1'이 생성된다

- 이 때 내가 아닌 다른 사용자는 원격에 있던 C1을 병합할 수 있다

- 그런데 변경된 C1'도 언젠가는 푸시되고 그럼 원격에는 같은 커밋이었던 C1과 C1'이 동시에 존재하게 된다

- 이 상황에서 또 누군가는 충돌을 해결하기 위해 merge와 rebase를 사용하게 되는데 이 경우 동일한 커밋의 사본도 여러개 존재할 뿐아니라 충돌도 발생하고 히스토리가 꼬이는 상황이 생긴다

- 따라서 rebase와 git의 동작 원리를 이해하기 전까지는 가급적 rebase는 아직 원격에 존재하지 않는 로컬 브랜치들에만 적용한다

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
