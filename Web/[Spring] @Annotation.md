# @ Annotation

## @Resource, @Inject, @Autowired

의존 객체 자동 주입(Automatic Dependency Injection)은 자동적으로 스프링이 의존 대상 객체를 찾아 의존성을 주입하는 것을 의미한다.

### @Resource

Java에서 지원하는 어노테이션으로 특정 프레임워크에 종속적이지 않다.

> 이름 -> 타입 -> @Qualifier -> 실패

### @Inject

Java에서 지원하는 어노테이션으로 특정 프레임워크에 종속적이지 않다.

> 타입 -> @Qualifier -> 이름 -> 실패

### @Autowired

Spring에서 지원하는 어노테이션이다.

> 타입 -> 이름 -> @Qualifier -> 실패





### 참고

* [@Resource, @Autowired, @Inject 차이](https://velog.io/@sungmo738/Resource-Autowired-Inject-%EC%B0%A8%EC%9D%B4)