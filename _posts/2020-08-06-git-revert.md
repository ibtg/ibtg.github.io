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

- reset은 커밋을 마치 없었던 것 처럼 되돌리는 방법이라면, revert는 이 커밋의 변경사항을 되돌리는 새로운 커밋을 만든다

* 하지만 모두가 함께 쓰는 브랜치이기 때문에 이력관리가 중요하다면 변경사항을 되돌리는 새로운 커밋을 만드는 것이 더 좋다

- 아래 그림처럼 `featb 기능 추가` 커밋 이후에
  `사이트 제목삭제`라는 커밋이 추가되었지만, 이 커밋의 이전인 `featb 기능 추가` 커밋으로
  되돌아 가고 싶은 경우 되돌리고 싶은 커밋 - `[마우스 오른쪽 버튼 - 커밋 되돌리기]`을 사용한다 (revert)

     <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit7.png?raw=true">

* 이 Revert 커밋은 방금한 커밋만이 아니라 이전에 한 커밋도 얼마든지 되돌릴 수 있다

* `'사이트 제목 삭제'`를 명시적으로 되돌리는 커밋인 `'Revert "사이트 제목 삭제"'` 커밋이 아래 처럼 만들어 졌다

   <img src="https://github.com/ibtg/ibtg.github.io/blob/master/assets/img/post_img/2020-08-06-git-commit-edit8.png?raw=true">

- revert를 사용해서 커밋을 되돌려야 하는 경우

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

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
