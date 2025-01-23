@Controller vs @RestController
============

## 1. @Controller
> @Controller란?
  - 컨트롤러는 일반적으로 클라이언트 요청에 송수신하여 해당하는 화면으로 연결시켜주는 작업을 해준다.

> @Controller 과정

  ![controller 이미지](/Spring/images/controller.png)
  - 클라이언트가 URI 형태로 요청을 보낸다.
  - DispatcherServlet에서 해당하는 HandlerMapping을 찾는다.
  - HandlerMapping에서 해당 Controller에 위임한다.
  - Controller에서 모든 요청을 처리한 후에 ViewName을 반환한다.
  - DispatcherServlet에서 ViewResolver를 통해 해당하는 View로 클라이언트를 연결시켜준다.

## 2. @RestController
> @RestController란?
  - @Controller에 @ResponseBody가 추가된 어노테이션이다.
  - @ResponseBody가 있기 때문에 반드시 ResponseEntity로 묶어서 하나의 객체 형태로 반환해야한다.

> @RestController 과정

  ![restcontroller 이미지](/Spring/images/restcontroller.png)
  - 클라이언트가 URI 형태로 요청을 보낸다.
  - DispatcherServlet에서 해당하는 HandlerMapping을 찾는다.
  - HandlerMapping에서 해당 Controller에 위임한다.
  - Controller에서 모든 요청을 처리한 후에 ResponseEntity 형태로 반환한다.
  - DispatcherServlet에서 Json 형태로 클라이언트에 반환한다.

> @Controller vs @RestController
  - 사실 @controller로 @RestController 역할을 다하기 위해서는 @ResponseBody만 붙여주면 된다.
  - ViewName만이 아닌 데이터를 반환하기 위해 @ResponseBody를 사용한다.
  - @ResponseBody에서는 ViewResolver가 아닌 HttpMessageConverter가 작동한다.
  - ResponseEntity에서는 JSON 형태의 데이터뿐 아니라 Http 상태 코드도 같이 반환해준다.
  - Front, Back이 나눠지면서 Controller에서 View에 관련된 처리를 하는게 아니라 이것을 확실히 구분해주는 것이 좋다.

> @Controller 사용 방법

  ![controllermethod 이미지](/Spring/images/controllermethod.png)
> @RestController 사용 방법

  ![restcontrollermethod 이미지](/Spring/images/restcontrollermethod.png)

### 참고 사이트
- https://velog.io/@leesomyoung/Spring-Boot-RestController%EC%99%80-Controller%EC%9D%98-%ED%8A%B9%EC%A7%95%EA%B3%BC-%EC%B0%A8%EC%9D%B4%EC%A0%90
- https://backendcode.tistory.com/213
