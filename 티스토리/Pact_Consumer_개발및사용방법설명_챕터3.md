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

---

## API Consumer Unit Test 및 Pact DSL(.Json) 파일생성
아래 github는 Pact에서 제공하는 기본 예제샘플을 기반으로 테스트 용이하게   
추가하거나 수정하였습니다.

샘플 github : https://github.com/lswteen/pact-workshop-jvm-spring

### Step1
Pact 에서 제공하는 워크샵을 시작하게되면 우선 git clone 진행이 필요합니다.
consumer, product 2개의 실행 가능한 Multi Moudle이 존재합니다.

아래링크로 이동한뒤 git clone 하시거나 git Fork 떠서 본인 github저장소로 옮기셔도됩니다.
여기서 main만이동되기에 필요한 step branch가 필요하다면 fork말고 git clone하셔도됩니다.

https://github.com/pact-foundation/pact-workshop-jvm-spring

```shell
> git clone https://github.com/pact-foundation/pact-workshop-jvm-spring
```

### Step2
pact-workshop-jvm-spring Repository에 main branch 구조를 살펴보면 아래와같습니다.
```text
- consumer
- diagrams
- gradle/wrapper
- provider
.gitignore
LICENSE
README.md
build.gradle
docker-compose.yaml
gradlew
gradlew.bat
settings.gradle
```
docker-compose.yaml 파일에 작성된 docker 컨테이너를 기동해야합니다.  
아래 링크를 참고하세요!

