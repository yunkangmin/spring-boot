https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0  
  
- Working Directory는 변경 내용이 실제 파일로 존재한다.  
  바로 눈에 보이기 때문에 사용자가 편집하기 수월하다.  
  HEAD와 index가 .git 디렉토리에 존재하며 이는 컴퓨터에 효율적인 상태이기 때문에  
  사람이 알아보기 어렵다.
- HEAD는 현재 브랜치의 가장 마지막 커밋의 스냅샷이다.
- index는 stage를 말함.  
- git status 명령어를 사용했을 때 변경사항이 없다고 나온 것은 세 트리가 같기 때문이다.  
- Changes not staged for commit -> 커밋을 위해 준비되지 않은 변경 사항
- Changes to be committed -> 커밋 할 변경 사항
- branch를 checkout하면 HEAD가 새로운 브랜치를 가리키고,  그 브랜치의 마지막 커밋의 스냅샷을  
  index에 놓는다. 그리고 index의 내용을 워킹 디렉토리로 복사한다.  
  -> 즉, HEAD, index, Working Directory가 같아진다. 

### Reset
- HEAD, index, Working Directory를 이해하면 reset 명령이 어떻게 동작하는지 쉽게 알 수 있다.
- reset 명령이 하는 첫 번째 일은 HEAD 브랜치를 이동시킨다.  
  checkout 처럼 HEAD가 가리키는 브랜치를 바꾸지는 않는다.  
  HEAD는 계속 현재 브랜치를 가리키고 있고, 현재 브랜치가 가리키는  
  커밋을 바꾼다.  
  즉 git reset 9e5e6a4 명령은 현재 브랜치에서 9e5e6a4 커밋으로 HEAD를 이동시킨다.
  index와 Working Directory는 마지막 스냅샷을 가리키고 있다.
  
#### soft
- reset --soft 옵션을 사용하면 딱 여기까지만 실행한다.
- git reset [] HEAD~ 명령은 index나 워킹 디렉토리는 그대로 놔두고 HEAD를 부모 커밋(바로 직전 커밋)으로 되돌린다.
  이 상태에서 index를 업데이트하고 커밋을 하면 git commit --amend 명령과 같다.  
  즉, 커밋을 새로 만들지 않고 HEAD에 있는 커밋을 수정한다. 

#### mixed
- mixed는 soft에서 한 발짝 더 나아가 index를 현재 HEAD가 가리키는 스냅샷으로 업데이트할 수 있다.
  딱 여기까지가 mixed의 역할이다.
- reset 명령을 실행할 때 아무 옵션을 주지 않으면 기본적으로 --mixed 옵션으로 동작한다.
  -> git reset HEAD~
  
#### hard
- hard 옵션을 사용하면 워킹 디렉토리까지 업데이트한다. 





















  
