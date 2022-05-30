### 정리
- reset은 HEAD의 위치를 바꿔버리는 반면에 revert는 커밋의 내용을 되돌리는 커밋을 새로 만든다.   
- merge된 커밋을 revert 할 때 아래와 같이 명령어를 사용한다.
```
$ git revert -m 1 1123a41
$ git revert -m 2 1123a41
```
위와 같이 명령어를 사용할 때 revert는 어느 커밋으로 되돌아가는지를 말한다.  
단순히 git revert [커밋아이디]를 사용하면 해당 커밋이 되돌려지는 것이다.(해당 커밋을 안한것과 같음)
그리고 revert는 되돌려진 후 커밋까지 되는 명령어이다.
git revert --no-commit 옵션을 주면 되돌려지는 것까지만 하고 커밋은 안한다.(index까지만 변경된 상태)

## git revert 사용법 : 이미 커밋한 내용을 되돌리는 방법
Git 저장소에서 작업을 하다 보면 이미 커밋한 내용을 되돌려야할 때가 있다.  
혼자서 작업하거나 로컬에서 작업중일 때는 reset 명령어로 브랜치를 특정 커밋으로 완전히  
되돌려 버릴 수도 있지만, 원격 저장소에 이미 Push한 내용을 변경하려면 force push를 해야만 한다.  
협업할 때는 force push는 절대해서는 안되며, 원격 저장소에서도 막아놓는게 일반적이다. 
이럴 때 사용할 수 있는 명령어가 바로 특정 커밋의 내용을 되돌리는 revert이다.  
이 글에서는 git revert 명령어를 사용하는 방법에 대해서 소개한다.  


### git revert: patch 파일로 이해하는 revert 명령어
Git 저장소에 커밋한 내용을 되돌리는 방법으로는 크게 reset과 revert가 있다.  
reset은 HEAD의 위치를 바꿔버리는 반면에 revert는 커밋의 내용을 되돌리는 커밋을  
새로 만든다.  
따라서 원격 저장소에 삭제하고 싶은 내용을 Push했더라도 revert 커밋을 추가해주기만 하면된다.  
  
여기서는 revert에 대해서 좀 더 깊게 알아본다.  
마지막 커밋에 d라는 커밋을 추가했는데, 이 내용을 되돌리고 싶다고 가정해보자.  
먼저 최신 커밋들을 한 번 확인해보자.  

```
$ git log --oneline -n 3
adf0356 (HEAD -> master) Add d
3cdce39 Add c
e71e556 Add d
```
마지막 커밋 ID가 adf035680일 때, 이 변경사항에 대해서 다음 명령어로 패치 파일을 생성할 수 있다.  
(패치파일은 커밋의 변경사항을 파일로 저장한 내용이다.)

```
$ git format-patch -1 adf035680
0001-Add-d.patch
```
생성된 패치 파일의 내용을 확인해보자.
```
$ cat 0001-Add-d.patch
From adf0356806ef55b757197c5f3df724abe96629d2 Mon Sep 17 00:00:00 2001
From: Lainyzine <lainyzine.com@gmail.com>
Date: Sun, 9 May 2021 19:03:24 +0900
Subject: [PATCH] Add d

---
 d | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 d

diff --git a/d b/d
new file mode 100644
index 0000000..e69de29
--
2.30.1 (Apple Git-130)

```
 이 내용은 d 파일을 추가하는 내용이다. git apply -R 명령어를 사용하면  
 이 커밋 내용을 거꾸로 적용할 수 있다. 
 ```
$ git apply -R 0001-Add-d.patch
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    deleted:    d

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    0001-Add-d.patch

no changes added to commit (use "git add" and/or "git commit -a")
```
git status로 저장소 상태를 확인해보면 d파일이 삭제된 것을 확인할 수 있다.  
patch 파일은 삭제하고 이 내용을 커밋해준다.  
```
$ rm 0001-Add-d.patch
$ git add .
$ git commit -m 'Revert adf035680'
[master 06a56d9] Revert adf035680
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 d

$ git log --oneline -n 3
06a56d9 (HEAD -> master) Revert adf035680  #d 파일을 삭제한 커밋을 추가
adf0356 Add d
3cdce39 Add c
```
여기까지 수동으로 커밋을 되돌리는 revert 작업을 해봤다.  
  
