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


## Pact JS
dd
