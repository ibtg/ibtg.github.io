---
layout: post
title: 'git config를 사용한 옵션 설정'
subtitle: 'git config'
categories: development
tags: git
comments: true
---

- git config에 대해서 공부하고 정리한 글입니다

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
