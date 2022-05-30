# 정리
- 아이템(로우)의 속성들을 업데이트   
- 아이템을 업데이트하거나 저장    
  
데이터를 조회하는 방법은 2가지이다. 쿼리랑 스캔이다. 쿼리는 파티션    
키를 바탕으로 데이터를 찾는다. 스캔은 전체 테이블에서 데이터를    
찾을 때 쓴다.   
쿼리를 잘 쓰려면 파티션 키를 잘 정해야한다. 파티션 키를 통해 데이터를    
분산하고, 조회할 때 쓰기 때문이다. 파티션 키를 음식 종류(type)로    
정했다. 음식 종류에 따라 음식을 찾기 위해서이다. 한식이 500종류, 일식이     
200종류, 양식이 1000종류 있는 경우를 생각해보자. 유저가 50대 한국인    
이어서 한식만 찾는다면 어떤 쿼리를 써야할까? 한식을 키로 두고 쿼리를    
쓰면 된다.    
스캔은 테이블의 모든 아이템을 출력하는 것이고 쿼리는 지정한 조건에    
맞는 내용을 출력하는 것이다.   
스캔은 전체 테이블에서 데이터를 찾을 때 사용한다. 스캔은 데이터 양이     
많을 수록 처리량이 많아서 비효율적이 된다. 그렇지만 필터를 쓰면    
SQL을 쓰는 것처럼 편하게 데이터 조회가 가능한 게 장점이다.    
키를 지정하지 않아도 되는 게 쿼리와의 차이점이다.    
  
---
  
DynamoDB 테이블에 아이템을 저장하는 방법은 다음과 같다.   
- Item: 저장할 아이템을 정의한다. 각 속성의 자료형을 구분해줘야 한다. 아이템이   
있으면 내용을 업데이트하고, 없으면 새로 생성한다.  
   - S, SS: 문자열, 문자열 배열(String Set)이다.  
   - N, NS: 숫자, 숫자 배열(Number Set)이다. 단, JavaScript에서는 문자열 형태로   
   값을 설정해줘야 한다.  
   - B, BS: 바이너리, 바이너리 배열(Binary Set)이다. Buffer 형태로 설정한다.  
- TableName: 아이템을 저장할 테이블 이름을 설정한다.   

DynamoDB를 사용하는 방법은 '확장 가능한 NoSQL 분산 데이터베이스를 제공하는   
DynamoDB'를 참조하기 바란다.  
  
dynamodb_1.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

dynamodb = new AWS.DynamoDB();

var params = {
  Item: { //필수  
    someKey1: {
      S: 'Hello String'
    },
    someKey2: {
      SS: ['Hello String 1', 'Hello String 2' ]
    },
    someKey3: {
      N: '1'
    },
    someKey4: {
      NS: [ '1', '2' ]
    },
    someKey5: {
      B: new Buffer([1, 2, 3])
    },
    someKey6: {
      BS: [ new Buffer([1, 2, 3]), new Buffer([4, 5, 6]) ]
    }
  },
  TableName: 'ExampleTable' //필수
};