다시 adf035680 커밋으로 reset하고 이번에는 revert 명령어로 똑같은 작업을 해보자.  
먼저 reset 후 저장소 로그를 확인해보자.

```
$ git reset --hard adf0356
$ git log --oneline -n 3
adf0356 (HEAD -> master) Add d
3cdce39 Add c
e71e556 Add b
```

이번에는 revert 인자로 revert할 커밋ID adf0356를 주고 실행한다.  
```
$ git revert adf0356 --no-edit
Removing d
[master 6807b0a] Revert "Add d"
 Date: Sun May 9 19:21:50 2021 +0900
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 d
```
 
d 파일이 삭제된 것을 확인할 수 있다.  
git log 명령어로 현재 저장소의 상태를 확인해보자.  
```
$ git log --oneline -n 3
6807b0a (HEAD -> master) Revert "Add d"
adf0356 Add d
3cdce39 Add c
```
최신 커밋을 Patch 파일로 만들고 거꾸로 적용하고 커밋한 결과 revert로 커밋을 되돌린  
것이, 똑같다는 것을 확인할 수 있다. 
  
(이 예제에서는 단순히 d라는 파일을 삭제하고 커밋을 해도 같은 결과를 얻을 수 있다.  
커밋 내용이 많다면 위와 같이 revert 명령어를 사용하는 게 매우 편리하다.)

### edit 옵션: revert 커밋 메시지 편집하기
앞선 예제에서 revert를 실행할 때 명시적으로 --no-edit 옵션을 사용했다.  
이렇게 실행하면 자동적으로 커밋 메시지가 아래와 같이 작성된다. 

```
commit 6807b0a7a05ed7788a2a66a49f77ef47d0b3785e (HEAD -> master)
Author: Lainyzine <lainyzine.com@gmail.com>
Date:   Sun May 9 19:21:50 2021 +0900

    Revert "Add d"

    This reverts commit adf0356806ef55b757197c5f3df724abe96629d2.
```
커밋 메시지를 따로 수정하고 싶지 않다면 --no-edit 옵션을 붙여준다.  
옵션없이 실행하거나 -e(--edit) 옵션을 사용하면 revert를 실행할 때 editor 환경변수에  
지정된 에디터가 실행된다.  

```
git revert [commitId] -e
git revert [commitId] --edit
```
에디터에서 커밋 내용을 수정할 수 있다.  
저장 후 에디터를 종료하면 수정된 커밋 메시지가 저장된다.  
  
### git revert HEAD : 바로 직전 커밋을 되돌리는 커밋 만들기
바로 직전에 작업한 내용을 되돌리는 커밋을 만들고 싶다면,  
아래 명령어를 실행한다.  
```
git revert HEAD
```
단, HEAD는 항상 바로 직전 커밋을 의미하는 것은 아니다.  
정확히는 현재 위치를 가르키기 때문에 커밋ID를 명시적으로 지정하는 것을 추천한다.  
  
### git revert [commitId] : 특정 커밋의 내용을 되돌리는 방법
revert 명령어는 꼭 바로 직전의 커밋을 되돌리는 용도로만 사용하는 것은 아니다.  
앞에서 살펴보았지만 revert 명령어는 커밋ID를 인자로 받는다.  
즉, 어떤 커밋의 내용이든 되돌리는 것이 가능하다.  
저장소가 아래와 같은 상황이라고 해보자.  

```
$ git log --oneline -n 4
adf0356 (HEAD -> master) Add d
3cdce39 Add c
e71e556 Add b
f665b8f Add a
```
다음 예제에서는 a 파일을 추가한 f665b8f 커밋을 되돌려보겠다.  

