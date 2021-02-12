---
layout: post
title: 'git branch, merge 명령어 정리'
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

- 브랜치는 커밋을 가리키므로 `[HEAD]`도 커밋을 가리킨다

- 결국 `[HEAD]`는 현재 작업 중인 브랜치의 최근 커밋을 가리킨다

- `[HEAD]` 포인터가 `[master]` 브랜치를 가르키면 커밋2의 상태 보여주고 `A branch`를 가리키면 커밋3의 상태를 보여준다

- 브랜치의 최신 커밋이 아닌 과거 커밋으로도 `[HEAD]`를 이동시킬 수 있는데 이 경우에는 `[master]`브랜치의 포인터와 `[HEAD]`가 떨어져 있기에 `'분리된 HEAD(Detached HEAD)'` 상태가 된다

* 최신 커밋은 부모 커밋을 가르킨다

* 따라서 커밋은 부모 커밋에 대한 정보를 담고 있는 반면 부보 커밋은 자식 커밋에 대한 정보를 담고 있지 않다

* 어느 커밋에서 정보를 가져오면 부모 커밋을 알 수 있지만 자식 커밋을 알 수 없다

* 병합을 통해 생성된 병합 커밋에는 부모 커밋이 두개 존재한다

* 커밋하면 커밋 객체가 생긴다. 커밋 객체에는 부모 커밋에 대한 참조와 실제 커밋을 구성하는 파일 객체가 들어있다

* 브랜치는 논리적으로는 어떤 커밋과 그 조상들을 묶어서 뜻하지만, 사실은 단순히 커밋 객체 하나를 가르키는 것이다

* 위 그림에서 master branch는 1월 3일 버전의 커밋 객체 하나를, feature branch는 1월 4일의 커밋 객체 하나를 가리킨다

---

- 브랜치를 사용하는 경우

  - 새로운 기능추가

    - 대표적으로 브랜치를 사용하는 경우

    - [master]브랜치에는 정상적으로 동작하는 안정적인 버전의 프로젝트가 저장되어 있기 때문에 기능을 추가할 때는 [master] 브랜치의 최신커밋으로 브랜치를 생성해서 개발한다

    - 이후 완료한 다음 이상이 없으면 [master]브랜치로 병합한다

  - 버그 수정

    - 버그가 발생하면 [master]브랜치로부터 새로운 브랜치를 생성해서 작업한다

    - 이 때 브랜치 이름은 hotFix, bugFix 같은 이름을 사용한다

    - 버그 수정이 끝나면 [master] 브랜치로 병합한다

  - 병합과 리베이스 테스트

    - 병합이나 리베이스는 입문자에게 까다로운 일 중하나이다

    - 임시 브랜치를 만들어서 병합과 리베이스 테스트를 해본다

    - 잘못되었을 경우 그냥 브랜치를 삭제하면 된다

  - 이전 코드 개선

    - 새로 브랜치를 만들어 이전 코드를 과감하게 삭제하고 새 코드를 작성한다

    - 다른 브랜치의 이전 커밋에는 잘 돌아가는 코드가 남아 있기 때문에 새롭게 코드를 작성하는 것에 대해서 걱정할 필요가 없다

  - 특정 커밋으로 돌아가고 싶을 때

    - 이미 저장 되어 있는 특정 커밋으로 돌아가고 싶을 때 일반적으로 hard reset이나 revert를 사용한다. 하지만 hard reset의 경우 커밋이 없어질 수 있기 때문에 추천하지 않고 revert는 사용이 조금 까다롭다

    - 이 경우 브랜치를 새로 만들어서 작업을 하고 이후 리베이스나 병합을 사용하는 것이 좋다

---

- 여러 버전이 꼬이는 것을 방지하기 위해서 하나의 브랜치에는 한 사람만 작업해서 올리는 것이 바람직하다.

  1. `[master]` 브랜치에는 직접 커밋을 올리지 않는다 (동시에 작업하다 꼬일 수 있다)

  2. 기능 개발을 하기 전에 `[master]`브랜치 기준으로 새로운 브랜치를 만든다

  3. 이 브랜치의 이름은 `[feature/기능이름]` 형식으로 하고 한 명만 커밋을 올린다

  4. `[feature/기능이름]` 브랜치에서 기능 개발이 끝나면 `[master]` 브랜치에 이를 합친다

---