dynamodb.putItem(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
저장할 아이템을 JSON 형태로 작성한다.  
```
{
  "someKey1": {
    "S": "Hello String"
  },
  "someKey2": {
    "SS": ["Hello String 1", "Hello String 2"]
  },
  "someKey3": {
    "N": "1"
  },
  "someKey4": {
    "NS": ["1", "2"]
  }
}
```
--item 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로 묶어서 설정한다.   
AWS CLI  
```
$ aws dynamodb put-item --table-name ExampleTable --item '{"someKey1": {"S":"Hello String"}, "someKey2": {"SS": ["Hello String 1", "Hello String 2" ]}, "someKey3": {"N": "1"}, "someKye4": {"NS": ["1", "2"]}}'
```
DynamoDB 테이블에 아이템을 업데이트하는 방법은 다음과 같다.   
- Key: 업데이트할 아이템의 검색 키를 설정한다. 문자열뿐만 아니라 문자열 배열,  
숫자, 숫자 배열, 바이너리, 바이너리 배열도 설정할 수 있다.   
- TableName: 아이템을 업데이트할 테이블 이름을 설정한다.  
- AttributeUpdates: 업데이트할 속성 및 값을 설정한다.  
   - Action: 속성 업데이트 동작이다.  
      - PUT: 아이템이 있으면 내용을 교체하고, 없으면 새로 생성한다.  
      - ADD: 숫자, 숫자 배열만 사용할 수 있다. 숫자이고 아이템이 있으면   
      값을 더한다. 숫자 배열이고 아이템이 있으면 배열에 원소를 추가한다.  
      아이템이 없으면 새로 생성한다.   
      - DELETE: 속성을 삭제한다.  
  - Value: 문자열, 문자열 배열, 숫자, 숫자 배열, 바이너리, 바이너리 배열을  
  사용할 수 있다.   
  
dynamodb_2.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

dynamodb = new AWS.DynamoDB();

var params = {
  Key: { //필수(이 키로 검색을 한다)
    someKey1: {
      S: 'Hello String'
    }
  },
  TableName: 'ExampleTable', //필수
  AttributeUpdates: { //필수
    someKey2: {
      Action: 'PUT',
      Value: {
        SS: [ 'Hello String 10', 'Hello String 20' ]
      }
    },
    someKey3: {
      Action: 'ADD',
      Value: {
        N: '10'
      }
    },
    someKey4: {
      Action: 'DELETE'
    },
    someKey5: {
      Action: 'PUT',
      Value: {
        B: new Buffer([10, 20 ,30])
      }
    },
    someKey6: {
      Action: 'DELETE'
    }
  }
};

dynamodb.updateItem(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```

검색할 키를 JSON 형태로 작성한다.   
```
{
  "someKey1": {
    "S": "Hello String"
  }
}
```
  
업데이트할 아이템을 JSON 형태로 작성한다.   
```
{
  "someKey2": {
    "Action": "PUT",
    "Value": {
      "SS": [ "Hello String 10", "Hello String 20" ]
    }
  },
  "someKey3": {
    "Action": "ADD",
    "Value": {
      "N": "10"
    }
  },
  "someKey4": {
    "Action": "DELETE"
  }
}              
```
--key, --attribute-updates 옵션에 작성한 JSON을 한 줄로 만들고, ''(작은  
따옴표)로 묶어서 설정한다.   
  
AWS CLI  
```
$ aws dynamodb update-item --table-name ExampleTable --key '{"someKey1": { "S": "Hello String" }}' --attribute-updates '{"someKey2": {"Action": "PUT", "Value": {"SS": [ "Hello String 10", "Hello String 20" ]}}, "someKey3": { "Action": "ADD", "Value": {"N": "10" }}, "someKey4": { "Action": "DELETE" }}'
```
DynamoDB 테이블에 **여러 아이템**을 **저장하고 업데이트하는**(한번에) 방법은  
다음과 같다.    
- RequestItems: 저장, 업데이트할 아이템 목록이다. 테이블 이름을 키로 설정한다.   
   - DeleteRequest: 조건에 맞는 아이템(로우)을 삭제한다.   
      - Key: 삭제할 아이템의 검색 키이다. 문자열뿐만 아니라 문자열 배열,   
      숫자, 숫자 배열, 바이너리, 바이너리 배열도 설정할 수 있다.
   - PutRequest: 아이템을 저장한다. 키에 맞는 아이템이 있으면 업데이트하고,   
   없으면 새로 생성한다.  
      - Item: 저장할 아이템을 설정한다. 문자열뿐만 아니라 문자열 배열, 숫자,  
      숫자 배열, 바이너리, 바이너리 배열도 설정할 수 있다.  
      
dynamodb_3.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

dynamodb = new AWS.DynamoDB();

var params = {
  RequestItems = { //필수
    'ExampleTable': [
      {
        DeleteRequest: {
          Key: { //필수
            someKey1: {
              S: 'Hello String'
            }
          }
        }
      },
      {
        PutRequest: {
          Item: { //필수
            someKey1: {
              S: 'World String'
            }
          }
        }
      }
    ]
  }
};

dynamodb.batchWriteItem(params, function (err, data) {  
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});            
```
저장, 업데이트할 아이템을 JSON 형태로 작성한다.  
```
{
  "ExampleTable" : [
    {
      "DeleteRequest": {
        "Key": {
          "someKey1": {
            "S": "Hello String"
          }
        }
      }
    },
    {
      "PutRequest": {
        "Item": {
          "someKey1": {
            "S": "World String"
          }
        }
      }
    }
  ]
}   
```
--request-items 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로   
묶어서 설정한다.   
  
AWS CLI  
```
$ aws dynamodb batch-write-item --request-items '{'ExampleTable": [{"DeleteRequest": {"Key": {"someKey1": {"S": "Hello String"}}}}, {"PutRequest": {"Item": {"someKey1": {"S": "World String" }}}}]}'
```
DynamoDB 테이블에서 아이템을 가져오는 방법은 다음과 같다.  
- Key: 가져올 아이템의 검색 키를 설정한다. 문자열뿐만 아니라 문자열 배열,    
숫자, 숫자 배열, 바이너리, 바이너리 배열도 설정할 수 있다.   
- TableName: 아이템을 가져올 테이블 이름을 설정한다.   
- ConsistentRead: Strongly Consistent Read를 사용한다. false로 설정하거나   
이 옵션을 설정하지 않으면 Eventually Consistent Read를 사용한다.   
  
dynamodb_4.js   
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

dynamodb = new AWS.DynamoDB();

var params = {
  Key: { //필수
    someKey1: {
      S: 'Hello String'
    }
  },
  TableName: 'ExampleTable', //필수
  ConsistentRead: true
};

dynamodb.getItem(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
다음과 같이 가져올 아이템의 검색 키를 JSON 형태로 작성한다.  
```
{
  "someKey1": {
    "S": "Hello String"
  }
}
```
--key 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로 묶어서 설정한다.   
  
AWS CLI   
```
$ aws dynamodb get-item --table-name ExampleTable --key '{"someKey1": {"S": "Hello String" }}' --no-consistent-read
$ aws dynamodb get-item --table-name ExampleTable --key '{"someKey2": {"S": "Hello String"}}' --consistent-read
```
DynamoDB 테이블에서 여러 아이템을 가져오는 방법은 다음과 같다.   
- RequestItems: 가져올 아이템 목록이다. 테이블 이름을 키로 설정한다.   
   - Keys: 가져올 아이템의 검색 키를 배열 형태로 설정한다. 문자열뿐만 아니라  
   문자열 배열, 숫자, 숫자 배열, 바이너리, 바이너리 배열도 설정할 수 있다.   
- ConsistentRead: Strongly Consistent Read를 사용한다. false로 설정하거나  
이 옵션을 설정하지 않으면 Eventually Consistent Read를 사용한다.  
  
dynamodb_5.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

dynamodb = new AWS.DynamoDB();
var params = {
  RequestItems: { //필수
    'ExampleTable': {
      Keys: [ //필수
      {
        someKey1: {
          S: 'Hello String'
        }
      },
      {
        someKey1: {
          S: 'World String'
        }
      }
    ],
    ConsistentRead: true
    }
  }
};

dynamodb.batchGetItem(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else 
    console.log(data);
});
```
가져올 아이템의 검색 키 목록을 JSON 형태로 작성한다.  
```
{
  "ExampleTable": {
    "Keys": [
      {
        "someKey1": {
          "S": "Hello String"
        }
      },
      {
        "someKey": {
          "S": "World String"
        }
      }
    ],
    "ConsistentRead": "true"
  }
}
```
--request-items 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로 묶어서  
설정한다.   
AWS CLI   
```
$ aws dynamodb batch-get-item --request-items '{"ExampleTable": {"Keys": [{"someKey1": {"S": "Hello String" }}, {"someKey1": {"S": "World String" }}], "ConsistentRead": "true"}}'
```

DynamoDB 테이블에 쿼리하는 방법은 다음과 같다.  
- KeyConditions: 검색할 키를 정의한다.  
   - ComparisonOperator: 검색 조건이며 EQ, NE, IN, LE, LT, GE, GT, BETWEEN,   
   NOT_NULL, NULL, CONTAINS, NOT_CONTAINS, BEGINS_WITH가 있다. EQ를 제외한  
   다른 조건들은 범위 키(Range Key)에서 사용할 수 있다.  
   - AttributeValueList: 검색할 키 목록을 배열 형태로 설정한다.   
- TableName: 검색할 테이블의 이름을 설정한다.   
- QueryFilter: 검색 결과를 설정한 조건대로 걸러내는 옵션이다.  
   - 검새조건이며 EQ, NE, IN, LE, LT, GE, GT, BETWEEN, NOT_NULL, NULL,   
   CONTAINS, NOT_CONTAINS, BEGINS_WITH가 있다.  
   - AttributeValueList: 걸러낼 속성 목록을 배열 형태로 설정한다.   
- ConsistentRead: Strongly Consistent Read를 사용한다. false로 설정하거나   
이 옵션을 설정하지 않으면 Eventually Consistent Read를 사용한다.  
  
dynamodb_6.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

dynamodb = new AWS.DynamoDB();

//someKey1 키의 값이 'Hello String'인 아이템 중에서 somekey3 키의 값이  
// 1인 아이템을 조회
var params = {
  KeyConditions: { //필수
    someKey1: {
      ComparisonOperator: 'EQ',
      AttributeValueList: [
        {
          S: 'Hello String'
        }
      ]
    }
  },
  TableName: 'ExampleTable', //필수
  QueryFilter: {
    someKey3: {
      ComparisonOperator: 'EQ',
      AttributeValueList: [
        {
          N: '1'
        }
      ]
    }
  },
  ConsistentRead: true
};

dynamodb.query(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
검색할 키를 JSON 형태로 작성한다.   
```
{
  "someKey1": {
    "ComparisonOperator": "EQ",
    "AttributeValueList": [
      {
        "S": "Hello String"
      }
    ]
  }
}
```
걸러낼 키를 JSON 형태로 작성한다.  
```
{
  "someKey3": {
    "ComparisonOperator": "EQ",
    "AttributeValueList": {
      {
        "N": "1"
      }
    ]
  }
}
```
--key-conditions, --query-filter 옵션에 작성한 JSON을 한 줄로 만들고   
''(작은 따옴표)로 묶어서 설정한다.   
  
AWS CLI  
```
$ aws dynamodb query --table-name ExampleTable --key-conditions '{"someKey1": {"ComparisonOperator": "EQ", "AttributeValueList": [{"S": "Hello String"}]}}' --query-filter '{"someKey3":{"ComparisonOperator":"EQ", "AttributeValueList": [{"N":"1"}]}}' --consistent-read
```
  
DynamoDB 테이블에서 스캔하는 방법은 다음과 같다.  
- TableName: 검색할 테이블의 이름을 설정한다.  
- ScanFilter: 검색 결과를 설정한 조건대로 걸러내는 옵션이다.   
   - 검색조건이며 EQ, NE, IN, LE, LT, GE, GT, BETWEEN, NOT_NULL, NULL,   
   CONTAINS, NOT_CONTAINS, BEGINS, WITH가 있다.  
   - AttributeValueList: 걸러낼 속성 목록을 배열 형태로 설정한다.  
  
dynamodb_7.js  
```
var AWS = require('aws-sdk');
AWS.config.loadFromPath('./config.json');

dynamodb = new AWS.DynamoDB();

var params = {
  TableName: 'ExampleTable', //필수
  ScanFilter: {
    someKey3: {
      ComparisonOperator: 'EQ',
      AttributeValueList: [
        {
          N: '1'
        }
      ]
    }
  },
};

dynamodb.scan(params, function (err, data) {
  if (err)
    console.log(err, err.stack);
  else
    console.log(data);
});
```
걸러낼 키를 JSON 형태로 작성한다.  
```
{ 
  "someKey3": {
    "ComparisonOperator": "EQ",
    "AttributeValueList": [
      {
        "N": "1"
      }
    ]
  }
}
```
--scan-filter 옵션에 작성한 JSON을 한 줄로 만들고 ''(작은 따옴표)로   
묶어서 설정한다.  
  
AWS CLI  
```
$ aws dynamodb scan --table-name ExampleTable --scan-filter '{"someKey3":{"ComparisonOperator": "EQ", "AttributeValueList": [{"N": "1"}]}}'
```
다른 함수들의 사용 방법은 다음 링크를 참조하기 바란다.   
  
http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html  
  
  
























