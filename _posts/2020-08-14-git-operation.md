---
layout: post
title: 'git의 동작원리'
subtitle: 'git operation'
categories: development
tags: git
comments: true
---

- git의 동작원리에 대해서 공부하고 정리한 글입니다

---

### git add 명령어의 동작원리

- git add 내부 동작원리를 알기위해 아래와 같이 git 로컬 저장소를 만든다

```bash


$ mkdir git-test

$ cd git-test

$ git init # git 로컬 저장소 생성

$ ls -al # .git 폴더 생성 확인

$ ls -al .git # .git 폴더 내부 확인

```

---

- ls -al 명령어에서 출력되는 결과는 다음과 같다
  - rw-r--r--1 : 파일의 권한과 상태. `-`로 시작하면 일반파일, `d`로 시작하면 폴더이다
  - cat-hanbit : 파일의 소유자 아이디
  - 23 : 파일의 크기
  - HEAD : 파일이름, 폴더의 경우 `/`가 붙는다

* 아래 명령어를 low-level 명령어라고 하는데, 이를 통해서 gitd의 동작 원리를 이해할 수 있다

* git hash-objet < 파일명>

  - 일반 파일의 체크섬을 확인할 때 사용한다

* git show <체크섬>

  - 해당 체크섬을 가진 객체의 내용을 표시한다

* git ls-files --stage
  - 스테이지 파일의 내용을 표시한다
  - 스테이지 파일은 git add 명령을 통해 생성되는데 .git/index 파일이 스테이지 파일이다

```bash

echo "cat-hanbit" > cat.txt # 파일 생성

$ git status
# git status 명령어는 워킹트리와 스테이지, 그리고 HEAD 커밋 세가지 저장 공간의 차이를 비교해서 보여준다
# 새로 파일을 생성할 경우 워킹트리에만 해당 파일이 존재한다

```

```bash

$ git add cat.txt # 스테이지에 unstracked 상태의 파일을 추가한다

$ git status

$ ls -a .git
# .git 폴더 확인, index가 생성된 것을 확인할 수 있다

$ file .git/index
# index 확인, Git index가 출력된다
# index는 스테이지의 다른이름으로 이 index 파일이 Git의 스테이지이다

$ git ls-files --stage
# 스테이지의 내용 확인
# cat.txt 파일이 스테이지에 들어있으며 체크섬은 위에서 확인한 파일의 체크섬과 일치하는 것을 확인할 수 있다

```

```bash

$ ls -a /git/objects
# ff로 시작하는 폴더가 새로 생긴 것을 확인할 수 있다

$ ls -a /git/objects/ff
# 폴더 아래 존재하는 5bda와 폴더명을 합친 ff5bda가 앞서 확인한 체크섬의 값과 같은 것을 알 수 있다
# objects 폴더 안에 존재하는 파일들을 Git 객체이다

$ git show ff5bda
# ff5bda 객체의 내용확인

```

- 체크섬을 이용해서 객체의 종류와 내용을 확인할 수 있는 다른 명령들도 있다

- git cat-file -t <체크섬>

  - 해당 체크섬을 가진 객체의 타입을 알려주는 명렁어

- git cat-file <객체타입> <체크섬>
  - 객체의 타입을 알고있을 때 해당 파일의 내용을 표시해 준다

```bash

$ git cat-file -t ff5ba
# 체크섬으로 객체의 종류 확인
# blob 은 binary large object의 줄ㅇㅁ말

$ git cat-file blob ff5ba
# 해당 객체의 ㅐ뇽확인

```

- git add 명령은 워킹트리에 존재하는 파일을 stage에 추가하는 명령어이다

- 이 때 해당 파이릐 체크섬 값과 동일한 이름을 가진 blob 객체가 생성되고, 이 객체는 ./git/objects 파일에 저장된다

- 그리고 스테이지의 내용은 .git/index에 기록된다

---

### git commit 명령의 동작 원리

```bash

# 커밋과 상태 확인

$ git commit

$ git log

$ git status



# 커밋 상태 확인

$ ls -a .git/objects
# .git/objects/ 변화 확인
# 커밋은 객체이고 객체는 .git/objects에 저장된다(ex 30이라는 이름의 폴더 생성)


$ ls -a .git/objects/30
# 방금 만든 커밋은 이 폴더 아래 있다
# 모든 커밋은 체크섬이 다르기 때문에 서로 다른 체크섬 값을 가진다


$ git show 30ac46
# 방금 만든 커밋을 확인해 보면 커밋 객체라는 것을 확인할 수 있다


```

- 하지만 커밋 후에 스테이지를 확인해 보면 스테에지가 비어있지 않는 것을 확인할 수 있다

```bash

$ git ls-files --stage # 스테이지가 비어있지 않다

$ git status

```

- clean 하다는 말은 워킹트리, 스테이지, HEAD 커밋의 내용이 모두 똑같다는 말이다

