# @Bean @Configuration @Component

@Bean vs. @Configuration vs. @Component 의 차이는?

스프링이 개발자 대신 객체를 제어하기 위해서는 객체들이 **Bean**으로 등록되어있어야 한다.

스프링 MVC에서는 `@Controller`, `@Service`, `@Repository` 등으로 Bean으로 등록할 수 있으며, configuration 관련 객체들은 `@Bean`과 `@Component` 로 스프링 컨테이너에 객체를 Bean으로 등록할 수 있다.

## @Bean @Component @Configuration
@Bean은 메소드 레벨에서 선언하며, 반환되는 객체(인스턴스)를 개발자가 **수동**으로 Bean으로 등록하는 annotation이다.

반면 @Component는 클래스 레벨에서 선언함으로써 스프링이 런타임시에 컴포넌트스캔을 하여 **자동**으로 Bean을 찾고(detect) 등록하는 annotation이다.
(ClassPathBeanDefinitionScanner.java)

@Configuration은 외부라이브러리 또는 내장 클래스를 Bean으로 등록하고자 할 경우 사용한다. 1개 이상의 @Bean을 제공하는 클래스의 경우 반드시 @Configuration을 사용한다.

## @Bean @Component @Configuration Annotation
### @Bean
@Target은 METHOD
```java
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Bean {
    //
}
```

### @Component
@Target이 Type으로 지정되어 Class나 Interface에만 선언 가능
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Bean {
    //
}
```

### @Configuration
@Component annotation 포함
```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {
    //
}
```

### 참고
* [Spring Bean과 Componen의 차이](https://velog.io/@albaneo0724/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-Bean%EA%B3%BC-Component%EC%9D%98-%EC%B0%A8%EC%9D%B4)
* [Spring Component와 Configuration의 차이](https://velog.io/@albaneo0724/Spring-Component%EC%99%80-Configuration%EC%9D%98-%EC%B0%A8%EC%9D%B4)
* [Bean과 Component 차이](https://youngjinmo.github.io/2021/06/bean-component/)
