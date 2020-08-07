---
layout: post
title: 'git branch, merge '
subtitle: 'git branch merge'
categories: development
tags: git
comments: true
---

- Git의 branch와 merge에 대해서 공부하고 정리한 글입니다

---

### branch

- 커밋은 줄줄이 기차처럼 연결되어 있고 새로 만든 커밋은 기존 커밋 다음에 시간순으로 쌓인다.

- 특정 기준에서 줄기를 나누어 작업할 수 있는 기능을 브랜치(Branch)라고 한다

- 새로운 가지로 커밋을 만들려면 반드시 브랜치를 먼저 만들어야 한다

<img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge1.png?raw=true">

---

- `[master]`는 Git이 제공하는 기본적인 브랜치

- 첫 번째 커밋을 하면 자동으로 `'[master]'`라는 이름의 브랜치가 커밋을 가리키게 된다

- '브랜치가 커밋을 가리키게 된다' 라는 표현을 쓰는 이유는 브랜치는 단순한 포인터(Pointer)이기 때문이다.

- 따라서 아래와 같이`[master]` 브랜치에서 새로운 커밋을 올리면, `master`브랜치가 최신 커밋을 가리키게 된다

<img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge2.png?raw=true">

- 아래 그림처럼 `feature`이라는 새로운 브랜치를 만들게 되면 `feature` 브랜치는 `master` 브랜치와 동일한 `커밋3`를 가리키게 된다

- 브랜치가 포인트라는 점 때문에 커밋을 가리키는 것 만으로도 새로운 분기를 만들 수 있다는 장점이 있다

<img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge3.png?raw=true">

---

<img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge4.png?raw=true">

- 브랜치는 포인터 이기 때문에 커밋을 가리키는 것만으로도 분기를 만들 수 있다

- `A branch`에 커밋을 하나 더 하면 `master` 브랜치 보다 커밋 하나만큼 앞서게 된다

- 이 때 `A branch`와 `master` 브랜치 사이를 `[HEAD]`라는 특수한 포인터를 사용해서 넘나들 수 있다

- `[HEAD]`는 브랜치 혹은 커밋을 가리키는 포인터이다.

- `[HEAD]` 포인터가 `[master]` 브랜치를 가르키면 커밋2의 상태 보여주고 `A branch`를 가리키면 커밋3의 상태를 보여준다

- 브랜치의 최신 커밋이 아닌 과거 커밋으로도 `[HEAD]`를 이동시킬 수 있는데 이 경우에는 `[master]`브랜치의 포인터와 `[HEAD]`가 떨어져 있기에 `'분리된 HEAD(Detached HEAD)'` 상태가 된다

---

- 여러 버전이 꼬이는 것을 방지하기 위해서 하나의 브랜치에는 한 사람만 작업해서 올리는 것이 바람직하다.

  1. `[master]` 브랜치에는 직접 커밋을 올리지 않는다 (동시에 작업하다 꼬일 수 있다)

  2. 기능 개발을 하기 전에 `[master]`브랜치 기준으로 새로운 브랜치를 만든다

  3. 이 브랜치의 이름은 `[feature/기능이름]` 형식으로 하고 한 명만 커밋을 올린다

  4. `[feature/기능이름]` 브랜치에서 기능 개발이 끝나면 `[master]` 브랜치에 이를 합친다

---

### Merge

- 병합(merge)는 간단히 말해서 두 버전의 합집합을 구하는 것이다

1. Merge commit(병합 커밋) : +A U A* = +A*

2. Fast-forward(빨리 감기) : A U B = B

   - 합친 결과물이 B와 동일하기 때문에 새로 상태를 만들어 줄 필요 없이 B로 상태를 바꿔주면 된다

3. Conflict(충돌) : A+ U A* = A+? A*?
   - 무엇을 기준으로 합쳐야할지 모르기 때문에 충돌이 난 부분만 확인하고 무엇을 남길지 수동으로 선택해 주면 된다

```bash

git merge 브랜치이름
# 지정한 브랜치의 커밋들을 현재 브랜치 및 워킹트리에 반영한다


```

---

<img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge5.png?raw=true">

- 커밋4는 커밋2를 단순하게 수정한 최신본이기 때문에 두 상태를 합치면 바뀌는 상태 없이 커밋 4가 되는 `Fast-forward`가 된다

- 따라서 `merge` 후에 `marster branch`와 `feature/detail-page` 브랜치는 커밋 4를 가리키게 된다

- 커밋5는 커밋 2를 중심으로 상태가 바뀌었기 때문에 커밋4에 있는 `marster branch`와 커밋5에 있는 `feature/cart` 를 합치게 되면 `merge commit`이 된다

- 이 때 상황에 따라, 새로운 `merge commit`을 `marster branch`에 둘지, `feature/cart`에 둘 지 선택한다.

- 즉, A와 B브랜치가 있을 때 두 개를 합쳤을 때 만들어진 AB브랜치를 A 브랜치에 올릴 건지, B 브랜치에 올릴 것인지 정하는 것이다.

- 만약 A 브랜치에 올린다면 AB브랜치가 A 브랜치에만 반영되고 B 브랜치에는 반영이 안된다

* 따라서 `[master] 브랜치`를 기준으로 병합한다는 것은 합친 결과물을 `[master]브랜치`에 반영한다는 것이다

<img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge6.png?raw=true">

---

- 두 커밋이 같은 코드를 수정했다면 병합 커밋을 만들다가 충돌이 날 가능성이 있다

- 그래서 동료들과 같이 쓰는 `[master] 브랜치`에 바로 병합하지 않고 나만 쓰는 `[feature/cart]`에서 먼저 병합해보고 문제가 없는지 확인한다

- 1단계 : `master 브랜치`를 땡겨와서 feature/cart 브랜치에서 병합한다

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge7.png?raw=true">

- 2단계: feature/cart 브랜치에서 병합한 커밋을 master 브랜치에 반영한다

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge8.png?raw=true">

---

### fetch

- 원격 저장소로부터 코드를 받아 병합하는 git pull 명령어와 달리, 그래프만 업데이트를 한다

- 따라서 git fetch 명령어를 사용하면 로컬 브랜치는 원래 상태에서 최근 커밋 위치를 가리키고,
  원격 저장소 origin/master는 fetch 명령어를 통해서 가져온 최신 커밋을 가리킨다.

- fetch 명령어를 사용해서 그래프를 불러와서 원격 저장소와 차이점을 파악한 다음
  git merge origin/master 명령어 통해서 Merge를 하게 되면 git pull 명령어를 쓸 때와 동일한 결과를 얻을 수 있다

```bash
git fetch [원격저장소별명] [브랜치이름]
# 원격 저장소의 브랜치와 커밋들을 로컬저장소와 동기화 한다.
# 옵션을 생략하면 모든 원격저장소에서 모든 브랜치를 가져온다

```

---

- 풀 리퀘스트 (Pull request)

  - 협력자에게 브랜치 병합을 요청하는 메세지를 보내는 것

- 베이스 브랜치(base brach) : 병합 결과물이 올라갈, 즉 기준이 되는 브랜치

- 비교 브랜치 (compare brach) : 기준 브랜치의 비교대상이 되는 브랜치. 내가 만들어서 base 브랜치에 반영시키고 싶은 브랜치

  <img src = "https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-02-git-branch-merge9.png?raw=true">

- Assigners : 이 풀 리퀘스트를 담당하는 동료. 보통 자기 자신

- Labels : 이 풀 리퀘스트에 관한 라벨을 달아준다. 예를들어 [프론트엔드], [백엔드]

- Reviewers : 협력자를 지정할 수 있다.

- base : 병합된 커밋이 들어갈 브랜치를 선택한다

- compare : 내가 만들어서 base 브랜치에 반영시키고 싶은 브랜치
- able to merge : 충돌없이 병합될 수 있다는 뜻

- Pull은 실제 코드를 내려 받는데 비해 fetch (패치)는 그래프만 업데이트 한다.

- 풀 리퀘스트는 코드의 라인마다 댓글을 달 수 있다.

- 따라서 해당 코드가 왜 고쳐졌는지, 혹은 어떻게 개선할 수 있는지 풀 리퀘스트 내부에서 토론을 진행할 수 있다

- 이 풀 리퀘스트에 대해서 수락(Accept), 수정 요청(Request), 병합(Merge pull request) 할 수있다

- 그림처럼 master 브랜치에 풀 리퀘스트를 하고 병합을 하면 새로운 병합 커밋이 생기고 master 브랜치가 그 커밋을 가르키게 된다

- 하지만 소스트리를 쓸 때 즉시 반영이 되지 않는 경우가 있는데 이때 [패치] 기능을 사용해서 Git에서 새로운 이력을 업데이트 한다

- Pull은 실제 코드를 내려 받는데 비해 패치는 그래프만 업데이트 한다

- [패치] 후 브랜치의 병합이 잘 된 것을 확인한 이후에 [master] 로 브랜치를 옮긴다음에 [pull]을 해서 로컬 저장소의 [master]와 원격 저장소의 [master]를 동일하게 만들어준다

---

### Release, Tag

- 버전을 올리는 것은 메이저 업그레이드와 마이너 업그레이드로 나뉜다

  - 메이저 업그레이드 : 사람들이 크게 느낄 변화를 적용(v2.x → v3.x)
  - 마이너 업그레이드 : 작은 변화 등이 있을 때 (v.2.3 → v.2.4)

- 릴리즈(release)

  - 프로그램을 출시하는 것

- 태그(Tag)
  - 새로운 프로그램을 출시하고 현재 코드 상태를 버전 v1.0.0이라고 기록할 때 이를 태그를 통해서 간단하게 표시할 수 있다
  - 브랜치 처럼 커밋을 가리키는 가벼운 포인터이다
  - 태그도 브랜치처럼 푸시를 해주어야 원격저장소에서도 볼 수 있다

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