```bash
# 커밋 후 각 공간의 상태


ff5bda cat.txt ---------------> ff5bda cat.txt --------------- ff5bda cat.txt
                   (add)                           (commit)


<working tree>                      <stage>                      <HEAD>30ac46

```

---

### git tree 객체

```bash

$ ls -a .git/objects
# 7a 라는 객체 생긴 것 확인할 수 있다
# 체크섬이 같은 객체는 같은 내용을 가지게 된다


$ ls -a .git/objects/7a
# object 폴더 내용 확인
# 7a5459 객체 확인할 수 있다

$ git show 7a5459
# tree 객체라는 것을 확인할 수 있다

$ git ls-tree 7a5459
# 트리 객체 내용 확인
# 스테이지와 동일 하다

$ git ls-files --stage
# 스테이지 내용 확인, 위 트리 객체와 동일하다

$ git log --oneline -n1
# 커밋 체크섬 확인 (30ac46)

$ git cat-file -t 30ac46
# 커밋 객체 타입 확인
# commit이라는 것을 확인할 수 있다

$ git cat-file commit 30ac46
# 커밋 객체 내용 확인
# 커밋 객체의 내용을 보면 커밋 메시지와 트리 객ㅊ로 구성되어있는 것을 알 수 있따
# 트리 객체의 체크섬은 위에서 확인한 7a5459임을알 수 있다

```

- 위 과정을 통해 아래와 같이 add, commit이 동작하는 것을 확인할 수 있다

```bash
                                                               7a5456 ( 커밋 상태 최종 버전)

                                                                    |
ff5bda cat.txt ---------------> ff5bda cat.txt --------------- ff5bda cat.txt
                   (add)                           (commit)
                                                               "커밋 테스트용커밋"

<working tree>                      <stage>                      <HEAD>30ac46


```

1. 커밋을 하면 스테이지의 객체로 트리가 만들어 진다
2. 이 때 커밋에는 커밋 메시지와 트리 객체가 포함된다

---

### 파일 수정하고 추가 커밋

```bash
$ cat cat.txt
# 앞서 만든 파일 내용 확인

$ git hash-object cat.txt
# 체크섬 확인

$ echo "Hello, cat-hanbit" >> cat.txt

$ git hash-object cat.txt
# 변경된 체크섬 확인

$ git ls-files --stage
# 스테이지의 파일 확인
# 변경된 커밋이 아니라 이전 커밋의 체크섬이 출력된다

$ git ls-tree HEAD
# 헤드 커밋의 내용 확인
# 변경된 커밋이 아니라 이전 커밋의 체크섬이 출력된다

```

- 파일 내용을 변경하게 되면 워킹트리의 cat.txt의 체크섬만 다른 값을 가진다

```bash
                                                                 7a5456
                                                                    |
                                                                    |
f3e6fa cat.txt                ff5bda cat.txt ---------------> ff5bda cat.txt

                                                              "커밋 테스트용커밋"

<working tree>                   <stage>                       <HEAD>30ac46

```

- 변경 내용을 스테이지에 추가하게 되면 워킹트리와 스테이지는 동일한 체크섬을 가지게 된다

```bash

$ git add cat.txt

$ git ls-files --stage

```

- 수동으로 직접 트리를 만들고 커밋하기

```bash

$ git write-tree # 스테이지의 내용으로 트리 생성


$ git ls-tree 4ced2d
# 스테이지의 내용과 같은 것을 확인할 수 있다


```

- 트리로 커밋하기

```bash
$ echo "트리로 커밋하기" | git commit-tree 4ced2d -p HEAD
# git commit-tree 명령어를 사용하면 트리 객체를 이용해서 직접 커밋을 생성할 수 있다
# echo "트리로 커밋하기 " :  커밋 메시지를 생성하는 부분
# 4c2d2d : 트리 객체의 채크섬
# -p HEAD : 부모 커밋이 HEAD커밋 이라는 사실을 알려준다
# 커밋 객체는 반드시 부모 커밋을 가져야 하기 때문에 이렇게 부모를 지정해주는 옵션이 -p
# 커밋 체크섬은 각각 다른 값을 가진다


$ git cat-file commit aa3d23
# 생성된 커밋 확인


$ git log --oneline
# 우리가 생성한 커밋이 로그에는 나오지 않는다
# HEAD가 아직 갱신 되지 않았기 때문이다.
# git commit 명령은 이전 HEAD를 부모로 하는 커밋 객체를 생성한 후 방금 만든 새 커밋이 새로운 HEAD가 된다
# 그런데 방금 만든 커밋은 HEAD를 갱신하지 않았기 때문에 직접 해준다

```

- HEAD 갱신하기

```bash

# HEAD 갱신하기

$ ls .git # HEAD파일 확인할 수 있다

$ cat .git/HEAD

$ cat .git/refs/heads/master # 체크섬 값이 저장되어 있는 것을 알 수 있다, HEAD 커밋의 체크섬과 같은 값이다

$ git update-ref refs/heads/master 9eae86 # 직접 커밋한 객체로 업데이트 한다

$ cat .git/refs/heads/master # 업데이트 확인

$ git log --oneline # 로그 확인

```

