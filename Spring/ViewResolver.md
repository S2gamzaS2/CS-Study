# View Resolver란?

**뷰 리졸버(View Resolver)**는 Spring MVC 웹 애플리케이션의 중요한 컴포넌트 중 하나로, **MVC 패턴에서 컨트롤러가 처리한 결과를 어떤 뷰로 응답할지 결정**하는 역할을 합니다.

![다운로드 (8)](https://github.com/user-attachments/assets/70659a34-4db5-4cb4-a7b8-40b107f3840f)

---

## View Resolver의 역할

### 1. **뷰 이름 매핑**
컨트롤러는 특정 뷰를 식별하기 위해 **문자열 형태의 뷰 이름**을 반환합니다.  
뷰 리졸버는 이 이름을 실제 뷰 템플릿 파일로 매핑합니다.  
예를 들어, 컨트롤러가 "home"이라는 뷰 이름을 반환하면, 뷰 리졸버는 이를 `home.jsp` 또는 `home.html`로 변환합니다.

### 2. **다양한 뷰 기술 지원**
Spring MVC는 다양한 뷰 기술을 지원합니다.  
이를 위해 여러 **뷰 리졸버**를 제공합니다.  
- JSP, Thymeleaf, FreeMarker 등 다양한 템플릿 엔진을 사용할 수 있습니다.

---

## 주요 View Resolver

| **View Resolver**              | **설명**                                                                                     |
|---------------------------------|---------------------------------------------------------------------------------------------|
| **InternalResourceViewResolver** | JSP와 같은 리소스를 처리하는 가장 일반적인 뷰 리졸버. 접두사와 접미사를 설정하여 경로를 결정.           |
| **UrlBasedViewResolver**         | URL 패턴을 기반으로 뷰를 결정하는 리졸버.                                                    |
| **ResourceBundleViewResolver**   | 뷰 설정 정보를 **프로퍼티 파일**에 저장하고 이를 기반으로 뷰를 결정.                               |
| **XmlViewResolver**              | 뷰 설정 정보를 **XML 파일**에 저장하고 이를 기반으로 뷰를 결정.                                  |
| **BeanNameViewResolver**         | 뷰의 **빈 이름**을 직접 사용하여 뷰를 결정.                                                 |
| **ContentNegotiatingViewResolver** | 클라이언트의 요청 내용(예: Accept 헤더)을 기반으로 적절한 뷰를 선택.                              |

---

<img width="640" alt="다운로드 (9)" src="https://github.com/user-attachments/assets/1a0d19fb-45f2-4bee-8b41-c4f558e2e70b" />

## Spring MVC 뷰 처리 과정

### 1. **핸들러 어댑터 호출**
- 핸들러 어댑터를 통해 `new-form`이라는 **논리 뷰 이름**을 획득합니다.

---

### 2. **ViewResolver 호출**
- **ViewResolver**를 순서대로 호출하여 `new-form`이라는 뷰 이름에 해당하는 뷰를 찾습니다.
  1. **BeanNameViewResolver**:
     - `new-form`이라는 이름의 스프링 빈으로 등록된 뷰를 찾지만, 해당 뷰가 없음.
  2. **InternalResourceViewResolver**:
     - 다음으로 호출되어 처리됩니다.

---

### 3. **InternalResourceViewResolver**
- 이 뷰 리졸버는 **InternalResourceView**를 반환합니다.

---

### 4. **뷰 - InternalResourceView**
- **InternalResourceView**는 JSP와 같은 리소스를 처리할 때 사용됩니다.
- `forward()` 메서드를 호출하여 요청을 JSP로 포워딩합니다.

---

### 5. **`view.render()` 호출**
- `view.render()`가 호출되며, **InternalResourceView**는 `forward()`를 통해 JSP를 실행합니다.

---

