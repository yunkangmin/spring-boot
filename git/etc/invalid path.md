- 윈도우에서 `git clone`을 하는데 `invalid path` 에러가 발생한다.  
- 원인 파일을 보니 이름에 특수문자가 들어가서 그런 것 같다.   
- 상태 확인   
   - `git clone`을 한 뒤 해당 폴더에 들어가면 아무것도 없다.
   - `git status`를 입력하면 모두 `deleted` 상태라고 나온다. 
   - `git log`를 입력하면 로그는 정상적으로 나온다.  

- 찾아보니 윈도우 특유의 파일 보호 시스템인 듯하다.
- 터미널의 해당 폴더에서 다음과 같은 명령어를 입력한다.  
```
git config core.protectNTFS false
git checkout -f HEAD
```
- `git status`, `ls` 명령어로 확인해보면 문제의 파일을 제외하고 제대로 있는 것을   
확인할 수 있다.   




































