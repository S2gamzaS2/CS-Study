# [Spring] Web MVC 요청 처리 과정(DispatcherServlet을 중심으로)

- Dispatcher Servlet은 Spring MVC 프레임워크의 핵심 클래스
- HTTP 요청을 처리하고 적절한 핸들러(Controller)로 라우팅하는 역할을 수행
- 웹 애플리케이션의 요청-응답 흐름을 관리
- 상속 관계
- 
    (DispatcherServlet -> FrameworkServlet -> HttpServletBean -> HttpServlet -> GenericServlet -> Servlet)
    
- Front Controller 패턴 구현
    
    모든 요청이 단일 진입점을 통과하여 처리
    
- 이외에도 사용하는 디자인 패턴
    - 어댑터 패턴
    - 템플릿 메서드 패턴
    - 전략 패턴
    - 컴포지트 패턴

![DispatcherServlet](./images/DispatcherServlet-image.png)

## DispatcherServlet의 동작 흐름

1. 요청 수신
    - 클라이언트에서 HTTP 요청을 보냄
    - 서블릿 컨테이너가 설정된 포트로 들어오는 요청을 받음
    - web.xml(Servlet 2.x) 또는 WebApplicationInitializer(Servlet 3.x 이상) 등을 참고해 어느 서블릿으로 전달할지 결정
    - 요청이 DispatcherServlet에 매핑되어 있으면, DispatcherServlet.service(...)가 호출됨
    - 내부적으로 HTTP 메서드에 따라 doGet·doPost 등으로 분기하지만, 최종적으로 DispatcherServlet.doDispatch(...)가 핵심 로직 담당
2. doDispatch 메서드: 요청 분배 (Dispatching)
    - HttpServletRequest, HttpServletResponse를 통해 URL, HTTP 메서드, 파라미터, 헤더 정보를 확인
    - 핸들러 선택 → 핸들러 호출 → 뷰 결정 과정을 순차적으로 진행
    - HandlerInterceptor가 등록되어 있으면, 컨트롤러 실행 전 preHandle 메서드가 먼저 호출됨
    - preHandle에서 요청 처리를 중단하고 응답을 직접 반환할 수도 있음
3. 핸들러 탐색 (HandlerMapping)
    - 전략 패턴
    - 등록된 HandlerMapping 목록에서 현재 요청 URL과 매칭되는 핸들러(컨트롤러)를 탐색
    - RequestMappingHandlerMapping은 @Controller, @RequestMapping 기반 매핑 정보를 관리
    - BeanNameUrlHandlerMapping은 Bean 이름을 URL처럼 인식
    - 가장 먼저 일치하는 핸들러를 찾으면 탐색 종료
    - 매핑 실패 시 404 Not Found
    - 매핑 성공 시 어떤 클래스의 어떤 메서드를 호출할지 나타내는 객체(HandlerMethod) 반환
4. 핸들러 호출 (HandlerAdapter)
    - 어댑터 패턴
    - 핸들러 형태(@Controller, HttpRequestHandler 등)에 맞게 호출할 수 있는 HandlerAdapter 선택
    - RequestMappingHandlerAdapter: @RequestMapping 메서드 처리
        
        > @RequestMapping은 클래스파일에 RuntimeVisibleAnnotations 속성으로 등록되어 JVM이 런타임에 동적으로 읽을 수 있음
        > 
    - HttpRequestHandlerAdapter: HttpRequestHandler 구현체 처리
    - SimpleControllerHandlerAdapter: 옛날 Controller 인터페이스 처리
    
    ⇒ 일치하는 어댑터가 없으면 예외
    
    - 핸들러 메서드 실행 전, RequestMappingHandlerAdapter는 HandlerMethodArgumentResolver로 파라미터(@RequestParam, @PathVariable, @RequestBody 등)를 자동 바인딩
    - 컨트롤러 메서드를 리플렉션으로 실제 호출
    - 실행 후 HandlerInterceptor.postHandle로 후처리 가능
5. 핸들러 실행 결과 (Handler method return)
    - Model + View 방식
        - ModelAndView 또는 뷰 이름(String)과 Model 반환
        - 뷰 이름 + 모델 데이터를 통해 렌더링
    - 직접 데이터 반환(@ResponseBody)
        - @RestController 또는 @ResponseBody 사용
        - HttpMessageConverter가 JSON, XML 등으로 직렬화해 HTTP 바디로 직접 작성
        - 뷰 렌더링 없이 곧바로 응답 생성
6. 예외 처리 (HandlerExceptionResolver)
    - 컨트롤러 실행 중 발생한 예외를 DispatcherServlet이 캐치
    - 등록된 HandlerExceptionResolver(ExceptionHandlerExceptionResolver, ResponseStatusExceptionResolver, DefaultHandlerExceptionResolver 등) 순회
    - 처리 가능한 예외이면 적절한 ModelAndView 또는 JSON 응답 반환
    - 처리 불가 시 서블릿 컨테이너가 5xx 에러 응답
7. 뷰 결정 (ViewResolver)
    - 뷰 이름이 있으면 ViewResolver가 실제 뷰 객체로 매핑
        - 예: InternalResourceViewResolver는 JSP 파일 경로를 prefix/suffix로 조합
        - Thymeleaf는 각자 방식으로 템플릿 뷰를 생성
    - 뷰 이름이 없으면 RequestToViewNameTranslator가 URL을 바탕으로 뷰 이름 추론 가능
    - @ResponseBody 또는 @RestController면 뷰 단계를 생략
8. 응답 반환 (Response Return)
    - 렌더링된 결과를 웹 브라우저나 호출한 클라이언트에게 전달
    - 헤더, 쿠키, 세션 정보를 세팅하고 스트림을 닫음
    - 인터셉터가 등록되어 있으면 afterCompletion으로 로그, 리소스 해제 등 후처리
    - HTTP 요청 스레드는 작업 완료 후 스레드 풀로 반환
