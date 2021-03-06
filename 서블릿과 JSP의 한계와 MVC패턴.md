# 서블릿과 JSP의 한계

- 서블릿으로 개발할 때는 View화면을 위한 HTML과 자바코드가 하나의 코드에 쓰여서 지저분 하고 복잡하다.
하나의 서블릿이나 JSP만으로 비즈니스 로직과 뷰 렌더링까지 모두 처리하면 하나의 코드가 너무많은 역할을 하게된다.
**즉 비즈니스 로직의 변경이 발생해도 해당 코드를 손대야 하고, UI를 변경할 일이 있어도 비즈니스 로직이 함께있는 파일을 수정해야 한다는 것이다.**

<p align="center">
  <img width="40%" src="https://user-images.githubusercontent.com/87755660/148186940-a8172dd5-3b09-48e3-b7a5-e6bd82897a96.png"/>
</p>

- JSP와 HTML이 함께 있는 모습이다. 위의 코드는 짧고 간단해 유지보수가 어렵지 않지만, 
큰 프로젝트에서 HTML 코드하나를 수정해야 하는데 수백 수천줄의 JSP코드가 HTML코드가 함께 있다고 상상하면 쉬운일이 아니다.

# 변경의 라이플 사이클

- 뷰와 비즈니스 로직 코드의 진짜 문제는 둘 사이의 변경의 라이프 사이클이 다르다는 것이다. 
예를 들어서 UI를 일부분 수정하는 것과 비즈니스 로직을 수정하는 일은 각각 다르게 발생할 가능성이 매우 높고 대부분 서로에게 영향을 주지 않는다. 
이렇게 변경의 라이프 사이클이 다른 부분을 하나의 코드로 관리하는 것은 유지보수에 좋지 않다.

# 기능 특화

- JSP 같은 뷰 템플릿은 화면을 렌더링 하는데 최적화 되어있기 때문에 이 부분의 업무만 담당하는 것이 가장 효과적이다.

# MVC 패턴 (Model View Controller)

- MVC패턴은 서블릿이나 JSP로 처리하던 것을 컨트롤러(Controller)와 뷰(View)라는 영역으로 서로 역할을 나눈 것을 말한다. 웹 애플리케이션은 보통 이 MVC패턴을 사용한다.

1. Controller: HTTP 요청을 받아서 파타미터를 검증하고 비즈니스 로직을 실행한다. 그리고 뷰에 전달할 결과 데이터를 조회해서 모델에 담는다.
2. Model: 뷰에 출력할 데이터를 담아둔다. 뷰가 필요한 데이터를 모두 Model에 담아서 전달해주는 덕분에 뷰는 비즈니스 로직이나 접근을 몰라도 되고, 화면을 렌더링 하는 일에만 집중 할 수 있다.
3. View: 모델에 담겨있는 데이터를 사용해서 화면을 그리는 것에 집중한다. 여기서는 HTML을 생성하는 부분을 말한다.

<p align="center">
  <img width="40%" src="https://user-images.githubusercontent.com/87755660/148187874-76186d38-0cf6-4b5b-b326-dc6b36aaf08f.png"/>
</p>

- 클라이언트가 Controller를 호출하면 컨트롤러에서 비즈니스 로직과 데이터에 접근해서 얻어진 데이터를 Model에 전달하고 제어권을 뷰 로직으로 넘긴다. 
뷰에서는 Model에 있는 데이터를 참조해서 뷰를 렌더링하고 HTML응답으로 클라이언트에게 보내준다.

서블릿을 컨트롤러로 사용하고 JSP를 뷰로 사용해서 MVC패턴을 적용할 수 있다.
request.setAttribute(), request.getAttribute() 를 사용하면 데이터를 보관하고 조회할 수 있다. 
View로 제어권을 넘겨줄때는 dispatcher.forward(request, response);를 사용하는데 dispatcher.forward()는 다른서블릿이나 JSP로 이동할 수 있는 기능이다. 서버 내부에서 다시 호출이발생한다.

<p align="center">
  <img width="40%" src="https://user-images.githubusercontent.com/87755660/148188436-f0653463-a558-4611-9eff-c7737cfa864e.png"/>
</p>

- request가 제공하는 setAttribute()를 사용하면 request 객체에 데이터를 보관해서 뷰에 전달할 수 있다. 뷰는 request.getAttribute()를 사용해서 데이터를 꺼내면 된다.

[redirect vs forward]
리다이렉트는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다. 
따라서 클라이언트가 인지할 수 있고, URL경로도 변경된다. 반면에 forward는 서버 내부에서 일어나는 요청이기 때문에 클라이언트가 전혀 인지하지 못한다.

---------------

참고자료 : https://velog.io/@januaryone/Spring-MVC-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC
