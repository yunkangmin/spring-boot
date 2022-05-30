# NPM 소개
지금까지는 뷰 CLI로 프로젝트를 구성하고 실행할 때 npm install, npm run dev와 같은 명령어를 사용했다.   
NPM(Node Package Manager)의 정의에 대해서는 02장에서 이미 살펴본 바 있다. 즉, '전 세계 자바스크립트  
라이브러리가 존재하는 공개 저장소'라고 설명했는데, 자바스크립트의 활용 범위가 넓어지면서 자바스크립트  
라이브러리를 쉽게 다운로드 할 수 있는 NPM은 현대 웹 앱에서는 없어서는 안 될 도구이다. 따라서 이러한  
패키지 관리 도구 1개 정도는 자유자재로 다룰 줄 알아야 복잡한 뷰 프로젝트도 쉽게 구성할 수 있다.  
  
이번 절에서는 뷰 CLI를 사용할 때 알고 있으면 좋을 만한 NPM 기능을 살펴보자.  
- NPM 설치 명령어
- 전역 설치 VS 지역 설치
- NPM 커스텀 명령어

# NPM 설치 명령어
NPM에서 가장 흔하게 사용되는 명령어는 npm install이라는 설치 명령어이다. npm install 명령어를 명령 프롬프트  
창에 입력하면 npm 설정 파일(package.json)에 설정된 라이브러리 목록을 다운로드할 수 있다. 앞에서 뷰 CLI로   
webpack-simple 프로젝트를 생성할 때 사용했었다. 여기서 웹팩 추가 설정을 위해 플러그인 라이브러리나 애플리케이션  
로직과 관련된 외부 라이브러리를 추가하려면 어떻게 해야할까? --save 옵션과 --save-dev 옵션을 활용하면 된다.  
  
## npm install --save 옵션과 --save-dev 옵션
--save 옵션과 --save-dev 옵션은 모두 해당 라이브러리를 프로젝트 폴더에 다운로드하는 옵션이다. 두 옵션의 차이점은  
단지 npm 설정 파일의 라이브러리 목록에 설치된 라이브러리 이름이 추가되는 곳만 다르다는 것이다. 다음 코드를 보자  
package.json 파일  
```
"dependencies" : {
  "vue" : "^2.4.4"
},
```
```
"devDependencies" : {
  "babel-core" : "^6.26.0",
  "babel-loader" : "^7.1.2",
  "babel-preset-env" : "^1.6.0",
  "babel-preset-stage-3 : "^6.24.1",
  "cross-env" : "^5.0.5",
  "css-loader" : "^0.28.7",
  "file-loader" : "^1.1.4",
  "vue-loader" : "^13.0.5",
  "vue-template-compiler : "^2.4.4",
  "webpack" : "^3.6.0",
  "webpack-dev-server" : "^2.9.1"
}
```
뷰 CLI로 생성한 webpack-simple 프로젝트의 package.json 파일 일부이다. dependencies 속성에는 뷰 코어 라이브러리가  
추가되어 있고, devDependencies 속성에는 웹팩과 관련된 라이브러리가 추가되어 있다. 이미 짐작했을지 모르겠지만  
dependencies 속성에는 애플리케이션을 동작시키는 데 필요한 라이브러리가 들어가고, devDependencies 속성에는 애플리케이션을   
개발할 때 필요한 라이브러리가 들어간다.   
  
라이브러리를 설치할 때 npm install --save 명령어를 사용하면 dependencies 속성에 라이브러리 이름이 추가되고, npm   
install --save-dev 명령어를 사용하면 devDependencies 속성에 라이브러리가 추가된다.   
```
npm install lodash --save
```
```
"dependencies" : {
  //추가됨
  "lodash" : "^4.17.4",
  "vue" : "^2.4.4"
},
```
앞쪽의 화면과 위 코드는 npm install --save 명령어로 로대시(lodash)라는 자바스크립트 라이브러리를 설치한 결과이다.  
dependencies 속성에 lodash 라이브러리가 버전과 함께 추가되었다.   
  
이번에는 반대로 npm install lodash --save-dev 명령어를 입력해 보자  
```
npm install lodash --save-dev
```
```
"devDependencies" : {
  "babel-core" : "^6.26.0",
  "babel-loader" : "^7.1.2",
  "babel-preset-env" : "^1.6.0",
  "babel-preset-stage-3 : "^6.24.1",
  "cross-env" : "^5.0.5",
  "css-loader" : "^0.28.7",
  "file-loader" : "^1.1.4",
  //추가됨
  "lodash" : "^.4.17.4",
  "vue-loader" : "^13.0.5",
  "vue-template-compiler : "^2.4.4",
  "webpack" : "^3.6.0",
  "webpack-dev-server" : "^2.9.1"
}
```
기본적으로 npm install 명령어를 입력하면 모든 라이브러리를 설치할 수 있지만 이러한 차이점이 있다는 것을  
알아두자.   

