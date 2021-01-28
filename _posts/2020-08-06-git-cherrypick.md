---
layout: post
title: 'git cherry-pick 명령어 정리'
subtitle: 'git edits'
categories: development
tags: git
comments: true
---

- git cherry-pick에 대해서 공부하고 정리한 글입니다

---

### cherry-pick

- 다른 브랜치의 커밋 하나만 내 브랜치에 반영하는 방법

  - 아래 사진에서 feat/a의 두번째 커밋을 가져와 feat/b의 브랜치에 반영하고 싶을 때

  - feat/a 브랜치와 병합하면 첫번째 커밋과 세번째 커밋까지 브랜치에 반영이 되므로 이 상황을 피하고 싶을 때 체리픽을 사용한다

    <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit2.png?raw=true">

  - 체리픽이 성공하고 난 후에 feat/b의 두번째 커밋과 feat/a의 두번째 커밋은 커밋 ID가 서로 다르다는 것을 확인할 수 있다

  - 이를 통해서 변경사항을 복사해 왔지만 서로 같은 커밋은 아니라는 것을 알 수 있다

    <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit3.png?raw=true">

  ```bash
  $ git cherry-pick "커밋 해쉬코드"
  #  커밋을 가져와서 추가할 브랜치인 feat/a 브랜치 위에서 실행

  ```

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