[docker-compose 구동방법](https://angryfullstack.tistory.com/115)

pact-workshop-jvm-spring 에서 제공하는 docker-compose.yaml 파일을 가볍게 살펴보면..
```yaml
version: "3"                                      //버전 정보

services:
  postgres:
    image: postgres                               //이미지명 
    healthcheck:
      test: psql postgres --command "select 1" -U postgres
    ports:
      - "5432:5432"                               //기본 PORT
    volumes:
      - data:/var/lib/postgresql/data             //디스크 초기화로인한 별도의 mount db
    environment:
      POSTGRES_USER: postgres                     //USER 재 정의 필요 
      POSTGRES_PASSWORD: password                 //PASSWORD 재 정의 필요
      POSTGRES_DB: postgres

  broker_app:
    image: pactfoundation/pact-broker             //이미지명
    platform: linux/amd64                         //OS 선택
    links:
      - postgres
    ports:
      - 9292:9292                                 //기본 PORT
    environment:
      PACT_BROKER_PORT: '9292'
      PACT_BROKER_LOG_LEVEL: INFO                 //서버 로그레벨
      PACT_BROKER_SQL_LOG_LEVEL: DEBUG            //SQL 로그레벨
      PACT_BROKER_BASIC_AUTH_USERNAME: jobkorea   //USER 재 정의 필요
      PACT_BROKER_BASIC_AUTH_PASSWORD: 1111       //PASSWORD 재 정의 필요
      PACT_BROKER_DATABASE_USERNAME: postgres
      PACT_BROKER_DATABASE_PASSWORD: password
      PACT_BROKER_DATABASE_HOST: postgres
      PACT_BROKER_DATABASE_NAME: postgres
volumes:
  data: {}
```

### Step3
Unit Test 검증 및 .Json PACT파일 생성  
테스트 코드에는 여러건의 Controller API를 검증하게 구성되어있습니다.

일단 임의로 Controller 에서 제공하는 API 1건을 테스트코드로 직접 구현 해보도록 하겠습니다.
코드는 /products API에 대한 Pact Contract Test를 위한 Unit 코드입니다.

아래 코드가 정상적으로 Success가 떨어지면 
```groovy

..코드생략

task copyPacts(type: Copy) {
	description 'Copies the generated Pact json file to the provider resources directory'

	from('build/pacts/')
	into('../provider/src/test/resources/pacts/')
}

..코드생략

```
pact-workshop-jvm-spring > consumer 멀티모듈 build.gradle파일에 정의된 설정정보   
Pact파일이 생성됩니다.



```java
package au.com.dius.pactworkshop.consumer;

import au.com.dius.pact.consumer.MockServer;
import au.com.dius.pact.consumer.dsl.PactDslWithProvider;
import au.com.dius.pact.consumer.junit5.PactConsumerTestExt;
import au.com.dius.pact.consumer.junit5.PactTestFor;
import au.com.dius.pact.core.model.RequestResponsePact;
import au.com.dius.pact.core.model.annotations.Pact;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.web.client.RestTemplate;

import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import static io.pactfoundation.consumer.dsl.LambdaDsl.newJsonArrayMinLike;
import static org.junit.jupiter.api.Assertions.assertEquals;

@ExtendWith(PactConsumerTestExt.class)
public class ProductConsumerPactOneTest {

    //해더영역 UTF-8 설정
    private Map<String, String> headers() {
        Map<String, String> headers = new HashMap<>();
        headers.put("Content-Type", "application/json; charset=utf-8");
        return headers;
    }

    //consumer, provider 명칭은 Pact broker server에서 식별자로 사용됨
    //
    @Pact(consumer = "FrontendOneApplication", provider = "ProductOneService")
    RequestResponsePact getAllProducts(PactDslWithProvider builder) {
        return builder.given("products exist")      //given value 식별자로 사용
                .uponReceiving("get all products")
                .method("GET")
                .path("/products")
                .matchHeader("Authorization", "Bearer (19|20)\\d\\d-(0[1-9]|1[012])-(0[1-9]|[12][0-9]|3[01])T([01][1-9]|2[0123]):[0-5][0-9]")
                .willRespondWith()
                .status(200)
                .headers(headers())
                .body(newJsonArrayMinLike(1, array ->
                    array.object(object -> {
                        object.stringType("id", "09");
                        object.stringType("type", "CREDIT_CARD");
                        object.stringType("name", "Gem Visa");
                        object.stringType("version","v1");
                    })
                ).build())
                .toPact();
    }

    
    @Test
    @PactTestFor(pactMethod = "getAllProducts")     //@Pact 선언된 method와 동일한 이름
    void getAllProducts_whenProductsExist(MockServer mockServer) {
        Product product1 = new Product();
        product1.setId("09");
        product1.setType("CREDIT_CARD");
        product1.setName("Gem Visa");
        product1.setVersion("v1");

        List<Product> expected = Arrays.asList(product1);

        RestTemplate restTemplate = new RestTemplateBuilder()
                .rootUri(mockServer.getUrl())
                .build();
        List<Product> products = new ProductService(restTemplate).getAllProducts();

        assertEquals(expected, products);
    }
}

```

아래코드가 정상적으로 성공하면 .Json PACT파일이 생성됩니다.  
>./gradlew consumer:test
```shell
(base) renzo@MI-1-renzo1980 pact-workshop-jvm-spring % ./gradlew consumer:test
Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 13s
4 actionable tasks: 4 up-to-date
<-------------> 0% WAITING
> IDLE
(base) renzo@MI-1-renzo1980 pact-workshop-jvm-spring % 

```
pact-workshop-jvm-spring > consumer > pacts 

FrontendOneApplication-ProductOneService.json
```json
{
  "provider": {
    "name": "ProductOneService"
  },
  "consumer": {
    "name": "FrontendOneApplication"
  },
  "interactions": [
    {
      "description": "get all products",
      "request": {
        "method": "GET",
        "path": "/products",
        "headers": {
          "Authorization": "Bearer 1957-08-21T20:33"
        },
        "matchingRules": {
          "header": {
            "Authorization": {
              "matchers": [
                {
                  "match": "regex",
                  "regex": "Bearer (19|20)\\d\\d-(0[1-9]|1[012])-(0[1-9]|[12][0-9]|3[01])T([01][1-9]|2[0123]):[0-5][0-9]"
                }
              ],
              "combine": "AND"
            }
          }
        }
      },
      "response": {
        "status": 200,
        "headers": {
          "Content-Type": "application/json; charset=utf-8"
        },
        "body": [
          {
            "name": "Gem Visa",
            "id": "09",
            "type": "CREDIT_CARD",
            "version": "v1"
          }
        ],
        "matchingRules": {
          "body": {
            "$": {
              "matchers": [
                {
                  "match": "type",
                  "min": 1
                }
              ],
              "combine": "AND"
            },
            "$[*].id": {
              "matchers": [
                {
                  "match": "type"
                }
              ],
              "combine": "AND"
            },
            "$[*].type": {
              "matchers": [
                {
                  "match": "type"
                }
              ],
              "combine": "AND"
            },
            "$[*].name": {
              "matchers": [
                {
                  "match": "type"
                }
              ],
              "combine": "AND"
            },
            "$[*].version": {
              "matchers": [
                {
                  "match": "type"
                }
              ],
              "combine": "AND"
            }
          }
        }
      },
      "providerStates": [
        {
          "name": "products exist"
        }
      ]
    }
  ],
  "metadata": {
    "pactSpecification": {
      "version": "3.0.0"
    },
    "pact-jvm": {
      "version": "4.1.7"
    }
  }
}

```

