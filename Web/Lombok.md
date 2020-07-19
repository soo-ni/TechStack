# Lombok

Lombok은 자바 컴파일 시점에서 특정 어노테이션으로 해당 코드를 추가할 수 있는 라이브러리

Java에서 반복적으로 작성되는 getters/setters나 equals, hashCode, toString 또는 생성자 관련 코드들을 간결하게 만들어줌

필드값을 추가삭제 할 때도 getters/setters에 대한 신경을 쓸 필요가 없음
=> 유지 보수에 용이



## Lombok Annotation

| @                        | Contents                                                     |
| ------------------------ | ------------------------------------------------------------ |
| @Data                    | @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor를 한번에 사용 |
| @Getter                  | 필드값에 대한 getters 메소드 자동 생성                       |
| @Setter                  | 필드값에 대한 setters 메소드 자동 생성                       |
| @NoArgsConstructor       | 기본생성자 자동 생성                                         |
| @AllArgsConstructor      | 필드값을 모두 포함한 생성자를 자동 생성                      |
| @ToString                | toString 메소드 자동 생성<br />출력을 원치 않는 필드가 있을 경우, exclude 키워드를 이용 <br />ex) @ToString(exclude = "필드명") |
| @EqualsAndHashCode       | equals와 hashCode 메소드 자동 생성                           |
| @RequiredArgsConstructor | 생성자를 자동 생성하지만, 필드명 위에 @NonNull로 표기된 경우만 생성자의 매개변수로 받음 |





## 참고

* [https://www.popit.kr/%EC%8B%A4%EB%AC%B4%EC%97%90%EC%84%9C-lombok-%EC%82%AC%EC%9A%A9%EB%B2%95/](https://www.popit.kr/실무에서-lombok-사용법/)
* https://siyoon210.tistory.com/24