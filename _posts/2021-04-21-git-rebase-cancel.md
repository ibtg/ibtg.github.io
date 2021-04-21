---
layout: post
title: 'git rebase를 취소하는 방법'
subtitle: 'git rebase cancel'
categories: development
tags: git
comments: true
---

- git의 rebase를 취소하는 방법에 관한 글입니다

---


### git rebase 취소하기

- `git rebase` 명령어는 이전의 커밋을 최신 커밋처럼 사용할 수 있다는 장점이 있다

- 하지만 잘못 `rebase` 를 해서 다시 `rebase` 전으로 되돌아가고 싶은 경우, 이 때 `git reflog`와 `git reset`명령어를 사용해서 `rebase` 하기 전 커밋으로 돌아갈 수 있다

- [rebase](https://dkmqflx.github.io/development/2021/01/28/git-reflog/) 명령어는 이전 커밋을 확인할 수 있는 명령어로, 우선 되돌아가고 싶은 커밋을 확인하기 위해 `git reflog` 명령어를 수행한다

  ```bash
  $ git reflog

  ```

- 명령어를 수행하면 다음과 같은 결과가 출력된다 

  ```bash
  aafd16c HEAD@{1}: commit: commit 1
  b714abf HEAD@{2}: commit: commit 2
  198ccde HEAD@{3}: commit: commit 3
  ec591c6 HEAD@{4}: commit: commit 4
  ```

- 만약 `commit 3`으로 돌아가고 싶다면 `reset` 명령어를 `--hard` 옵션과 사용해서 해당 커밋으로 이동한다

  ```bash
    $ git reset --hard 198ccde

  ```

- 그리고 `rebase` 한 결과를 remote repository에도 push 한 경우에는 강제 push를 해서 remote repository에도 번경 사항을 반영시켜준다

  ```bash
    $ git push -f origin "remote branch"

  ```

---

## Reference

- [How to undo a mistaken git rebase (LIFE SAVER)](https://medium.com/@shreyaWhiz/how-to-undo-a-mistaken-git-rebase-life-saver-2977ff0a0602)
