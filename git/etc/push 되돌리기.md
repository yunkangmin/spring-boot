### 정리
- 머지 후 푸시된 것 되돌리기
   1. git log --oneline으로 커밋 이력확인 or 인텔리제이 show history
   2. 되돌리고 싶은 커밋을 git revert -m 1 [커밋아이디]로 되돌림
   3. git status로 커밋확인
   4. push

## push 되돌리기
Git으로 버전 관리를 하며 개발하다보면,  
작성한 커밋들을 되돌려서 다시 이전 상태로 원상복구하고 싶은 경우가 한번쯤 있을 것이다.  
만약 로컬까지만 저장된 커밋의 경우는 $git reset 명령어를 이용해 쉽게 커밋을 되돌릴 수 있지만  
이 커밋이 GitHub과 같은 원격 저장소까지 이미 올라갔다면 애기는 조금 달라진다.  
이 글에서는 이를 해결하기 위한 몇 가지 방법들을 하나씩 소개하려고 한다.  

> 원격 저장소로는 가장 대중적으로 사용되고 있는 GitHub을 사용한다. 

우선 아래와 같이 "Commit A", "Commit B", "Commit C" 세 개의 커밋들을  
모두 푸시까지 한 상태에서 되돌리고 싶은 상황이라고 가정해보자.  
참고로 세 커밋의 작업 내용은 아래와 같다.  

- Commit A : A.txt 생성  
  (b31064c29b55f826d1dae3f1bddecc4a27a10e36)
- Commit B : B.txt 생성  
  (16e2e7f1789973a227a5e6bb47221ed683a8b1ae)
- Commit C : C.txt 생성  
  (03001425c69fe8eccf219db243d9ecc9a60d64b8)
  
### 1. 로컬에서 커밋 되돌린 후 강제 푸시
첫번째 방법은 로컬 저장소에서 일단 커밋을 되돌린 후, 이를 원격 저장소에 강제로  
반영시키는 방법이다.  

#### 방법
먼저 로컬에서 $ git rest 명령어를 이용해 내가 되돌리고 싶은 커밋들을 되돌린다.  

```
$ git reset --hard HEAD~3
```

그리고 난 후, $ git push를 실행하면

```
$ git push origin master
```
아래와 같은 에러 문구가 나타날 것이다.  

