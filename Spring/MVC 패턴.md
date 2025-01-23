# [Spring] MVC 패턴

Spring MVC == Spring Web MVC

서블릿(Servlet) API를 기반으로 클라이언트의 요청을 처리

![image.png](images/mvc-image.png)

### MVC란?

- MVC 패턴은 애플리케이션을 설계할 때 사용하는 디자인 패턴
- 애플리케이션의 기능을 Model, View, Controller로 분리하여 각각의 역할에 맞게 구현함으로써 유지보수성과 확장성을 높이는 개발 방식
- 장점
    - UI와 비즈니스 로직을 명확히 분리할 수 있어 상호 영향 없이 독립적으로 개발 및 유지보수 가능

---

### **Model(모델)**

- 클라이언트의 요청에 따라 데이터를 처리하거나 필요한 로직을 수행한 후 결과를 반환하는 역할
- Spring MVC 기반의 애플리케이션에서는 클라이언트의 요청에 대해 서비스 계층(Service Layer)이 구체적인 작업을 수행하며, 이 과정에서 작성된 비즈니스 로직(Business Logic)을 통해 Model 데이터가 생성된다.
- Model 데이터는 요청에 대한 결과를 포함하며, 이후 View에 전달되어 클라이언트에게 응답으로 제공된다.

---

### **View(뷰)**

- View는 Model 데이터를 활용하여 화면에 출력할 리소스를 생성
- 웹 브라우저를 비롯한 다양한 애플리케이션의 인터페이스로 표현될 수 있으며, Spring MVC에서는 다양한 View 기술을 지원
    - HTML 페이지 출력
    - PDF 및 Excel과 같은 문서 생성
    - XML, JSON 형태의 포맷 변환

---

### **Controller(컨트롤러)**

- Controller는 클라이언트 요청을 가장 먼저 받아들이는 역할
- 요청을 적절한 비즈니스 로직에 전달하고, 처리된 Model를 View에 연결하여 응답이 클라이언트로 반환되도록 조정
- 즉, Controller는 Model과 View 사이에서 데이터와 흐름을 관리하는 중재자 역할을 수행한다.

---

### **Spring MVC의 구성 요소 및 동작 원리**

### **Spring MVC의 동작 순서 정리**

1. 클라이언트 요청 → DispatcherServlet 호출
2. Handler Mapping으로 적절한 핸들러 검색
3. 핸들러 어댑터 실행 및 핸들러 호출
4. ModelAndView 생성
5. View Resolver 호출로 뷰 결정
6. View 렌더링 및 클라이언트 응답 반환

### DispatcherServlet

- Spring MVC의 프론트 컨트롤러 패턴 구현체
- Front Controller 패턴은 모든 HTTP 요청을 중앙에서 처리하는 컨트롤러를 통해 적절한 컨트롤러로 요청을 위임하는 설계 방식
    - 클라이언트 요청은 서블릿 컨테이너가 먼저 수신하며, 공통적으로 수행해야 할 작업은 DispatcherServlet이 처리하고, 세부적인 로직은 관련된 컨트롤러로 넘김
- 이전에는 URL 매핑을 위해 모든 서블릿을 web.xml에 수작업으로 등록해야 했지만, DispatcherServlet을 활용하면서 요청 처리가 단순화되었고, web.xml의 의존성을 줄일 수 있게됨

### 처리과정

- 클라이언트 요청 → 서블릿 컨테이너 → DispatcherServlet
- Handler Mapping → 적절한 컨트롤러 호출
- 컨트롤러 처리 결과 반환 → View Resolver로 뷰 결정
- 뷰 렌더링 → HTTP 응답 반환

### **동작 흐름**

1. 클라이언트가 HTTP 요청을 보냄
2. DispatcherServlet이 요청을 가로채고, 이를 적절한 컨트롤러로 라우팅
3. 컨트롤러가 요청을 처리한 후, 데이터를 준비하고 적절한 뷰 이름을 반환
4. View Resolver가 뷰 이름을 실제 뷰 파일로 변환
5. 뷰가 데이터를 렌더링하여 클라이언트에게 응답을 반환

> @RestController의 경우 4,5 번 생략
MessageConverter를 이용
>
