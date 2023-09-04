---
title: "MSA 아키텍처 API 자동화 Pact Consumer Unit 테스트 코드 개발 및 Broker Server Publish 챕터3"  
date: 2023-08-15
---

Pact Broker Server 구현 Docker-compose.yaml 이궁금하신분들은 아래 링크를 참조해주세요
https://angryfullstack.tistory.com/115

# Pact Consumer DSL(Contract Test JSON)만들기
Pact Contract(.JSON) 파일을 만드는 방법은 Consumer(소비자) 에서 인터페이스로 작업할 API에 대해서
제공하는 언어로 Unit Test를 구현하면됩니다. 

우선 각 개발 언어별 Pact 라이브러리를 사용해서 개발진행하게되는데 이번내용에서는 2가지 언어로 작업에정입니다

## Pact 개념
https://martinfowler.com/articles/consumerDrivenContracts.html  
소비자 주도 계약 이라는 개념에서 시작하였고 자세한 사항은 해당 페이지를 참고하세요!!

[Pact 챕터1 MSA아키텍처 API 자동화 테스트 Contract Test Pact 오픈라이브러리 알아보기](https://angryfullstack.tistory.com/111)



## Pact JVM
공식문서에서 튜토리얼(60분)  
Springboot 진행할수 있는 어플리케이션 워크샵을 제공하고있습니다.  
https://github.com/pact-foundation/pact-workshop-jvm-spring

Java + Springboot 를 사용하는 워크샵에 단계는 아래와같습니다.

step 1: Simple Consumer calling Provider  
step 2: Client Tested but integration fails  
step 3: Pact to the rescue  
step 4: Verify the provider  
step 5: Back to the client we go  
step 6: Consumer updates contract for missing products  
step 7: Adding the missing states  
step 8: Authorization  
step 9: Implement authorisation on the provider  
step 10: Request Filters on the Provider  
step 11: Using a Pact Broker  

```text
공식문서에서 제공하고있는 JVM 워크스테이션은 Pact Contract Test를 진행할수 있는 11단계의 플로우를 공유하고있고
step7 번까지 진행하면 인증테스트를 제외한 기본적인 Rest API 테스트 코드를 직간접적으로 확인이 가능합니다.

step8에는 SpringSecurity 에서 인증을 제공하는 Header 영역에 Authorization 체크하는 부분을 추가하여
Contract Test 하는 기능을 제공합니다.

기본 API테스트까지 필요하신분들은 step7까지 진행하시고 
회원정보, 권한등 인증에대한 API검증도 진행하시려면 step11까지 테스트해보시는것을 권장드립니다. 
```


### 작업전 확인사항
```text
Pact Specification Version 3.0
언어 스팩이 상이할경우 Pact 버전차이로 인해서 정상적으로 처리되지 않는삽질을 할수있기때문에 DSL구성된
.JSON 파일에 metadata 를 꼭 확인하시기 바랍니다.
```
```json
  "metadata": {
    "pactSpecification": {
      "version": "3.0.0"
    },
    "pact-jvm": {
      "version": "4.1.7"
    }
  }
```

### Supported JDK and specification versions:

| Branch                                                                 | Specification | JDK        | Kotlin Version | Latest Version | Notes |
|------------------------------------------------------------------------|---------------|------------|----------------|----------------|-------|
| [4.6.x](https://github.com/DiUS/pact-jvm/blob/v4.6.x/README.md) master | V4 + plugins  | 17+        | 1.8.22         | 4.6.2          |       |
| [4.5.x](https://github.com/DiUS/pact-jvm/blob/v4.5.x/README.md)        | V4 + plugins  | 11+/17+(1) | 1.7.20         | 4.5.8          |       |
| [4.1.x](https://github.com/DiUS/pact-jvm/blob/v4.1.x/README.md)        | V3            | 8-12       | 1.3.72         | 4.1.42         |       |


### Previous versions (not actively supported)

| Branch                                                                    | Specification | JDK       | Kotlin Version | Scala Versions | Latest Version |
|---------------------------------------------------------------------------|---------------|-----------|----------------|----------------|----------------|
| [4.4.x](https://github.com/DiUS/pact-jvm/blob/v4.4.x/README.md)           | V4 + plugins  | 11+       | 1.6.21         | N/A            | 4.4.9          |
| [4.3.x](https://github.com/DiUS/pact-jvm/blob/v4.3.x/README.md)           | V4            | 11+       | 1.6.21         | N/A            | 4.3.19         |
| [4.2.x](https://github.com/DiUS/pact-jvm/blob/v4.2.x/README.md)           | V4 (1)        | 11-15 (2) | 1.4.32         | N/A            | 4.2.21         |
| [4.0.x](https://github.com/DiUS/pact-jvm/blob/v4.x/README.md)             | V3            | 8-12      | 1.3.71         | N/A            | 4.0.10         |
| [3.6.x](https://github.com/DiUS/pact-jvm/blob/v3.6.x/README.md)           | V3            | 8         | 1.3.71         | 2.12           | 3.6.15         |
| [3.5.x](https://github.com/DiUS/pact-jvm/blob/v3.5.x/README.md)           | V3            | 8         | 1.1.4-2        | 2.12, 2.11     | 3.5.25         |
| [3.5.x-jre7](https://github.com/DiUS/pact-jvm/blob/v3.5.x-jre7/README.md) | V3            | 7         | 1.1.4-2        | 2.11           | 3.5.7-jre7.0   |
| [2.4.x](https://github.com/DiUS/pact-jvm/blob/v2.x/README.md)             | V2            | 6         | N/A            | 2.10, 2.11     | 2.4.20         |

### Pact 라이브러리 패키지 구조
```
au.com.dius.pact:consumer
au.com.dius.pact.consumer:groovy
au.com.dius.pact.consumer:junit
au.com.dius.pact.consumer:junit5
au.com.dius.pact.consumer:java8
au.com.dius.pact.consumer:specs2_2.13
au.com.dius.pact:pact-jvm-server
au.com.dius.pact:provider
au.com.dius.pact.provider:scalatest_2.13
au.com.dius.pact.provider:spring
au.com.dius.pact.provider:maven
au.com.dius.pact:provider
au.com.dius.pact.provider:junit
au.com.dius.pact.provider:junit5
au.com.dius.pact.provider:scalasupport_2.13
au.com.dius.pact.provider:lein
au.com.dius.pact.provider:gradle
au.com.dius.pact.provider:specs2_2.13
au.com.dius.pact.provider:junit5spring
au.com.dius.pact.core:support
au.com.dius.pact.core:model
au.com.dius.pact.core:matchers
au.com.dius.pact.core:pactbroker
```

## Pact JS
코로나확인 으로 몸저누움
### Consumer Unit Test 및 Pact DSL(.Json) 파일생성

#### Step1
Pact 에서 제공하는 워크샵을 시작하게되면 우선 git clone 진행이 필요합니다.

consumer, product 2개의 실행 가능한 Multi Moudle이 존재합니다.


