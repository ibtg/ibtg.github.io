---
layout: post
title: 'git revert 명령어 정리'
subtitle: 'git revert'
categories: development
tags: git
comments: true
---

- git revert 명령어에 대해서 공부하고 정리한 글입니다

---

### revert

- `reset` 이나 `restore` 명령어를 사용하면 예전 버전으로 돌아갈 수 있지만 history에는 예전으로 돌아갔다는 것이 남지 않는다.

- `reset` 이나 `restore` 명령어가 커밋을 마치 없었던 것 처럼 되돌리는 방법이라면, `revert`는 이 커밋의 변경사항을 되돌리는 새로운 커밋을 만든다

- 주로 master 브랜치에 문제가 있는 커밋이나 다시 되돌리고 싶은 커밋 중 history에 남기고 싶은 커밋에 대해서 `revert` 명령어를 사용 한다

- 그리고 이미 서버에 push 된 커밋인 경우 `reset` 또는 `rebase`를 사용학 보다는 `revert`를 사용한다

- `revert`는 새로운 커밋을 만들기 때문에 history를 수정하지 않기 때문이다

- `revert` 명령어를 사용하는 경우 `revert commit`에서 다른 기능을 추가하거나 버그를 고치거나 하는 작업은 하지 않는다

- 예를 들어 master 브랜치에 기능을 merge 한 다음에 릴리즈 하더라도 나중에 문제가 생기거나 하면 해당 커밋을 완전히 제거해야할 경우가 있는데 이 때 `revert`를 사용해서 이러한 문제를 해결할 수 있다

- 아래 Third Commit에서 문제가 생긴 경우 `revert` 명령어를 사용하면 변경한 모든 내용들이 삭제된 새로운 커밋이 생긴다

```bash
* Forth Commit (HEAD -> master)
* .
* Third Commit
* WIP
* Sceond Commit
* Initialise Commit
```

```bash

$ git revert "commit hashcode"
# git revert HEAD

$ git log
# 텍스트 에디터에 커밋 메시지 입력할 수 있다

```

- 명령어 수행해서 log를 확인해 보면 아래처럼 새로운 커밋이 생긴 것을 확인할 수 있다

```bash
* Third Commit revert commit (HEAD -> master)
* Forth Commit
* .
* Third Commit
* WIP
* Sceond Commit
* Initialise Commit
```

```bash
$ git show 커밋 "commit hashcode"
# 어떤 부분이 삭제되었는지 확인할 수 있다

```

- 아래처럼 커밋 사이에 있는 커밋을 되돌려야 하는 경우 revert를 사용할 수 있다

```bash

C1 <--- F1 <--- C2 <--- F2 <--- C3(master)
# F1과 F2를 취소하고 싶은 경우 revert를 사용해서 이를 해결 할 수 있다

$ git revert F2
$ git revert F1

C1 <--- F1 <--- C2 <--- F2 <--- C3 <--- RF2 <--- RF1(master)
# git revert를 실행할 때는 최신 커밋 부터 취소를 하는 것이 좋다
# 이렇게 하면 F2와 F!을 취소하는 커밋(RF2, RF1)을 각각 만들어 낸다
# 이전의 히스토리를 변경하지 않고도 깔끔하게 히스토리 중간의 여러 커밋 내용을
# 작업 이전 상태로 되돌릴 수 있으므로 현업에서도 유용하게 사용한다
```

- `revert` 명령어를 사용하더라도 커밋을 만들지 않을 수도 있다

- 아래처럼 `--no-commit` 옵션과 함께 사용하면 바로 커밋을 하지 않고 취소되는 변경사항을 staging area에 추가해 준다

```bash

$ git revert --no-commit "commit hashcode"

$ git status

```

- 소스트리

  - 모두가 함께 쓰는 브랜치이기에서 이력관리가 중요하다면 `revert` 명령어를 사용해서 변경사항을 되돌리는 새로운 커밋을 만드는 것이 더 좋다

  * 아래 그림처럼 `featb 기능 추가` 커밋 이후에 `사이트 제목삭제`라는 커밋이 추가되었지만, 이 커밋의 이전인 `featb 기능 추가` 커밋으로 되돌아 가고 싶은 경우 되돌리고 싶은 커밋 - `[마우스 오른쪽 버튼 - 커밋 되돌리기]`을 사용한다 (revert)

     <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit7.png?raw=true">

  - 이 `revert commit` 은 방금한 커밋만이 아니라 이전에 한 커밋도 얼마든지 되돌릴 수 있다

  - `'사이트 제목 삭제'`를 명시적으로 되돌리는 커밋인 `'Revert "사이트 제목 삭제"'` 커밋이 아래 처럼 만들어 졌다

     <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit8.png?raw=true">

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