### CLI 명령어

```bash
$ git branch [-v]
# 로컬 저장소의 브랜치 목록을 보는 명령어
# -v 옵션을 사용하면 최신 커밋들도 함께 확인할 수 있다
# 표시된 브랜치 중에서 이름 왼쪽에 * 가 붙어 있으면 HEAD 브랜치이다

$ git branch --all
# github와 같은 서버에 연결된 repository 라면 서버에 있는 모든 branch 정보를 보여준다

$ git branch [-f] <브랜치이름> [커밋체크섬]
# 새로운 브랜치를 생성한다
# 커밋체크섬 값을 주지 않으면 HEAD로 부터 브랜치를 생성한다
# 이미 있는 브랜치를 옮기고 싶을 때는 -f 옵션을 사용한다

$ git branch -r[v]
# 원격 저장소에 있는 브랜치를 보고 싶을 때
# -v 옵션을 추가하면 커밋 요약을 볼 수 있다

$ git checkout <브랜치이름>
$ git switch <브랜치이름>
# 특정 브랜치로 체크아웃 할 때
# git checkout 은 뒤에 커밋 해쉬코드를 쓰면 특정 커밋으로 이동할 수 도있다

$ ls
# 특정 커밋으로 이동한 후 ls 명령어 통해 해당 브랜치 상태에 있는 파일들만 표기되는 것을 볼 수 있다

$ git checkout -b <브랜치이름> <커밋 체크섬>
$ git switch -C <브랜치이름>
# 특정 커밋에서 브랜치를 새로 생성하고 동시에 체크아웃 까지 한다

$ git merge <대상 브랜치>
# 현재 브랜치와 대상 브랜치를 병합할 때 사용
# 병합 커밋이 새로 생기는 경우가 많다

$ git branch --merged
# 현재 브랜치에 merge가 된 브랜치를 볼 수 있다
# 만약 현재 브랜치에서 새로운 브랜치를 만들기만 하고 다른 커밋을 추가하지 않은 경우 함께 표시된다.

$ git branch --no-merged
# 현재 브랜치에 merge가 되지 않은 브랜치를 볼 수 있다

$ git rebase <대상 브랜치>
# 내 브랜치의 커밋을 대상브랜치에 재배치 시킨다
# 히스토리가 깔끔해져서 자수 사용하지만 조심해서 사용해야 한다

$ git branch -d <브랜치이름>
# 특정 브랜치를 삭제한다
# HEAD 브랜치나 병합이 되지 않은 브랜치는 삭제할 수 없다

$ git push origin --delete <브랜치이름>
# 내 repository가 서버에도 있어서 branch를 삭제한 것을 원격에도 업데이트 하고 싶은 경우

$ git branch -D <브랜치이름>
# 브랜치를 강제로 삭제
# -d로 삭제할 수 없는 브랜치를 지우고 싶을 때 사용한다
# 조심해서 사용해야 한다

$ git branch --move <변경 전 브랜치이름> <새로운 브랜치이름>
# 브랜치의 이름을 변경하고 싶은 경우

$ git push --set-upstream origin <변경된 브랜치이름>
# 변경사항을 원격에 업데이트 하고 싶은 경우

$ git log master..<브랜치 이름>
# 브랜치가 너무 많은 경우에 master 브랜치와 특정 브랜치 사이의 커밋들만 확인하고 싶을 때 사용

$ git diff master..<브랜치 이름>
# 브랜치가 너무 많은 경우에 master 브랜치와 특정 브랜치 사이의 코드들을 보고 싶을 때 사용
```

---

- git checkout <커멧체크섬>을 사용하는 경우 HEAD와 브랜치가 분리되는 Detached HEAD 상황이 된다

- 이 상황에서도 커밋을 생성할 수 있지만 다른 브랜치로 체크아웃하는 순간 Detached HEAD의 커밋은 다 사라져서 안보이게 된다

- 커밋은 로컬저장소에 남아 있기 때문에 git reflog 명령으로 복구할 수 있지만 권장하지 않는 작업 방법이다

- 공식 홈페이지도 브랜치를 사용해서 체크아웃을 하고 있다

---

