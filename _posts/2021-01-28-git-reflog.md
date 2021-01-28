---
layout: post
title: 'git reflog 명령어 정리'
subtitle: 'git reflog'
categories: development
tags: git
comments: true
---

- git reflog에 대해서 공부하고 정리한 글입니다

---

### reflog

- `reset` 명령어를 사용해서 git history를 수정한 다음, 다시 이전으로 돌아가고 싶을 때 `git reflog` 명령어를 사용한다

- `reflog`는 `reference log`, log를 참조한다는 것

- 바로 이전에 HEAD가 가리키고 있던 내용들을 기억함으로써, 원하는 시점으로 돌아갈 수 있게 해준다

- 즉, 내가 실수하더라도 다시 예전 상태로 돌아갈 수 있다

- 단, 이전에 커밋을 한 상태에서만 가능하다

- 만약 커밋을 하지 않은 상태에서 내가 로컬에 작성한 파일을 `git rset --hard` 명령어를 사용해서 초기화했다면 다시 돌아갈 확률이 적다.

```bash
$ git reflog
# 지금까지 실행했던 명령어들과
# 명령어로 인해 변경된 HEAD가 가리키고 있던 커밋 확인할 수 있다

$ git reset --hard "해쉬 코드"
# HEAD가 다시 해쉬코드를 가리키게 된다

$ git reflog
# reset으로 HEAD가 변경되었기 때문에, 변경된 내용을 확인할 수 있다

```
