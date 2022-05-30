## 목표
1. 개발환경 세팅
2. Vue 프로젝트 생성

### 1. 개발환경 세팅
1. Nodejs 설치  
   - 설치이유
      - npm을 사용하기 위해 설치
      - 웹 개발 라이브러리 설치도우미(쉽게 설치 가능)
      - @vue/cli를 쉽게 설치할 수 있다.(@vue/cli는 vue 프로젝트 생성을 빠르게 할 수있는 라이브러리,  
      @vue/cli를 설치하면 vue create 프로젝트명만 입력하면 vue 프로젝트가 생성됨) 
   - 최신 or 최신 LTS 버전 아니면 에러남  
   - 요즘 윈도우/맥 개발과정 똑같음  
   - 디폴트 경로에 설치할 것  
   - 만약 설치하다가 에러가 나면 왼쪽거를 지우고 오른쪽걸로 다시 설치  
[node.js 사이트](https://nodejs.org/ko/)에 접속    
![image](https://user-images.githubusercontent.com/33191974/147869968-10706d3e-9648-4498-a824-b792d6e8fccb.png)    
![image](https://user-images.githubusercontent.com/33191974/147870137-e66a562b-a7cb-47a0-8aa6-2251e4ff2cd3.png)    
2. [VScode 에디터 설치](https://nanci.tistory.com/161)  
3. VSCode 터미널 세팅  
![image](https://user-images.githubusercontent.com/33191974/147873636-a0fdc4d9-dac8-4d2d-b9ca-eb4b41ce284f.png)
아래와 같이 치고 엔터를 누른다.  
아래 명령어는 vuecli를 설치하고 vuecli는 vue 개발환경 세팅을 도와주는 프로그램이다.  
![image](https://user-images.githubusercontent.com/33191974/147870274-2992635e-1feb-490f-b4e2-066dfa425137.png)   
아래처럼 나오면 설치는 잘 된것이다.  
![image](https://user-images.githubusercontent.com/33191974/147870963-45714ba0-3e7c-453b-b008-1cbbc72fe2ad.png)  
설치가 잘 되었는지 확인하려면 vue나 vue info 명령어를 입력하여 아래와 같이 나온다면 잘 된 것이다.   
![image](https://user-images.githubusercontent.com/33191974/147871002-d9e7291f-6807-4342-93b0-c8b52a22468d.png)  
4. VSCode 부가기능 설치하기  
![image](https://user-images.githubusercontent.com/33191974/147870321-3de69927-3a65-4e82-a919-57414625fca0.png)     
   - vetur 설치  
   ![image](https://user-images.githubusercontent.com/33191974/147870350-5f548e6e-3eea-4a9a-9468-54e4ff428d1b.png)  
   - HTML CSS Support 설치  
   ![image](https://user-images.githubusercontent.com/33191974/147870390-28e31793-2c37-471b-8b16-981c6213ef87.png)  
   - Vue 3 Snippets(vue 자동완성 기능)  
   ![image](https://user-images.githubusercontent.com/33191974/147870442-99fd5906-a647-4bed-aaf6-30a52686d95e.png)    
   
## Vue 프로젝트 세팅  
c드라이브 밑에 vue라는 폴더를 만든다.  
그리고 VSCode에서 아래와 같이 vue 폴더를 연다.  
![image](https://user-images.githubusercontent.com/33191974/147871085-3434e1e3-c9a6-4e09-b929-f0594b5ad0b2.png)  
아래와 같이 뜨면 왼쪽 버튼을 누른다(해석 : 이 폴더에 있는 파일의 작성자를 신뢰합니까?  
코드는 이 폴더에 있는 파일을 자동으로 실행할 수 있는 기능을 제공합니다.  
이러한 파일의 작성자를 신뢰할 수 없는 경우 파일이 악성일 수 있으므로 제한 모드에서  
속하는 것이 좋습니다. 자세한 내용은 문서를 참조하세요.)  
![image](https://user-images.githubusercontent.com/33191974/147871120-9bb8dfce-2b97-46e0-a1fd-06c24caaede9.png)   
아래와 같이 왼쪽 목록에 파일명이 보이면 잘 열린 것이다.   
![image](https://user-images.githubusercontent.com/33191974/147871164-52c8c6ee-7d33-4f40-8bd2-61f54b6d8b51.png)  
터미널을 열어서 경로가 아래와 같이 보이는지 확인한다.  
![image](https://user-images.githubusercontent.com/33191974/147871187-ea47704e-fa1c-4991-ad83-1aa8ed4e9adc.png)  
아래와 같이 입력한다.  
![image](https://user-images.githubusercontent.com/33191974/147871223-54810954-a9fd-400a-bc25-a412b86833b7.png)   
아래와 같이 선택창이 나오면 vue3을 선택한 뒤 enter키를 누른다.  
![image](https://user-images.githubusercontent.com/33191974/147871256-815f01bd-f875-4220-b2a9-04794cc5d9ee.png)  
아래와 같이 나오면 설치가 잘된 것이다.  
![image](https://user-images.githubusercontent.com/33191974/147871313-770c2b8a-aca4-47e3-8c43-f4bba55b06f5.png)    
왼쪽에 보면 새로운 새로운 프로젝트가 생성되었다. 이 폴더를 open folder를 이용해 다시 에디터로 연다.  
아래와 같이 굵은 글씨로 뜨면 잘 열린 것이다.   
![image](https://user-images.githubusercontent.com/33191974/147871386-326efdd2-3a77-4716-9773-ae665e0d4db4.png)  
이제 App.vue(메인페이지)에 코드를 짜면 된다.  
웹 브라우저는 vue 파일을 해석할 수 없다.  
.vue파일을 .html 파일로 컴파일한 뒤 그 html 파일을 브라우저에서 여는 것이다.  
vue파일에 짠 코드를 .html에 넣는 식으로 동작한다.  
이 동작은 main.js가 담당한다.  
node_modules는 프로젝트에 쓰이는 라이브러리들이다.  
src는 소스코드를 담는 곳이다.  
public은 html 파일, 기타파일을 보관한다.  
package.json은 우리가 설치한 라이브러리 버전, 프로젝트 설정을 기록해놓은 파일이다.  
![image](https://user-images.githubusercontent.com/33191974/147871900-9ab0251c-cf71-484a-a175-68ee7a9883f4.png)  
<template> 안에는 HTML을 짜고 <script>안에는 js짜고 <style>안에는 css를 짜면 된다.  
![image](https://user-images.githubusercontent.com/33191974/147871458-5d051102-9299-41fe-abc1-182fee569e2f.png)    
미리보기를 하고 싶으면 터미널에 npm run serve를 치면 된다.  
아래와 같이 나오면 local에 있는 주소를 복사해서 크롬에서 접속하면 된다.     
![image](https://user-images.githubusercontent.com/33191974/147871543-8bfc398d-a9c5-47be-94f9-9e8912d6665a.png)    
코드를 짜고 저장만 하면 바로 적용이 된다.    
![image](https://user-images.githubusercontent.com/33191974/147871587-f6d626c2-d67f-417e-a541-15e4eee2035d.png)


  
  































