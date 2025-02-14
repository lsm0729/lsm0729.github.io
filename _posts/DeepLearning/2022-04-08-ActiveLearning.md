---
title:  "[ActiveLearning] Not All Labels Are Equal." 

categories:
  - DeepLearning
tags:
  - [ActiveLearning]


toc: true
toc_sticky: true

date: 2022-04-08
last_modified_at: 2022-04-08
---

Not All Labels Are Equal: Rationalizing The Labeling Costs for Training Object Detection 논문 기반 이해한 내용 및 실험 결과에 대한 고찰입니다. -> [논문 링크](https://arxiv.org/abs/2106.11921)
{: .notice--warning}

> 원격으로 내 프로젝트와 코드들을 버전별로 백업할 수 있다.

[누구나 쉽게 이해할 수 있는 Git 입문](https://backlog.com/git-tutorial/kr/intro/intro1_1.html)

- 로컬에서 관리한 코드 파일들을 <u>Git 명령어들을 통해 내 Github 저장소(원격 서버)에 올려 버전 별로 관리</u>할 수 잇다.

## Git

- Working Directory
  - `.git`이 관리하나 추적 대상은 아닌 작업 디렉터리
  - `git status` 해보면 이부분의 파일들은 Untracked files라고 나타낸다.
  - 내용이 수정된 파일은 자동으로 Working Directory 에 올라와 `git add`시킬 수 있는 상태가 된다.
- Stage Area
  - 곧 `commit`이 될 파일들이 모여있는 곳(`git add` 한 파일들)
  - Git이 추적한다. 
  - `git status` 해보면 이부분의 파일들은 Changes to be committed 라고 나타난다.
  - 이 곳에 올라와 있는 커밋 목록들이 최종적으로 원격 서버에 업로드 된다.

### Git을 사용할 준비

- Git bash 를 켠다.
- `cd`를 이용해 로컬 Git Repository로 설정할 로컬 폴더로 이동한다.
- 이 로컬 환경에서 최초로 Git 을 사용하는 경우엔 내 Github 계정을 등록해두어야 한다. 
  - ***
- git config --global user.name "계정 이름" 명령어로 내 Github 계정을 등록한다.
    - 컴퓨터에 등록된 깃허브 계정을 바꾸려면 [자격 증명 관리자] 윈도우 설정에 들어가야함

<br>

### Git 명령어

#### git init

> `git init` 

- 👉 현재 위치의 로컬 폴더를 로컬 Repository로 만든다.

로컬 폴더를 Git Repository로 만들기 위해서 필요한 명령어다. 이제 이 로컬 폴더에 `.git` 폴더가 생기기 때문에 여러가지 Git 명령어를 사용할 수 있다. 이 폴더를 원격 서버인 내 Github 계정에 올릴 수도 있다.

<br>

#### git clone

> `git clone + Github주소` 

- 👉 Github 원격 서버에 있는 어떤 Github Repository를 내 로컬(내 컴퓨터)로 복사해와 로컬 Repository로 만들고 싶을 때 사용한다.

```
git clone https://github.com/ansohxxn/my_first_cpp_project
```

위 명령어를 실행하면 이제 현재 로컬 폴더 위치에 https://github.com/ansohxxn/my_first_cpp_project 원격 서버에 위치한 해당 Repository 내용물 전체가 복사되고 로컬에 저장된다. 그럼 이제 현재 폴더에 `.git` 하위 디렉터리가 생기고 이 곳이 로컬 Repository가 된다. `git init`은 로컬 폴더를 아직 Github 원격 서버에 올리지 않은 Repository로 삼는 작업이라면 `git clone`은 기존에 이미 Github 원격 서버에 있는 Repository를 로컬로 가져와 로컬 Repository 를 만드는 역할을 한다. 원격 저장소에서 `zip` 파일로 받으면 `.git` 폴더는 없는 체로 복사되기 때문에 복사해와서 Git 을 사용하려면 `git clone`으로 복사해오는게 좋다. `.git`폴더까지 복사되기 때문에.

<br>

#### git add

```
git add my_first_code.cpp
git commit -m "뫄뫄 기능을 추가, 롸롸 기능을 삭제함"
git push
```

> `git add + Github 원격 서버에 올릴 파일` 

- 👉 Commit 목록에 로컬 github repository 폴더 내의 파일들의 변경 상태를 추가
- Repository에 내용이 변경이 된 파일을 반영하거나, 새로운 파일을 추가하려고 할 때 사용한다. 파일이 수정 되거나 추가 되어도 `git add`, `git commit` 과정을 거치지 않으면 Repository에 반영되지 않는다.
- `git add .` 👉 레포지토리 내의 모든 파일의 변경 사항을 Commit 목록에 추가하려고 할 때

<br>

#### git rm

> `git rm + 삭제할 파일` 

- 👉 레포지토리 내의 파일이 삭제됐거나 혹은 Github 원격 서버에 해당 파일이 반영되지 않기를 원할 때.
- Commit 목록에 이를 알린다. 

> `gir rm --cached`

- Commit 될 목록인 Stage Area 에서만 제거하고 Working Directory 에서의 상태는 유지하고 싶을 때

<br>

#### git commit

> `git commit -m "커밋 메모 메세지"` 

- 👉 `git add`와 `git rm`으로 추가되 왔던 현재의 Commit 목록을 Github 원격 서버에 반영할 준비를 한다.
  - 이 커밋 목록 단위로 원격 서버에 올린다.
- 단 `git commit`만으로는 원격 서버에 반영되지 않는다. 내 로컬 레포지토리 안에서만 반영이 된 것 뿐이다.
- `git commit`을 하고 나면 현재의 Commit 목록은 비워진다.
- `-m`을 붙여 커밋 메세지를 적어줄 수도 있다.
  - 나중에 수정 사항들을 다시 보기 편하게 하기 위해서 관련있는 사항들끼리 묶어서 커밋하고 어떤 사항들이 반영된 것인지 커밋 메세지를 적어주는 것이 좋다. 
  - 커밋 메세지를 적지 않고 그냥 `git commit`만 하면 커밋 메세지를 적는 vi 에디터가 나올텐데 `:wq` 를 입력하여 빠져나올 수 있다.
- `git commit` 명령어를 사용하지 않고 Github 원격 레포지토리 사이트에서 Commit 할 수도 있다.

<br>

#### git remote 

> `git remote`

현재 이 로컬 저장소에 연결되어있는 원격 저장소를 알려준다.

> `git remote add {remote repository}` 혹은 `git remote add origin` (origin은 remote repository의 별칭이다.)

로컬 레포지토리로부터 원격 서버에 올리기 위해선 어떤 원격 Github 저장소에 올릴 것인지를 정해야 한다. 위 명령어로 최초에 한번만 정해놓으면 다음엔 `git push`해도 이때 정해놓은 원격 저장소로 알아서 업로드 할 수 있다. 원격으로부터 `git clone` 해와서 로컬 레포지토리를 만드는 경우가 아니라 처음부터 `git init`으로 로컬 레포지토리를 만든 후에 다른 원격서버에 올리는 경우 등등 이런 경우에 사용할 수 있겠다.

<br>

#### git push

> `git push`

- 👉 그동안 로컬에 반영되어있던 Commit 목록들을 전부 연동되어 있는 Github Repository 원격 서버에 올린다. 

> `git push 원격 저장소 별칭 + 브랜치 이름` (**git push origin master**)

`origin`은 원격 저장소의 별칭이기 떄문에 현재 연결되어있는 원격 저장소의 `master` 브랜치에 이를 `push`하라는 의미가 된다.

> `git push --deletate 브랜치이름`

해당 브랜치 삭제

<br>

#### git pull

> `git pull`

- 👉 Github 원격 서버 상에서 Repository 내용을 <u>이와 연동이 되어 있는, 동일한 Repository인 내 로컬 Repository에 변경 사항들을 반영 시킬 때</u> 사용한다.
- 주의 📢
    - 협업 중에 동료가 코드를 수정해서 바꾸고 Github Repository에 `push` 했다면 나는 변경된 repository를 `git pull`해서 Local Repository 에 적용한 후에 작업을 해야 한다.
    - 이렇게 변경 사항을 적용 하지 않고 `push` 해 버리면 에러가 난다. 협업시 원격 저장소에 변경 사항이 있을 경우를 대비해 항상 `pull` 먼저 하고난 후에 작업을 시작하도록 하자.
- `git pull`은 연결된 원격 저장소에 권한이 있는 사람마 ㄴ가능하다.
- 로컬 저장소로 가져오는 과정에서 **병합이 발생**한다.
- `git pull`은 `git fetch + git merge`와 같다. 

> `git pull 원격 저장소 별칭 + 브랜치 이름` (**git pull origin master**)

- 해당 원격 저장소의 해당 브랜치를 가져와서 현재의 브랜치에 `merge` 한다. `git pull`은 `merge`도 해주니까! 
- git pull origin master 👉 `origin` 원격 저장소의 `master` 브랜치를 현재 브랜치로 가져온다.

<br>

### git fetch

> `git fetch`

- 병합 과정 없이 원격 저장소의 최신 내용으로 업데이트 하는 방식. 
  - 병합 없는 `git pull`과 같다.

<br>

#### git status

> `git status`

- 👉 현재의 Commit 목록을 확인

<br>

#### git stash

> `git stash`

아직 마무리 되지 않은 현재의 Working Directory(파일에 변화는 있지만 add 는 아직 안된)에 있는 파일들 작업을 스택에 잠시동안 임시 저장하는 명령어다. 즉 하던 작업을 임시로 저장하는 일이다. 예를 들어 <u>현재 작업을 다 마치지 않았는데 다른 브랜치로 잠시 변경해야할 일이 생긴다면</u>, 현재 작업을 다 못 마쳐도 그냥 미완성인 상태로 commit 해두고 다른 브랜치로 변경할 수도 있지만, 불필요한 commit을 생성하는 일이기 때문에 좋지 않다. `git stash` 하면 <u>현재 작업 중인 Working Directory가 스택에 임시로 저장되고 Working Directory는 비워진다.</u> 

> `git stash list`

현재 임시 저장된 `stash`된 임시 저장 파일 목록들을 확인할 수 있다. 스테이시 된 파일들의 인덱스가 적용되어 있다.

> `git stash apply`

가장 최근의 `stash`를 가져와 적용한다. 즉, 임시 저장해놨던 파일들을 다시 Working Directory로 불러들인다. 


***




[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
