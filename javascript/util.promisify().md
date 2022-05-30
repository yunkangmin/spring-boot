node.js의 내장함수인 util 모듈 중에 promisify라는 함수가 있다.  
이 것을 쓰면, 비동기로 돌리려는 함수를 promise로 감싸주지 않고 사용할 수 있다.   
그래서 지금부터 이 Promise를 조금 더 잘 다룰 수 있도록 하는 util.promisify()  
함수를 소개해보려고 한다.
  
hello.txt  
```
안녕하세요~!
```
util.js  
```
const fs = require('fs');

fs.readFile('./hello.txt', 'utf-8', (err, result) => {
  //안녕하세요~!
  console.log(result);
});
```
위와 같이 hello.txt와 utiltest.js 파일이 있다.   
util.js은 node.js 내장함수인 fs 모듈을 이용하여 hello.js 파일의 내용을 가져와  
console로 찍는 작업을 한다.    
  
파일을 다루는 fs 함수는 비동기 방식으로 처리하기 때문에, fs 함수를 쓴다면, 보통  
아래와 같은 구조로 작성하여 사용한다.  
```
const fs = require('fs');

const getFileData = function(filePath, callback) {
  fs.readFile(filePath, 'utf-8', (err, result) => {
    if (result === undefined) {
      callback(err, null);
    } else {
      callback(null, result);
    }
  });
};
```
그리고 위 코드의 getFileData 함수는 아래와 같이 사용한다.   
```
//fs 함수 처리 후 작업을 비동기 함수로 전달
getFileData('./hello.txt', (err, result) => {
  if (err) {
    console.log(err);
  } else {
    //안녕하세요~!
    console.log(result);
  }
});
```
그리고 우리는 이 콜백함수를 보내는 것을 피하기 위해 프로미스를 사용했다.   
```
const getDataFromFile = function(filePath) {
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, 'utf-8', (err, result) => {
      if (result === undefined) {
        reject(err);
      } else {
        resolve(result);
      }
    });
  });
};

// 프로미스로 호출해 사용
getDataFromFile('./hello.txt').then(data => {
  console.log(data);
});
```
위 코드의 getDataFromFile 함수는 프로미스를 반환하는 방식으로 코드를  
작성해서 사용한다.  
  
이 getDataFromFile 함수를 util.promisify() 함수를 이용하여, 프로미스 코드를   
작성하지 않고, 프로미스로 만들 수 있다.  

# util.promisify() 사용법
```
// util 함수 선언
const util = require('util');
const fs = require('fs');

const getDataFromFile = functioin(filePath, callback) {
  fs.readFile(filePath, 'utf-8', (err, result) => {
    if (result === undefined) {
      callback(err, null);
    } else {
      callback(null, result);
    }
  });
};
```
아까 콜백으로 넘기던 방식의 getDataFromFile 함수가 있다. 이것을 util.promisify()  
함수의 매개변수로 넣어주면 된다.  
```
//getDataFromFile 함수를 프로미스 방식으로 변환해준다.
const readFile = util.promisify(getDataFromFile);

readFile('./hello.txt', 'utf-8')
  .then(result => {
    //안녕하세요~!
    console.log(result);
});
```
이렇게 하면 getDataFromFile 함수가 프로미스 방식으로 변환되어 사용할 수 있다.  
  
변환시키려는 함수는 꼭 getDataFromFile과 같은 구조여야 한다.  
```
const Func = function(inputData, callbakc) {
  somethingAsyncFunction(() => {
    if (inputData === undefined) {
      //비동기 함수 레어 발생 시 처리하는 부분
      callback('err', null);
    } else {
      //비동기 함수가 정상 처리시 데이터 넘겨줄 부분
      callback(null, inputData);
    }
  })
}

Func('hello~').then(data => {
  //hello~
  console.log(data);
}
```
추가로 fs와 같은 함수를 포함한 몇 가지는 위처럼 구조를 만들어 주지 않고도   
바로 쓸 수 있다.  
```
const util = require('util');
const fs = require('fs');

const readFile = util.promisify(fs.readFile);

readFile('./hello.txt', 'utf-8').then(result => {
  console.log(result);
});
```






















