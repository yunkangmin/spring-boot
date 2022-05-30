CloudSearch 검색 도메인에 데이터를 올리는 방법은 다음과 같다.   
- CloudSearchDomain: 객체를 생성할 때 endpoint에 검색 도메인의 엔드포인트   
주소를 설정한다.  
- contentType: 데이터 형식이다. application/json, application/xml을 사용할   
수 있다.   
- documents: 검색 도메인에 올릴 데이터이다. 문자열을 그대로 설정해도 되고,   
fs.readFile, fs.readFileSync 함수로 얻은 Buffer 형식의 변수를 설정해도 된다.   
  
cloudsearch_1.js   
```
var AWS = require('aws-sdk');
var fs = require('fs');
AWS.config.loadFromPath('./config.json');

cloudsearchdomain = new AWS.CloudSearchDomain({
  endpoint: 'doc-exampledomain2-7fq636cmiddehdtdfpa3d3s454.ap-northeast-1.cloudsearch.amazonaws.com'
});

var params = {
  contentType: 'application/json', //필수
  documents: JSON.stringify([
    {
      'type': 'add',
      'id': '1',
      'version': 1, 
      'lang':'ko',
      'fields': {
        'name': '홍길동',
        'address': '서울시 종로구',
        'phone': '010-1234-5678',
        'rank': '대리',
        'age': 27
      }
    },
    {
      'type': 'add',
      'id': '2',
      'version': 1,
      'lang': 'ko',
      'fields': {
        'name': '이율곡',
        'address': '서울시 성북구',
        'phone': '010-4567-8901',
        'rank': '과장',
        'age': 35
      }
    }
  ])
  //documents:fs.readFileSync('./data.json')
};

cloudsearchdomain.uploadDocuments(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
  
AWS CLI   
```
$ aws cloudsearchdomain --endpoint-url http://doc-exampledomain2-7fq636cmiddehdtdfpa3d3s454.ap-northeast-1.cloudsearch.amazonaws.com upload-documents --content-type "application/json" --documents data.json
```
CloudSearch 검색 도메인에서 데이터를 검색하는 방법은 다음과 같다.  
- CloudSearchDomain 객체를 생성할 때 endpoint에 검색 도메인의 엔드포인트 주소를   
설정한다.  
- query: 검색어이다.  
- sort: 정렬 설정이다. `<필드>` asc, `<필드>` desc 형식이다.  
- return: 결과값에서 지정한 필드만 가져온다. 각 필드를 ,(콤마)로 구분하고  
빈 칸이 있으면 안된다.   
- size: 검색 결과의 개수를 설정한다.  

cloudsearch_2.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

cloudsearchdomain = new AWS.CloudSearchDomain({
  endpoint: 'search-exampledomain-vczqocfxwcndcxuxtdscb4hw5m.ap-northeast-1.cloudsearch.amazonaws.com'
});

var params = {
  query: 'dicaprio', //필수
  sort: 'title desc',
  return 'title, _score, actors',
  size: 3
};

cloudsearchdomain.search(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
AWS CLI  
```
$ aws cloudsearchdomain --endpoint-url http://search-exampledomain-vczqocfxwcndcxuxtdscb4hw5m.ap-northeast-1.cloudsearch.amazonaws.com search --searchquery "dicaprio" --sort "title desc" --return "title, _score, actors" --size 3
```

CloudSearch 자동완성을 사용하는 방법(자동완성으로 검색)은 다음과 같다.   
- CloudSearchDomain 객체를 생성할 때 endpoint에 검색 도메인의 엔드포인트  
주소를 설정한다.  
- query: 검색어이다.  
- suggester: 자동완성(suggester) 이름이다.  
- size: 자동완성 검색 결과의 개수를 설정한다.   
  
cloudsearch_3.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

cloudsearchdomain = new AWS.CloudSearchDomain({
  endpoint: 'search-exampledomain-vczqocfxwcndcxuxtdscb4hw5m.ap-northeast-1.cloudsearch.amazonaws.com'
});

var params = {
  query: 'incep', //필수  
  suggester: 'title', //필수
  size: 3
};

cloudsearchdomain.suggest(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else 
    console.log(data);
});    
```

AWS CLI   
```
$ aws cloudsearchdomain --endpoint-url http://search-exampledomain-vczqocfxwcndcxuxtdscb4hw5m.ap-northeast-1.cloudsearch.amazonaws.com suggest --query "incep" --suggester "title" --size 3
```
다른 함수들의 사용방법은 다음 링크를 참조하자.  
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudSearch.html  
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudSearchDomain.html  