- reset —hard로 브랜치 되돌리기

  - rest은 현재 브랜치를 특정 커밋으로 되돌릴 때 사용한다

  - git reset `—-hard` <이동할 커밋 체크섬>

    - 현재 브랜치를 짖어한 커밋으로 옮긴다. 작업 폴더의 내용도 함께 변경된다

    - `—-hard` 옵션을 사용하려면 커밋 체크섬을 알아야 한다

    - git log를 통해 확인할 수 있지만 복잡한 커밋 체크섬을 하는 것은 번거롭기 때문에 HEAD~ 또는 HEAD^로 시작하는 약칭을 사용한다

- HEAD~<숫자>

  - HEAD~는 헤드의 부모 커밋, HEAD~2는 헤드의 할아버지 커밋을 의미한다

  - HEAD~n은 n번째 위쪽 조상이라는 뜻

- HEAD^<숫자>

  - HEAD^는 똑같이 부모 커밋

  - 반면 HEAD^2는 두번째 부모를 가르킨다

  - 병합 커밋처럼 부모가 둘 이상인 커밋에서만 의미 있다

```bash
$ git reset --hard <이동할 커밋 체크섬>

$ git reset hard HEAD~2
# HEAD를 2단계 이전으로 되돌리기

$ git log --oneline --all # 로그 확인

```

- reset —hard 명령어는 checkout 명령과 비슷하다

```bash

# reset --hard 명령어는 아래 세 명령어를 한번에 수행하는 명령어이다

# master 브랜치는 그 자리에 있고 HEAD만 옮겨진다(detached HEAD 상황)
$ git checkdout HEAD~2

 # 다시 master 브랜치와 HEAD를 연결
$ git branch -f master

$ git checkdout master

```

---

- git rebase <대상브랜치> 명령은 현재 브랜치에만 있는 새로운 커밋을 대상 브랜치 위로 재배치 시킨다

- 그런데 현재 브랜치에 재배치할 커밋이 없는 경우 rebase는 아무런 동작을 하지 않는다

- 또한 빨리 감기 병합이 가능한 경우 rebase 명령을 수행하면 빨리 감기 병합을 한다

```bash
# rebase 이전

mybranch1(HEAD)------>	 [2dbc561]
												     ↓
												 [905dda3]
	                          ↓
master         ------>	 [df1f4d1]
									          ↓
										     [cd6e80a]

# rebase 이후, 빨리 감기 병합이 되어 같은 커밋을 가리킨다

mybranch1(HEAD)------>
												 [2dbc561]
master         ------> 	     ↓
												 [905dda3]
	                          ↓
												 [df1f4d1]
									          ↓
										     [cd6e80a]

```

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

- Fast-forward 는 히스토리에 merge 되었다는 사실이 남지 않지 않는다
  하지만 히스토리에 남기고 싶은 경우라면 Fast-foward가 가능해도 새로운 commit을 만들어서 master branch에 commit할 수 있다

- 아래처럼 master 브랜치에서 새로운 브랜치를 만들고 새로운 커밋을 만든 후에 master 브랜치를 `feature/c`브랱치와 merge 하면 Fast-forward가 된다

```bash
$ git checkout -b feature/c

$ echo c > c.txt

$ git commit -m "Add c branch"

```

- 하지만 merge 흔적을 히스토리에 남기고 싶은 경우에는 다음처럼 `--no-ff`라는 옵션을 사용한다

- 아래처럼 `--no-ff`을 사용하면 새로운 커밋을 입력받기 위해 텍스트 에디터가 실행되고, 커밋 메시지를 작성한 후에 `git log`로 확인해 보면 merge 커밋이 생성되어있고 master 브랜치는 생성된 merge 커밋을 가리키고 있는 것을 확인할 수 있다

```bash
$ git merge --no-ff

$ git branch -d feature/c

$ git log
# 새로운 커밋 생성되어 있고 master 브랜치는 새로운 커밋 가리킨다
# feauture/c 브랜치는 삭제되었지만 feature/c와 합쳐졌다는 것을 알 수 있따


```

---

### 3-way 병합하기

- 3-way merge는 fast-forward merge에서 히스토리에 남는 것을 선호해서 새로운 커밋을 만들고 싶거나 또는 fast-forward가 불가능한 경우에 사용한다

- master 브랜치에서 새로운 feature/a 라는 브랜치를 만든 다음 여러개의 커밋을 추가하였고 master 브랜치에도 커밋이 추가되어 있는 경우엔은 fast-forward를 사용할 수 없다