# 전역설치와 지역설치
뷰 CLI를 설치할 때 사용한 -global 옵션을 기억해보자. npm install vue-cli -global 명령어를 실행하고  
명령 프롬프트 창에서 vue 명령어를 입력하면 뷰 프로젝트 구성과 관련된 도움말이 표시되었다.  
  
여기서 사용한 -global 옵션은 해당 라이브러리를 시스템 레벨에 설치하는 옵션이다. 방금 전에 살펴본  
--save와 --save-dev 옵션은 해당 프로젝트에 라이브러리 파일을 다운로드한다. 만약 옵션없이 npm install  
명령어만 입력해도 동일하게 해당 프로젝트에 라이브러리 파일을 다운로드 할 수 있다. 이처럼 -global 옵션을  
이용해 시스템 레벨에 설치하는 것을 전역 설치라고 한다. 그리고 --save, --save-dev 같이 해당 프로젝트에  
설치하는 것을 지역 설치라고 한다. 그럼 전역 설치와 지역 설치가 어떻게 다른지 확인해보자.  
  
아래 그림은 새 폴더를 하나 생성한 후 앞에서 사용했던 webpack-simple 프로젝트의 package.json 파일을  
복사해 온 폴더 구조이다.  
npmtest 폴더에 package.json 파일을 추가한 모습  
![image](https://user-images.githubusercontent.com/33191974/149930207-7eb5ec9c-46cf-48c6-843d-8f9ec1710d18.png)  
위 폴더 위치에서 명령 프롬프트 창을 열고 npm install webpack -g 명령어를 입력한다. 참고로 -g는 -global 옵션의   
약어이다. install 명령어 역시 i로 줄일 수 있다. 예를 들면 npm i webpack -g이다.     
![image](https://user-images.githubusercontent.com/33191974/149930506-755d873e-34f2-4595-8096-78321253b4b5.png)  
설치는 제대로 되었는데 앞에서 생성한 폴더에는 아무것도 추가되지 않았다. 왜냐하면 라이브러리를 현재 폴더 위치가   
아닌 전역(시스템 레벨)에 설치했기 때문이다. 삭제는 npm uninstall webpack -g를 입력한다.
  
그럼 이번에는 다음과 같이 npm install webpack 명령어를 입력한다.   

라이브러리를 설치했다는 메시지와 함께 폴더 구조를 살펴보면 다음과 같이 node_modules 폴더가 추가된 것을  
확인할 수 있다. node_modules 폴더를 열어보면 아래에 웹팩과 관련된 라이브러리 파일들이 설치되어 있다.   
![image](https://user-images.githubusercontent.com/33191974/149930954-a1653b72-6cb5-4ebc-92a2-f4b4cf95bcc3.png)  

지금까지 NPM 설치 명령어와 설치 명령어 옵션에 대해서 알아봤다.  

# NPM 커스텀 명령어
뷰 CLI로 구성한 프로젝트를 실행할 때 npm run build와 npm run dev 명령어를 사용했다. npm run build 명령어는 웹팩으로  
프로젝트를 빌드할 때 사용했고, npm run dev 명령어는 프로젝트를 웹팩 데브 서버로 구동할 때 사용했다. 이 명령어들은  
어디에서 튀어나온 것일까? 아래 코드를 살펴보자.   
npm 설정 파일(package.json)의 scripts 속성  
```
"scripts" : {
  "dev" : "cross-env NODE_ENV=development webpack-dev-server --open --hot",
  "build" : "cross-env NODE_ENV=production webpack --progress --hide-modules"
}
```
위 코드는 npm 설정 파일(package.json)의 scripts 속성 코드이다. dev 속성과 build 속성을 보면 여러 가지 옵션값이 들어가  
있는 걸 알 수 있다. dev 속성은 웹팩 데브 서버를 실행하는 명령어와 함께 --open과 같은 추가 옵션들을 주었고, build 속성은   
웹팩 빌드를 실행하는 명령어와 함께 --progress와 같은 추가 옵션들을 주었다. 이 dev 속성과 build 속성이 npm run으로 명령어를  
실행할 때의 대상 속성이다. 따라서 명령 프롬프트 창에 npm run dev 명령어를 입력하면 webpack-dev-server --open --hot   
명령어를 입력한 것과 같은 효과를 얻을 수 있다.  
  
이와 같은 방식으로 npm 설정 파일의 scripts 속성에 원하는 명령어를 추가하고, 해당 명령어를 실행했을 때  
동작하는 옵션들을 정의할 수 있다. 이렇게 하면 매번 긴 명령어를 입력하지 않고 'npm run 명령어' 형식으로 간단하게  
입력하여 실행할 수 있다. 프로젝트가 커지고 웹팩 설정 파일이 복잡해지면 scripts 속성 안에 명령어를 추가하여   
사용해보자.   





















































