---
title: "[Newwork] Rest? RESRful API?"
tags: Newwork
article_header:
  type: cover
  image:
    src: /screenshot.jpg
---

**Rest?, Restfful?**

​
REST 란 네트워크 아키텍처 원리의 모음 이다.<br/>
웹에 존재하는 모든 자원(이미지, 동영상, DB 자원)에 고유한 URI를 부여해 활용”하는 것으로,<br/> 
자원을 정의하고 자원에 대한 주소를 지정하는 방법론을 의미한다고 한다.<br/>
REST 원리를 따르는 시스템을 RESTful 이라한다.<br/>
​
**원리**
​
인터페이스 일관성 : 일관적인 인터페이스로 분리되어야 한다<br/>
무상태(Stateless)
- 각 요청 간 클라이언트의 콘텍스트가 서버에 저장되어서는 안 된다.<br/>
​
캐시 처리 가능(Cacheable)
- WWW에서와 같이 클라이언트는 응답을 캐싱할 수 있어야 한다.<br/>
- 잘 관리되는 캐싱은 클라이언트-서버 간 상호작용을 부분적으로 또는 완전하게 제거하여 <br/>
  scalability와 성능을 향상시킨다.<br/>
​
계층화(Layered System)<br/>
클라이언트는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지를 알 수 없다.<br/>
중간 서버는 로드 밸런싱 기능이나 공유 캐시 기능을 제공함으로써 시스템 규모 확장성을 향상시키는 데 유용하다.<br/>
​
Code on demand (optional)<br/>
- 자바 애플릿이나 자바스크립트의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여<br/>
  기능을 확장시킬 수 있다.<br/>
​
클라이언트/서버 구조 <br/>
- 아키텍처를 단순화시키고 작은 단위로 분리(decouple)함으로써 <br/>
  클라이언트-서버의 각 파트가 독립적으로 개선될 수 있도록 해준다.
​
**목표**
​
- 구성 요소 상호작용의 규모 확장성(scalability of component interactions)<br/>
인터페이스의 범용성 (Generality of interfaces)<br/>
- 구성 요소의 독립적인 배포(Independent deployment of components)<br/>
- 중간적 구성요소를 이용해 응답 지연 감소, 보안을 강화, 레거시 시스템을 인캡슐레이션 
  ( Intermediary components to reduce latency, enforce security and encapsulate legacy systems ) 
​
​
**디자인 가이드**
​
URL은 직관적으로 만들어야한다.동사보다는 명사를 사용해야 한다.<br/>
CamelCase, snake_case, spinal-case 중에서 가능하면 **spinal-case** <br/>
​
**restful 5가지 설계원칙**
​
1. Resources (URIs)
2. HTTP methods
3. HTTP headers
4. Query parameters
5. Status Codes
​
​
1. Resources
   리소스를 설명할 때 가능하면 동사가 아닌 구체적인 명사를 사용해라
   URI case : CamelCase, snake_case, spinal-case 중에서 가능하면 spinal-case를 사용해라.<br/>
   (구글, 페이팔과 같은 회사에서도 spinal-case를 사용하고 있다.)<br/><br/>
2. HTTP Methods<br/>
   REST는 HTTP 프로토콜을 사용한다. <br/>
   RESTful API는 HTTP 메소드를 사용하여 리소스에 어떤 액션이 수행되는지를 나타낸다.<br/>
​
- GET<br/>
  GET 메소드는 리소스를 가져오는데 사용된다.<br/> 
  GET 요청은 데이터를 가져오기만 해야하며 데이터에 영향을 주면 안된다.<br/>
- HEAD<br/>
  GET과 동일하지만 status line과 header section만 전송한다.<br/>
- POST<br/>
  서버에 데이터를 전달하는데 사용된다.<br/>
- PUT<br/>
  리소스를 업로드된 컨텐츠로 업데이트하는데 사용된다.<br/>
- DELETE<br/>
  리소스를 삭제하는데 사용된다.<br/>
- PATCH<br/>
  자원의 일부를 수정할 때는 PATCH가 목적에 맞는 method다.<br/>
3. HTTP headers<br/>
   HTTP 헤더는 요청 또는 응답 또는 엔티티 바디에 대한 정보를 제공한다.<br/>
   HTTP header에는 4가지 타입이 있다.<br/>
​
General Header : 요청 및 응답 메시지 모두에 포함될 수 있다.<br/>
Client Request Header : 요청 메시지에만 포함 될 수 있다.<br/>
Sever Response Header : 응답 메시지에만 포함 될 수 있다.<br/>
Entity Header : 엔티티 바디에 대한 메타 정보와 관련된 헤더이다.<br/>
​
4. Query parameters<br/>
   Paging : 리턴해야하는 데이터의 양이 많거나 예측하기 어려운경우는 페이징 처리를 하는것이 좋다.<br/><br/>
   
   Filtering : 속성의 기대값을 지정하여 리소스를 필터링한다. <br/>
   하나의 속성에 여러개의 기대값으로 필터링하는것이 가능하며 한번에 여러개의 속성으로 필터링 하는것도 가능하다.<br/><br/>
   
   Sorting : 리소스를 정렬한다. sort 파라미터는 정렬을 수행할 속성의 이름을 포함해야 한다.<br/><br/>
   
   Searching : 검색 파라미터는 Filter와 유사하게 전달되지만 정확한 값이 아니여도 되며<br/>
   대략적으로 일치하기만 하면 된다.<br/><br/>
   
5. Status Code
   RESTFul API에서 적절한 HTTP 상태 코드를 사용하는 것은 매우 중요하다.<br/>
   주로 사용되는 상태 코드는 아래와 같다.<br/>
​
200 - OK<br/>
201- CREATED : 리소스가 생성된 경우<br/>
204 - NO CONTENT : 리소스가 성공적으로 삭제된 경우 혹은 응답 바디가 없는 경우<br/>
304 - NOT MODIFIED : 데이터가 변경되지 않음 (캐시된 데이터를 사용해라)<br/>
400 - BAD REQUEST : 요청이 유효하지 않음<br/>
401 - UNAUTHORIZED : 사용자 인증이 필요함<br/>
403 - FORBIDDEN : 리소스에 대한 접근이 허용되지 않음<br/>
404 - NOT FOUND : 리소스가 없음<br/>
500 - INTERNAL SERVER ERROR : 요청을 처리하는 중 알 수없는 에러가 발생함. API 개발자는 가능하면 이 에러는 피해야한다.<br/>