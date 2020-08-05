---
layout: post
title: 'git amend, cherry-pick, reset, revert, stash'
subtitle: 'git edits'
categories: development
tags: git
comments: true
---

- git amend, cherry-pick, reset, revert, stash에 대해서 공부하고 정리한 글입니다

---

### amend

- ammend 명령어를 사용하면 방금했던 커밋(원격 저장소에 푸시 했더라도)을 수정할 수 있다

- 커밋을 한 다음 커밋한 파일에서 수정해야할 부분이 있는 경우 우선 파일의 수정하고 싶은 부분을 수정한다

- 그리고 소스트리 기준 `Amend last commit (커밋옵션 → 마지막 커밋 정정)` 을 해주면 내가 지금 스테이지에 올린 변경사항이 기존 커밋에 추가되면서 기존 커밋이 덮어 씌워진다

- 이미 커밋을 원격 저장소에 푸시한 경우, 로컬저장소의 변경 사항을 원격 저장소에 강제로 덮어 씌우며 푸시하기 위해서 `[강제 푸시]`를 사용한다

- 강제푸시는 나 혼자만 쓰고 있는 브랜치에서만 해야한다

- 공용으로 사용하는 브랜치에서 강제 푸시를 하면 다른 사람의 커밋 히스토리가 엉망으로 꼬일 수 있다

- 이미 커밋을 원격 저장소에 푸시한 다음 다시 amend(마지막 커밋 정정)를 하면 로컬저장소의 커밋은 변경되지만 원격 저장소는 변경 사항이 반영되지 않기 때문에 master와 origin master가 각각 다른 브랜치로 존재한다

- 이 때 강제 푸시를 하게 되면 아래 사진과 같이 master와 origin master가 같은 브랜치로 존재하게 된다

<img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit1.png?raw=true">

---

### cherry-pick

- 다른 브랜치의 커밋 하나만 내 브랜치에 반영하는 방법

  - 아래 사진에서 feat/a의 두번째 커밋을 가져와 feat/b의 브랜치에 반영하고 싶을 때

  - feat/a 브랜치와 병합하면 첫번째 커밋과 세번째 커밋까지 브랜치에 반영이 되므로 이 상황을 피하고 싶을 때 체리픽을 사용한다

    <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit2.png?raw=true">

  - 체리픽이 성공하고 난 후에 feat/b의 두번째 커밋과 feat/a의 두번째 커밋은 커밋 ID가 서로 다르다는 것을 확인할 수 있다

  - 이를 통해서 변경사항을 복사해 왔지만 서로 같은 커밋은 아니라는 것을 알 수 있다

    <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit3.png?raw=true">

---

### reset

- 옛날 커밋으로 브랜치를 되돌리는 방법

- 앞의 feat/b 기능 추가 커밋을 기준으로, 해당 커밋까지 현재 브랜치를 초기화 하고 싶은 경우

* 소스트리 기준 되돌리고 싶은 `커밋 - [오른쪽 버튼 - 이 커밋까지 현재 브랜치를 초기화]` 한다(reset)

* reset하는 방법에는 3가지가 있다

* [Mixed] 모드

  - 작업 상태는 그대로 주지만 인덱스는 리셋

  - 아래 그림처럼 두번째 커밋 - 체리픽에서 추가했던 `cherrypick.md`변경 사항이 스테이지 아래로 튕겨 나온 것을 확인할 수 있다

  - 변경사항을 스테이지 아래로 두어서 다시 무엇을 스테이지 위로 Add할지 고민할 수 있다

     <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit4.png?raw=true">

* [Soft] 모드

  - 모든 로컬 변경 사항을 유지

  - `[mixed]`모드가 변경사항을 스테이지 아래두어서 무엇을 add할 지 정할 수 있다면, `[soft]`모드는 변경 사항을 스테이지 위로 둬서 변경사항을 스테이지 위로 두어서 다시 당장커밋할 수 있다

* [Hard] 모드

- 모든 작업 상태 내 변경 사항을 버린다. 지금 작업 공간에 상관없이, 예를 들어 몇개의 커밋을 추가했던지 간에 상관없이 깔끔하게 히스토리를 되돌린다

  - 두번째 커밋 까지 hard 모드로 reset

    <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit5.png?raw=true">

  - feat/b 기능 추가 커밋 까지 hard 모드로 reset

    <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit6.png?raw=true">

  - feat/b 브랜치는 과거로 돌아간 것을 확인할 수 있지만 아직 원격 브랜치인 origin/feat/b 에는 두번째 커밋이 남아 있다

  - reset한 다음 원격 브랜치에도 반영하기 위해서는 푸시해야 한다

  - 다만 히스토리를 수정하는 푸시이기 때문에 `[강제 푸시]`를 해야한다

  - 강제 푸시는 나만 쓰는 브랜치에서 해야 한다.

  - 다른 사람이 함께 사용하고 있는 곳에서 사용하게 되면 히스토리가 꼬이게 된다

---

### revert

- reset은 커밋을 마치 없었던 것 처럼 되돌리는 방법이랄면, revert는 이 커밋의 변경사항을 되돌리는 새로운 커밋을 만든다

* 하지만 모두가 함께 쓰는 브랜치이기 때문에 이력관리가 중요하다면 변경사항을 되돌리는 새로운 커밋을 만드는 것이 더 좋다

- 아래 그림처럼 `featb 기능 추가` 커밋 이후에
  `사이트 제목삭제`라는 커밋이 추가되었지만, 이 커밋의 이전인 `featb 기능 추가` 커밋으로
  되돌아 가고 싶은 경우 되돌리고 싶은 커밋 - `[마우스 오른쪽 버튼 - 커밋 되돌리기]`을 사용한다 (revert)

     <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit7.png?raw=true">

* 이 Revert 커밋은 방금한 커밋만이 아니라 이전에 한 커밋도 얼마든지 되돌릴 수 있다

* `'사이트 제목 삭제'`를 명시적으로 되돌리는 커밋인 `'Revert "사이트 제목 삭제"'` 커밋이 아래 처럼 만들어 졌다

   <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit8.png?raw=true">

---

### stash

- 변경 사항을 잠시 다른 곳에 저장하지만 커밋은 안 만들 때 사용한다

- 특정 브랜치에서 개발 도중 다른 브랜치로 이동을하려는데 현재 브랜치에 아직 커밋하지 않은 변경사항이 있지만 아직 이 파일들이 커밋을 하기에는 애매할 때 변경사항을 잠시 서랍속에 넣어두었다가 다시 꺼내쓰는 방법이다

- stach는 `tracked(추적중 - 한번이라도 Git에 올렸던 상태)`인 파일들만 들어간다

- 새로 만든 파일들은 untracked 상태니까 들어가지 않는다

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
