# @Valid 사용해서 DTO 검증

## 1. @Valid란?

**@Valid**는 JSR-303 표준 스펙(Java Bean Validation)을 기반으로 동작하며, 객체의 필드에 선언된 제약 조건(@NotNull, @NotBlank 등)을 검증 <br>
Spring Boot에서는 이를 활용하여 API 요청 데이터의 유효성을 간단히 확인 가능

### 1-1. @Valid의 특징

- **유효성 검증** : DTO 필드에 선언된 제약 조건을 기반으로 데이터를 검증

- **Cascading Validation** : 중첩된 객체의 필드까지 재귀적으로 검증

- **컨트롤러 레벨에서 기본 동작** : 주로 컨트롤러 메서드 매개변수와 함께 사용

<br>

## 2. @Valid 사용법

### 2-1. 의존성 추가

Spring Boot 프로젝트에서 @Valid를 사용하려면 `spring-boot-starter-validation` 라이브러리를 추가

    implementation 'org.springframework.boot:spring-boot-starter-validation'

### 2-2. DTO 정의

DTO 클래스에 유효성 검사를 위한 어노테이션 추가

```java
import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Size;

@Getter
@NoArgsConstructor
public class UserDto {

    @NotBlank(message = "이름은 필수 입력 항목입니다.")
    private String name;

    @Email(message = "유효하지 않은 이메일 형식입니다.")
    private String email;

    @NotNull(message = "나이는 필수 입력 항목입니다.")
    private Integer age;

    @Size(min = 10, max = 200, message = "소개는 10자 이상 200자 이하로 입력해주세요.")
    private String introduction;
}
```

### 2-3. 컨트롤러에서 사용

컨트롤러 메서드 매개변수에 @Valid와 함께 DTO를 선언하면, 요청 데이터가 자동으로 검증

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @PostMapping
    public ResponseEntity<String> createUser(@Valid @RequestBody UserDto userDto) {
        // 검증 성공 시 로직 실행
        return ResponseEntity.ok("사용자 생성 성공");
    }
}
```

<br>

## 3. 중첩 객체 검증

중첩된 객체를 포함하는 DTO를 검증하면, 중첩 객체 필드에 @Valid를 추가

```java
@Getter
@NoArgsConstructor
public class PostCreateRequestDto {

    @NotBlank(message = "제목은 필수 입력 항목입니다.")
    private String title;

    @Valid // 중첩 객체 검증 활성화
    private AuthorDto author;
}

@Getter
@NoArgsConstructor
public class AuthorDto {

    @NotBlank(message = "작성자 이름은 필수 입력 항목입니다.")
    private String name;

    @Email(message = "유효하지 않은 이메일 형식입니다.")
    private String email;
}
```

<br>

## 4. 예외 처리

### 4-1. 기본 예외 처리

유효성 검사 실패 시 Spring은 기본적으로 'MethodArgumentNotValidException을 발생

### 4-2. 커스텀 예외 처리

전역 예외 처리를 통해 사용자 정의 에러 메세지를 반환

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationExceptions(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getAllErrors().forEach(error -> {
            String fieldName = ((FieldError) error).getField();
            String errorMessage = error.getDefaultMessage();
            errors.put(fieldName, errorMessage);
        });
        return ResponseEntity.badRequest().body(errors);
    }
}
```

<br>

## 5. 주요 어노테이션

| 어노테이션     | 설명                                      |
| -------------- | ----------------------------------------- |
| `@NotNull`     | 값이 null이 아니어야 함                   |
| `@NotBlank`    | 문자열이 null이 아니고 공백이 아니어야 함 |
| `@Size`        | 문자열 또는 컬렉션의 크기 제한            |
| `@Min`, `@Max` | 숫자의 최소 및 최대값 설정                |
| `@Email`       | 이메일 형식이어야 함                      |
| `@Pattern`     | 정규식을 만족해야 함                      |

<br>

## 6. 참고 사항

- **@Validated와의 차이점** <br>
  `@Valid`는 JSR-303 표준이며 주로 컨트롤러 레벨에서 동작 <br>
  `@Validated`는 Spring에서 제공하며 서비스 계층에서도 유효성 검사를 지원

- **유효성 검사 실패 시 예외 처리 흐름** <br>
  Controller → Argument Resolver → Validator → Exception Handling

- **Cascading Validation** <br>
  중첩된 객체의 필드를 재귀적으로 검증하려면 상위 객체 필드에 반드시 `@Valid`를 추가
