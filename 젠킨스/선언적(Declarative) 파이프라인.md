## 선언적(Declarative) 파이프라인

### 선언적 파이프라인의 구조
선언적 파이프라인은 큰 하나의 Block안에 Directive와 Section을 포함하는 코드로 구성되어 있다.  
  
각 섹션에는 Directive, Step, Conditionals가 존재한다. 그렇다면 Block, Section, Directive은 무엇일까?
  
#### Block
블록은 시작과 끝이 {}로 묶인 코드의 묶음이다.

```
pipeline {
  //선언적 문법의 코드
}
```

#### Section
섹션은 전체 파이프라인의 흐름 내에서 특정한 시점에 실행이 필요한 Item을 묶는 방법이다.
- 묶인 Item에는 Directive, Step, Conditionals를 포함시킬 수 있다.

섹션은 또 세가지의 영역으로 구분할 수 있다.

1. stages
   - 파이프라인의 중심 로직을 정의하는 개별 stage의 정의를 묶는다.
      - stage의 모음

2. steps
   - stage의 DSL step을 묶는다.
      - 환경변수와 같이 stage의 다른 아이템으로부터 스텝을 분리하는 역할을 한다.

3. posts
   - stage의 끝이나 파이프라인 실행의 마지막에 실행되거나 확인되야 할 step과 조건값을 묶는다.
```
pipeline {
    agent{ label "node" }
    stages{ //stages
        stage("A"){
            steps{
              echo "================executing A==============" //steps
            }
            post{
                always{
                    echo "================always====================" //steps
                }
                success{
                    echo "================A executed successfully=============="
                }
                failure{
                    echo "================A execution failed==================="
                }
            }
        }
    }
    post { //posts
        always {
            echo "================always======================="
        }
        success{
            echo "================pipeline executed successfully=========="
        }
        failure{
            echo "================pipeline execution failed================="
        }
    }
}
```
...








