- 그 이유는 master 브랜치에서 feature/a 브랜치를 만든 다음 추가한 커밋에 대한 정보를 잃어버리게 되기 때문이다

- 따라서 아래 처럼 원래의 브랜치에서 새로운 커밋이 추가된 경우 3-way merge를 이용해야 한다

```bash
        ----- C3 <-- C4(feature/a)
        ↑
C1 <-- C2 <-- C5 <-- C6 <-- C7 (master)
```

- 즉, 베이스 branch인 master 브랜치와 파생된 feature/a 브랜치의 변동사항을 모두 합해서 merge 커밋을 만든 다음에 master 브랜치에 커밋해야 한다

- git merge를 사용할 때 아무런 옵션 없이 사용하는 경우에 fast-forward가 가능한다면 merge commit을 만들지 않고 fast-forward merge를 진행하지만 fast-forward가 불가능한 상황이라면 커밋 메시지를 입력하도록 진행된다

```bash
$ git merge feature/a
# 새로운 커밋 메시지 입력 받는다
```

- 3-way merge 후 다음과 같은 상태가 된다

```bash
        ----- C3 <-- C4(feature/a)
        ↑
C1 <-- C2 <-- C5 <-- C6 <-- C7 <-- C8 (master)
```

- 3-way merge를 실무에서는 다음과 같은 상황에서 사용할 수 있다

- 버그를 발견한 상황에서 버그 수정은 다음과 같은 단계로 이루어 진다

  1. (옵션) 오류가 없는 버전(주로 Tag가 있는 커밋)으로 롤백

  2. `[master]` 브랜치로부터 `[hotfix]`브랜치 생성

  3. 빠르게 소스 코드 수정 / 테스트 완료

  4. `[master]` 브랜치로 병합 (Fast-forward) 및 배포

  5. 개발 중인 브랜치에도 병합 (충돌 발생 가능성이 높음)

- 버그가 발생한 상황에서는 원래 작업 중이던 브랜치도 `[master]` 브랜치로 부터 시작했기 때문에 같은 버그를 가지고 있을 것이다

- 때문에 `[hotfix]` 브랜치의 내용은 `[master]` 브랜치와 개발 브랜치 모두에 병합 되어야 한다

- 보통 `[master]` 브랜치의 병합은 빨리 감기이기 때문에 쉽게 되는 반면 개발 중인 브랜치의 병합은 병합 커밋이 생성되고 충돌이 일어날 가능성이 높다

```bash
$ git checkout master

$ git checkout -b feature1 # 브랜치 생성 및 체크아웃

$ echo "기능 1 추가" >> file1.txt

$ git add file1.txt

$ git commit

$ git log --oneline --all --graph -n2

```

- 위 상황에서 커밋한 이후 장애가 발견되었을 때

- `[master]` 브랜치에서 `[hotfix]` 브랜치를 만들고 버그를 고친 후에 커밋을 한다

- 그리고 `[hotfix]`브랜치를 `[master]`브랜치에 병합한다

- `[master]`브랜치의 최신 커밋을 기반으로 `[hotfix]` 브랜치 작업을 했기 때문에 빨리 감기 병합이 가능하다

- 따라서, 아래처럼, `[hotfix]` 브랜치를 만들고 버그를 해결한 다음 `[master]` 브랜치와 Merge한다

```bash

$ git checkout -b hotfix master # master로 부터 hotfix브랜치 생성, 체크아웃

$ git log --oneine --all -ne # 2개의 커밋 로그만 보기

$ echo "some hot fix" >> file1.txt

$ git add file1.txt

$ git commit

$ git log --oneline -n1

$ git checkout master

$ git merge hotfix

$ git push

```

- hotfix 커밋은 버그 수정이었기 때문에 이 내용을 현재 개발 중인 `[feature1]` 브랜치에도 반영해야 한다

- `[feature1]` 브랜치와 `[master]` 브랜치은 서로 다른 분기로 진행되고 있다.

- 이 경우에는 빨리 감기 병합이 불가능 하므로 `3-way` 병합을 해야한다

- 따라서 병합 커밋이 생성된다

- 모든 `3-way` 병합이 충돌을 일으키는 것은 아니지만 두 브랜치 모두 file1.txt를 수정했기 때문에 충돌이 발생한다