![image](https://user-images.githubusercontent.com/33191974/136922733-fae9b94f-cdd5-45e8-a676-af4938c1f610.png)  

로컬 저장소의 커밋 히스토리가 원격 저장소의 커밋 히스토리보다 뒤쳐져 있는데  
푸시를 하였으므로 발생하는 에러이다.  
하지만 지금 우리가 원하는 것은 이 뒤쳐져 있는 로컬 저장소의 커밋 히스토리를 원격 저장소의  
커밋 히스토리로 강제로 덮어쓰는 것이므로 이를 위한 옵션 -f 또는 --force를 명령어에 추가하여야 한다.  

```
$ git push -f origin master
```
GitHub 페이지를 통해 원격 저장소에서의 커밋이 되돌려졌음을 확인할 수 있다.  

#### 주의사항
이 방법을 이용하면 원격 저장소에 흔적도 없이 내가 만들었던 커밋들을 제거할 수 있으므로  
겉보기에는 가장 깔끔한 해결책으로 보인다.  
하지만 만약 해당 브랜치가 팀원들과 공유하는 브랜치이고,  
내가 커밋들을 되돌리기 전에 다른 팀원이 혹시나 내가 작성한 커밋들을 이미 pull로 땡겨갔다면  
그 때부터 다른 팀원의 로컬 저장소에는 내가 되돌린 커밋들이 남아있게 된다.  
그 커밋들이 되돌려진 사실을 모르는 팀원은 자신이 작업한 커밋들과 함께 push할 것이고  
그 때 내가 되돌렸던 커밋들이 다시 원격 저장소에 추가되게 된다.  
  
따라서 이 방법은 다른 팀원이 내가 되돌린 커밋들을 pull로 땡겨가지 않았다고 확신할 수  
있는 경우, 예를 들어

- 나 혼자만 사용하는 브랜치에 push하였고, 이를 되돌리고 싶은 겨우
- 팀원들과 직접 커뮤니케이션해서 내가 되돌린 커밋을 pull로 땡겨간 팀원이 없다고  
  확인된 경우

이러한 경우에는 안전하고 간편하게 사용할 수 있는 방법이다.  

### 2. git revert 사용하기
앞서 1번 방법에서 발생했던 근본적인 문제점은 다른 팀원들과 공유하는 원격 저장소의  
커밋 히스토리를 강제로 조작한다는 점이다.  
이 때 $ git revert 명령어를 사용하여 revert 커밋을 커밋 히스토리에 쌓는 방식을 사용한다면  
이러한 문제점을 막을 수 있다.  
쉽게 말해서, 특정 커밋을 되돌리는 작업도 하나의 커밋으로 간주하여 커밋 히스토리에 추가하는  
것이므로, 내가 되돌린 작업을 다른 팀원들과 공유할 수 있게 된다.  

#### 방법
$ git revert [되돌리고 싶은 commit의 hash]는 특정 커밋에서의 변경 사항을 제거하는  
또 다른 커밋을 생성하는 명령어이다.  
따라서 Commit A -> Commit B -> Commit C 커밋의 순서로 커밋 히스토리가 쌓여있는 걸  
생각해봤을 때, 이를 다시 원래대로 돌리기 위해서는 Commit C -> Commit B -> Commit A  
순서로 거꾸로 revert하여야 한다.  
(우리가 흔히 사용하는 "실행취소"를 생각하면 이해하기 쉽다.)

```
$ git revert 03001425c69fe8eccf219db243d9ecc9a60d64b8 # Revert "Commit C"
$ git revert 16e2e7f1789973a227a5e6bb47221ed683a8b1ae # Revert "Commit B"
$ git revert b31064c29b55f826d1dae3f1bddecc4a27a10e36 # Revert "Commit A"
```
![image](https://user-images.githubusercontent.com/33191974/136925743-77b118d0-150a-46d4-a81d-2376bb277202.png)  
커밋 로그를 보면 알 수 있듯이, 이렇게 하면 되돌리고 싶은 커밋의 수만큼의 불필요한 revert 커밋이  
생겨난다.  
즉, 되돌리고 싶은 커밋이 100개라면 100개의 revert 커밋을 추가해야 한다.  
이 때 --no-commit 옵션을 이용하면 revert를 위한 커밋을 하나만 생성할 수도 있다.  
$git revert -no--commit [되돌리고 싶은 commit의 hash]을 실행하면, 아까처럼  
revert 커밋이 자동으로 생성되는게 아니라 working tree와 index(staging area)에만  
변경사항이 적용된다.  
![image](https://user-images.githubusercontent.com/33191974/136926276-5064d1e9-12e4-4212-8e50-8091f4b8b452.png)  
하지만 이 방법도 역시 일일이 revert할 커밋의 수만큼 명령어를 반복해서 실행해야한다는 단점이 있다.  
다행히 $ git revert는 특정 커밋 하나뿐만 아니라 복수개의 커밋에 대해서도 revert를 지원해주고 있다.  
그 때는 특정 커밋의 hash가 아닌 [되돌리고 싶은 커밋의 범위]를 인수로 입력해주면 된다. 

```
$ git revert --no-commit HEAD~3.. # 또는 master~3..master
```
마지막으로 index에 올라간 변경들을 한꺼번에 커밋한 다음, 원격 저장소에 푸시하면 된다.  
```
$ git commit m 'Revert "Commit C, B, A"'
```
![image](https://user-images.githubusercontent.com/33191974/136936546-cd9cba45-5dab-485f-9617-b9b91c339c6f.png)  
```
$ git push origin master
```
![image](https://user-images.githubusercontent.com/33191974/136936666-944d7fca-ec38-4f95-ae70-e46539d625a3.png)









































