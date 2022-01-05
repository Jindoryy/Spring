# JSP의 유효 범위와 속성

JSP에서 제공하는 session, request, application 객체들은 해당객체에 정의된 유효범위 안에서 서로 공유할 수 있는 특정한 영역을 가지고 있다. 
공유되는 데이터는 attribute(속성)라고 하며 속성을 공유할 수 있는 유효 범위를 scope(영역)이라고 한다.

1. Session :
세션 내장 객체는 세션이 유지되고 있는 범위 안, 즉 session scope안에서 서로 다른 페이지 이라고 할지라도 객체(데이터)들을 공유할 수 있는 속성을 가지고 있으며,
이 속성에 내장된 객체(데이터)는 **세션이 종료되는 순간에 반환**된다.
클라이언트 마다 하나씩 가지고 있는 **개별 저장소**이다. 하지만, 사용자마다 객체를 생성해야 해서 서버 부담이 커지기 때문에 최소한의 데이터만 저장하는게 좋다.
다른 브라우저 작업시 휘발 -> *한 브라우저 내에 1개의 session만 생성*

2. Request :
request내장객체는 클라이언트의 요청이 처리되는 동안에 속성을 사용할 수 있다.
즉 forward 또는 include방식을 이용하는 경우 여러개의 페이지에서도 요청 정보가 계속 유지되므로 request영역의 속성을 여러 페이지에서 공유할 수 있다.

3. Application :
웹 어플리 케이션이 실행되고 있는 동안 속성을 사용할 수 있다. **가장 큰 영역**

4. Page :
page영역은 위에 3가지 영역과 다르게 pageContext내장객체를 통해 접근할 수 있는 영역이다.

--------------

- JSP에서 정의하는 영역은 page, reauest, session, application으로 구성된다. 


|영역|영역객체|속성의 유효 범위|
|:---:|:---:|:---:|
|page|pageContext|해당 페이지가 클라이언트에 서비스를 제공하는 동안에만 유효
|request|request|클라이언트의 요청이 처리되는 동안 유효
|session|session|세션이 유지되는 동안 유효
|application|application|웹 어플리케이션이 실행되고 있는 동안 유효




- 속성과 관련된 메서드들


|리턴 타입|메서드명|기능
|:---:|:---:|:---:|
|Object|getAttribute(String key)|key 값으로 등록되어 있는 속성을 Object타입으로 리턴
|Enumeration|getAttributeNames()|해당 영역에 등록되어 있는 모든 속성들의 이름을 Enumeration타입으로 리턴
|없음|setAttribute(String key, Object obj)|해당 영역에 key값의 이름으로 obj객체를 등록
|없음|removeAttribute(String key)|key값으로 등록되어 있는 속성을 제거