- 앞의 단계까지 마친 후, 이 과정을 시각화 하면 다음과 같다

```bash

                                                                4c2ded
                                                                    |
f3e6fa cat.txt                    f3e6fa cat.txt                f3e6fa cat.txt

                                                          "트리로 커밋하기테스트용커밋"

                                                               <HEAD>30ac46
                                                                   ↓

<working tree>                      <stage>                    30ac460
                                                                (이전 커밋)

```

- 중복 파일 관리
- git에서 중복 파일은 blob으로 관리가 되는데 git의 blob은 제목이나 생성날짜와는 관계 없이 내용이 같을 경우 같은 체크섬을 가진다

```bash
$ cp cat.txt cat2.txt # 파일 복사

$ echo "cat-hanbit" > cat3.txt # 이전 버전과 같은 내용의 파일 생성

$ git add cat2.txt cat3.txt
# 전부 스테이지에 추가


$ git ls-files --stage
# 스테이지 내용 확인
# 해시 체크섬 값을 보면 cat.txt와 cat2.txt 파일 내용은 같고
# cat3.txt는 신규로 생성했지만 이전에 있는 cat.txt와 같은 값이라는 것을 알 수 있다

$ git commit # 커밋

```

- git add 명령은 워킹트리의 내용을 스테이지에 반영하는 것

- git commit 명령은 스테이지의 내용을 가지고 트리 객체를 만들고

- 이 트리 객체를 기반으로 기존 HEAD 커밋을 부모로 하는 새로운 커밋을 만든다

- 마지막으로 생성된 커밋은 다시 HEAD가 된다

---

### Branch

- 브랜치는 커밋의 참조일 뿐이다

- git branch test <커밋 체크섬> 명령을 실행하면

- 내부적으로는 <커밋 체크섬> 내용을 가지는 .git/refs/heads/test 텍스트 파일을 하나 생성하는 것이다

```bash

$ git branch test

$ git log --oneline
# test 브랜치는 7522e79 커밋을 참조하는 것을 알 수 있다

$ ls .git/refs/heads/
# master 파일 이외에도 test라는 파일 생성된 것 알 수 있다

$ cat.git/refs/heads/test
# 실제 파일 내용 확인
# HEAD 커밋의 전체 체크섬 내용일 들어있는 것을 확인할 수 있다

```

- 브랜치 삭제 및 재생성

```bash
$ git branch -d test

$ ls .git/refs/heads/ # test파일 사라진 것 확인할 수 있다

$ git branch test2

$ ls .git/refs/heads

$ git log --oneline -n1

$ rm .git/refs/heads/test2

$ git log --oneline -n1


```

- 브랜치 체크아웃

- 체크 아웃은 해당 브랜치로 HEAD를 이동시키고 스테이지와 워킹트리를 HEAD가 가르키는 커밋과 동일한 내용으로 변경하는 것

```bash
$ git branch test3 HEAD^ # 현재 HEAD의 부모 커밋으로 부터 test3 브랜치를 만든다

$ git log --oneline -n2

$ cat .git/HEAD

$ git checkout test3

$ cat .git/HEAD
# HEAD 파일을 보면 내용이 master에서 test로 변경된 것을 확인할 수 있다

$ git status

```

- 수동 체크아웃

```bash

$ echo "ref: refs/heads/master" > .git/HEAD
# test3 브랜치에서 HEAD파일 직접 수정

$ git log --oneline -n1
# HEAD가 master로 변경됨

$ git status
# 스테이지와 워킹트리는 변하지 않은 상태이다
# HEAD 커밋과 master 커밋으로 변경되고 워킹트리와 스테이지는 test3 커밋을 가리키고 있다

$ git reset --hard
# 간단하게 reset --hard명령을 사용하면 커밋, 스테이지, 워킹 트리의 내용이 모두 같아지게 된다

$ git status
#git checkout master 명령을 수행한 것과 동일한 상태가되었다

```

---

1. git add 명령을 수행하면 워킹트리의 내용을 스테이지에 추가한다

2. git commit 명령을 수행하면 스테이지의 내용으로 새로운 커밋을 만든다

3. 커밋 이후 git status 명령을 내리면 clean 상태임을 표시해주는데, 이 상태는 워킹트리, 스테이지, HEAD 커밋들이 모두 동일한 내용을 담고 있다는 뜻이다

4. 커밋 객체는 트리 객체와 blob 객체들의 조합으로 이루어져 있다

5. 커밋 객체는 부모 커밋에 대한 참조를 가지고 있다

6. 브랜치를 생성하면 단순히 브랜치 파일 하나를 추가한다

7. 브랜치를 체크아웃하면 HEAD를 해당 브랜치로 변경하고 브랜치가 참조하는 커밋의 내용으로 스테이지와 워킹트리의 내용을 변경한다

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
