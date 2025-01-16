# [JAVA] Filter, Interceptor

![image](images/fi-image.png)

### **Filter**

- HTTP 요청과 응답을 가로채어 특정 로직을 처리할 수 있는 매커니즘
- 작동시점
    - 서블릿, 정적 콘텐츠와 같은 리소스로 들어오는 요청이나, 리소스에서 나오는 응답
    - 서블릿 컨테이너(Tomcat)에서 동작하므로 jsp, static file에 대한 요청도 가로챔

### **필터 설정 파일**

```java
@Configuration
public class FilterConfig {

    @Bean
    public FilterRegistrationBean<MyFilter> loggingFilter(){
        FilterRegistrationBean<MyFilter> registrationBean
          = new FilterRegistrationBean<>();

        registrationBean.setFilter(new MyFilter());
        registrationBean.addUrlPatterns("/api/*");// 필터를 적용할 URL 패턴return registrationBean;
    }
}

```

### **Filter interface**

```java
public interface Filter {
//처음 인스턴스화 될 떄 한번 실행default void init(FilterConfig filterConfig) throws ServletException {
    }
// 매 요청, 응답마다 실행void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException;
// doFilter 메서드에서 실행 중인 모든 스레드가 완료된 후에 호출// 또는 타임아웃 후에 메서드를 호출 가능// 호출되면 필터 인스턴스는 더 이상 요청을 처리하지 않음//  주로 리소스를 해제할 떄 사용default void destroy() {
    }
}

```

### **Interceptor**

- HTTP 요청과 응답을 가로채어 특정 로직을 처리할 수 있는 매커니즘
- 작동 시점
    - 요청이 Controller에 전달되기 전,
    - 응답이 클라이언트에게 전송되기 전

- Interceptor 설정 및 등록WebConfig.class 파일에서 인터셉터를 빈으로 등록
    - 자바 설정
    
    ```java
    @Configuration
    @EnableWebMvc
    public class WebConfig implements WebMvcConfigurer {
    
        @Override
        public void addInterceptors(InterceptorRegistry registry) {
    	    registry.addInterceptor(new LocaleChangeInterceptor());
    	    registry.addInterceptor(new ThemeChangeInterceptor()).addPathPatterns("/**").excludePathPatterns("/admin/**");
        }
    }
    
    ```
    
    - XML 설정
    
    ```
    <mvc:interceptors>
    <bean class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor"/>
    <mvc:interceptor>
    	<mvc:mapping path="/**"/>
    	<mvc:exclude-mapping path="/admin/**"/>
    	<bean class="org.springframework.web.servlet.theme.ThemeChangeInterceptor"/>
    </mvc:interceptor>
    </mvc:interceptors>
    
    ```
    
    > WebConfig.class : Servlet WebApplicationContext 설정 클래스, 웹 관련 bean들이 등록됨
    > 

### **인터셉터 로직(Interception)**

HandlerInterceptor를 구현하여 다음과 같은 메서드를 구현할 수 있음

```java
public class CustomInterceptor implements HandlerInterceptor {

  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
          throws Exception {
// 컨트롤러가 호출되기 전에 실행// return true : 로직 수행 후 핸들러에 접근// retrun false : 작업 중단 => 컨트롤러 실행 Xreturn true;
  }

  @Override
  public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
          ModelAndView modelAndView) throws Exception {
//핸들러(컨트롤러) 실행 완료 후 , View 생성 전에 실행
  }

  @Override
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
          throws Exception {
//모든 작업이 완료(뷰 생성 완료)된 후에 실행
  }
}
```

### **주의 : @ResponseBody, ResponseEntity 사용하는 경우**

@ResponseBody 또는 ResponseEntity를 사용한 컨트롤러 메서드는 응답이 HandlerAdapter 내에서 처리되며, 이 시점에서 응답이 이미 작성되고 커밋되기 때문에 postHandle 단계에서 응답을 수정할 수 없음

### **해결 방법 : ResponseBodyAdvice 사용**

응답이 HandlerAdapter에서 처리되기 전에 호출됨. 여기서 추가적인 작업 수행 가능

> ResponseBodyAdvice 설정 방법 : @ControllerAdvice와 함께 빈으로 선언하거나, 특정 RequestMappingHandlerAdapter에 직접 설정
> 

### **인터셉터, 필터는 어떤 상황에 사용?**

- Filter는 Servlet Container 레벨에서 요청을 가로채어 처리
- Interceptor는 Spring MVC 레벨에서 요청을 가로채어 처리
- Security Layer는 가능한 일찍(요청을 받자마자, 가능한 앞단에서) 적용되는 것이 잠재적인 위협을 방지하는 것에 좋음
- 따라서 Interceptor는 Security Layer를 구성하는 데 적합하지 않음
- Spring 공식문서에선 Security Layer 구성으로 Spring Security나 Servlet filter chain을 사용하도록 권장함
