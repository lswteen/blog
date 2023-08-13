---
title: "MSA 아키텍처 API 자동화 Pact Broker Server 구성 방법 Docker-Compose.yaml 만들기 챕터2"  
date: 2023-08-13
---

# Pact Broker Server (Contract Test) 
Pact Broker Server 는 Contract 계약관계를 등록하는 서버입니다.  
[챕터1 Pact Contract 알아보기](https://angryfullstack.tistory.com/111) 에서 설명한것 처럼 Consumer(소비자) Provider(공급자) Server To Server 테스트를 하기위한 
API 정보 및 Contract(계약) 내용을 등록하고 Broker Server 제공하는 Web UI를통해서 등록된 API에 내용 그리고 
각서버별 API 변경정보 및 연동 상태를 확인할수 있는 기능을 제공합니다.

Pact Broker Server 계약정보는 Consumer(소비자) 중심의 계약 및 검증결과를 공유하는 애플리케이션입니다.
Json으로 직렬화 될수 있는 어떤 유형의 계약에도 사용할수 있기때문에 동일한 언어가 아니라도 Contract(계약) 등록된 API정보를 Provider(공급자) 에서 자유로운 언어로 unit 테스토로 검증할수 있습니다.

예시)  
1.Node 로 구성된 Frontend Consumer Pact 계약정보(JSON)를 Broker Server Publish   
2.Java Springboot 구성된 Provider 에서 Consumer에서 등록한 Pact 계약정보(JSON)를 Unit Test Code를 이용해서 검증할수 있습니다.

서로 다른 언어로 구성된 API도 Broker Server 에서 자유롭게 검증, 테스트 가능합니다.

## 주제
- 항목1
- 항목2
- 항목3
    - 항목3-1
    - 항목3-2

### 부주제
부주제에 대한 설명을 추가하세요.

![이미지 제목](이미지URL)

## 주제
- 항목1
- 항목2
- 항목3
  - 항목3-1
  - 항목3-2

## 주제
- 항목1
- 항목2
- 항목3
  - 항목3-1
  - 항목3-2

## 주제
- 항목1
- 항목2
- 항목3
  - 항목3-1
  - 항목3-2


