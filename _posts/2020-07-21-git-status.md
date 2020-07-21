---
layout: post
title: 'Git의 여러가지 상태(status)'
subtitle: 'git status'
categories: development
tags: git
comments: true
---

- Git의 상태(status)에 대해서 공부하고 정리한 글입니다

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

- Git으로 관리하는 파일의 4가지 상태
  - untracked
  - tracked
    - unmodified
    - modified
    - staged

1. 추적안됨(untracked)

   - 한번도 커밋되지 않은 파일

2. 스테이지 됨(staged)

   - add 명령어를 통해 스테이지에 파일을 올린 상태

   - 로컬저장소(.git 폴더)의 stage에 파일이 올라간다

3. 수정없음(unmodified)

   - commit 명령어를 통해서 버전으로 만들면 파일 상태가 '스테이지됨(staged)'에서 '수정없음(modified)'로 변경된다

   - '수정없음(modified)' 상태인 파일은 수정할 수 없다

   - push 명령어를 통해서 원격 저장소에 올린다

4. 수정됨(modified)

   - 커밋된 파일을 수정한 경우 modified로 파일 상태가 변한다

   - 파일 상태가 '수정 없음'인 파일은 변경사항이 없기 때문에 스테이지로 올릴 수 없다

   - 하지만 untracked 또는 modified 파일을 스테이지에 올리면 수정 없음 파일은 add 하지 않았지만 변경사항이 없기 때문에 같이 스테이지에 올라가게 된다.

   - 그리고 commit을 해서 새로운 버전을 만들면 git은 계산을 통해 앞 commit과 비교해서 어떠한 부분이 달라졌는지 알아낼 수 있다

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
