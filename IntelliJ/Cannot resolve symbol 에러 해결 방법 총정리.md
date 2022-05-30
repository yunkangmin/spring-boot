사실 이 에러는 Intelli J IDEA에서 잊을 만 하면 나오는 자주 보이는 에러이다.   
import가 제대로 안돼서 생긴 에러이다.   

# 해결방법
## 1. Build > Clean Project 후에 Build > Rebuild Project
## 2. File > Invalidate Caches / Restart (캐시비우고 재실행)
## 3. Gradle Refresh
## 4. Preferect > Build, Execution, Deployment > Build Tool > Gradle > Build and Run(Build and run using과 Run tests using을 Gradle을 IntelliJ IDEA로 변경)          
## 5. 위 방법으로도 해결이 안되면 IDE를 최신버전으로 업데이트하자.