```
$ git revert --no-edit f665b8f
Removing a
[master fc59dd2] Revert "Add a"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 a
```
정확히 f665b8f 커밋만 revert 되었다.  

### 여러 커밋을 한꺼번에 되돌리는 방법
revert 명령어로 여러 커밋을 되돌리 수도 있다.  
커밋 ID를 여러개 지정해서 실행하면, revert가 여러 번 진행된다.  
먼저 adf03568(Add d)로 리셋하고 이번에는 2개 커밋을 revert해보자.

```
$ git reset --hard adf03568
$ git revert --no-edit adf03568 3cdce394
Removing d
[master cd5e402] Revert "Add d"
 Date: Sun May 9 19:41:34 2021 +0900
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 d
Removing c
[master 26be041] Revert "Add c"
 Date: Sun May 9 19:41:34 2021 +0900
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644
```
2개의 커밋이 revert되지만, 각각 따로 revert 커밋이 생성된다.  
git log로 확인해보자

```
$ git log --oneline -n 5
26be041 (HEAD -> master) Revert "Add c"
cd5e402 Revert "Add d"
adf0356 Add d
3cdce39 Add c
e71e556 Add b
```
revert 커밋이 2개 만들어진 것을 확인할 수 있다.  
  
한 번에 여러 커밋을 revert하는 단일 커밋을 만드는 방법도 있다.  
git revert는 되돌릴 내용을 Index에 반영하고 커밋은 하지 않는 옵션을 제공하고 있다.  
다시 앞선 상태로 reset하고, 이번에는 -n 옵션을 사용해보자.  

```
$ git reset --hard adf03568
$ git revert -n adf03568 3cdce394
$ git status
On branch master
You are currently reverting commit 3cdce39.
  (all conflicts fixed: run "git revert --continue")
  (use "git revert --skip" to skip this patch)
  (use "git revert --abort" to cancel the revert operation)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    deleted:    c
    deleted:    d
```
두 개 커밋을 되돌린 내용이 한꺼번에 Index에 반영되어있지만, 커밋은 만들어지지 않은  
상태이다.  
필요하다면 이 상태에서 커밋에 반영할 추가적인 변경 작업을 할 수 있다.  
변경을 마치고 git revert --continue를 실행하면 커밋을 진행한다.  
이 때는 변경사항이 하나로 커밋된다.  

```
$ git revert --continue
# editor로 커밋 메시지 작성
```

### 머지 커밋(Merge commit)을 되돌리는 방법
이번에는 머지 커밋을 revert로 되돌리는 방법을 알아본다.  
아래와 같은 머지 커밋이 있다.   

```
$ git show 1123a41
commit 1123a419d52f8eea2273411b4afdfa1914e4195c (HEAD -> master)
Merge: 94668df 1bf9843
Author: Lainyzine <lainyzine.com@gmail.com>
Date:   Sun May 9 19:53:19 2021 +0900

    Merge branch 'merge'
```

이 커밋을 그냥 revert하면 다음과 같이 에러 메시지가 발생하며 실패한다.  

```
$ git revert 1123a41
error: commit 1123a419d52f8eea2273411b4afdfa1914e4195c is a merge but no -m option was given.
fatal: revert failed
```

1123a41 커밋은 94668df 커밋과 1bf9843 커밋을 머지한 커밋이다.  
이러한 커밋을 머지(Merge) 커밋이라고 부르는데, 이런 커밋을 되돌릴 때는  
어느 쪽 커밋으로 되돌릴 지 지정해주어야 한다.  
이 때 사용하는 옵션이 바로 m 옵션이다.  

```
$ git revert -m 1 1123a41
$ git revert -m 2 1123a41
```
예를 들어 94668df 커밋으로 되돌리려면 -m 1, 1bf9843 커밋으로 되돌리려면 -m 2를 지정해주면 된다.  
(git show에서 보여지는 Merge: 뒤의 값들에 차례대로 번호가 부여된다.)























