```bash
$ git checkout feature1 # 개발 중인 브랜치로 checkout

$ git log --oneline --all

$ git merge master # master 브랜치와 병합
# 충돌 발생

$ git status # 실패 원인 파악

# visual studio (editor)를 열어서 충돌이 일어난 부분을 수정한 후
# 변경 내용을 저장하고 다시 스테이지에 추가 및 커밋을 하면 수동으로 3-way 병합이 완료된다

$ cat file1.txt # 변경 내용확인

$ git add file1.txt

$ git status

$ git commit # git commi 명령으로 충돌난 3 way 병합을 마무리 짓는다

$ git log --oneline --all --graph -n4

```

---

### Conflict 해결 방법

- 다음은 C2에서 새로운 feature/a 라는 브랜치를 생성하고 난 이후 feature/a라는 브랜치와 master 브랜치 모두 동일한 파일을 수정한 경우이다

```bash
        ----- C3 (feature)
        ↑
C1 <-- C2 <-- C4  (master)
```

- 이러한 경우 merge를 하면 에러가 발생한다

```bash
$ git merge feature
# 에러 발생

$ git status
# 내용 확인
```

- 이러한 에러가 발생했을 때 수동으로 해결할 수 있는 방법은 다음과 같다

```bash
$ open main.txt
# 에러 발생한 파일 열어준다

# 파일 열면 아래처럼 파일이 충돌한 부분 확인할 수 있다
<<<<<<< HEAD
Oh.. Here!! From master branch!
=======
Oh.. Here!! From feature branch!
>>>>>>> feature


# master 브랜치에 있는 내용 쓰고 싶으면 feature 브랜치 내용 삭제해주고
# feature 브랜치에 있는 내용 쓰고 싶으면 master 브랜치 내용 삭제해준다
# 혹은 둘다 넣고 싶은 경우 confict와 관련된 <<<HEAD, >>>feature 지워준다

```

- 다만 이처럼 confict 해결할 때 다른 작업은 하지 않고 충돌한 부분만 해결한다

```bash
$ git merge --abort
# merge 취소하고 싶은 경우

$ git status
# both modified 확인할 수 있다
# (use "git add <file>..." to mark resolution) 부분 통해 add 해야하는 것을 알 수 있다

$ git add main.txt

$ git merge --continue
# fast forward가 아니기 때문에 merge commit 입력 받는다

$ git log
# 새로운 커밋 만들어진 것 확인할 수 있다
```

- vscode로 confict 해결하는 방법

```bash
$ git config --global -e

# 아래 명령어를 추가해준다
[merge]
  tool = vscode
[mergetool "vscode"]
  cmd = code --wait $MERGED


# 충돌 발생하면 vscode가 열린다
$ git mergtool

$ git status
# main.txt.orig 라는 이전에 merge confict가 발생했을 때의 내용을 포함한 파일도 함께 있다

$ git config --global mergetool.keepBackup false
# 해당 파일이 생기는 것을 막기 위한 옵션

```

- vscode 말고 또 다른 방법으로 merge를 할 수 있는 툴로는 [p4merge](https://www.perforce.com/ko/jepum/helix-core-apps/merge-diff-tool-p4merge) 가 있다

```bash

$ git config --global -e

[mergetool "p4merge"]
  path = "설치경로"

$ git mergetool
# p4merge가 실행된다
```

---

### 임시 브랜치 사용하기

- 입문자들이 merge나 rebase할 때 소스가 깨지거나 작업의 내용이 사라지는 걱정이 있는데 이럴 때 임시브랜치를 활용한다

- 원래 작업하려고 했던 브랜치의 커밋으로 임시 브랜치를 만들고 나면 해당 브랜치에서는 아무작업이나 해도 상관이 없다

- 나중에 그 블내치를 삭제하기만 하면 모든 내용이 원상 복구 된다

- 임시 브랜치가 필요 없어지는 git branch -D <브랜치 이름> 명령으로 삭제할 수 있다

```bash
$ git branch test feature1 # featue1 브랜치에서 임시 브랜치 생성

$ git checkout test

$ echo "abc" > test1.txt

$ git add .

$ git commit -m "임시 커밋"

$ git log --oneline --graph --all -n4

$ git checkout master

$ git branch -D test

$ git log --oneline --graph --all -n3
```

---

## Reference

- [팀 개발을 위한 Git, Github 시작하기](http://www.yes24.com/Product/Goods/85382769)
