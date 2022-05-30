앞서 설명한 대로 CloudFormation 템플릿은 JSON 형태로 작성한다. 다음은 CloudFormation 템플릿의  
가장 기본적인 형태이다.   
- Description: 템플릿의 설명이다.   
- Parameters: 템플릿으로 스택을 생성할 때 사용자가 입력할 매개변수 목록이다. 숫자의 문자열 형식을   
사용할 수 있다.   
- Resources: AWS 리소스 종류와 옵션, AWS 리소스 간의 관계를 정의한다.   
- Outputs: 스택을 생성한 뒤 출력할 값이다.   
- AWSTemplateFormationVersion: 현재 템플릿 구조의 버전이다. 2014년 4월 기준으로 2010-09-09가 최신  
버전이다. 이 버전이 맞지 않으면 템플릿으로 스택을 만들 수 없으므로 그대로 사용한다.   
  
CloudFormation 기본 템플릿  
```json
{
  "Description": "A text description for the template usage",
  "Parameters": {
    //A set of inputs used to customize the template per deployment
  }, 
  "Resources": {
    //The set of AWS resources and relationships between them
  },
  "Outputs": {
    //A set of values to be made visible to the stack creator
  },
  "AWSTemplateFormatVersion": "2010-09-09"
}
```



























