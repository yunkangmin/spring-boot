# 명령어 정리
- git checkout -f HEAD  
HEAD를 워킹 디렉토리, 인덱스, HEAD 셋다 최신 커밋으로 변경하고    
기존 워킹 디렉토리 수정사항도 최신 커밋 내용으로 변경한다.     
원래 checkout 명령은 워킹 디렉토리를 안전하게 다룬다.      
저장하지 않은 것이 있는지 확인해서 날려버리지 않는 다는 것을 보장한다.    
또 워킹 디렉토리에서 Merge 작업을 시도하고 변경하지 않은 파일만    
업데이트한다(git reset --hard 명령은 확인하지 않고 단순히 모든 것을    
바꿔버린다).  
  
- git checkout -h  
도움말  

---

https://mytory.net/archives/10078

## 과거 히스토리로 이동하기 checkout
#### 다시 원래대로 돌아오기
```
git checkout master
```
