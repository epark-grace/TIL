# `@BeforeAll` 메소드 내에서 스프링 빈 사용하기

## 문제
- JUnit5의 `@BeforeAll` 메소드는 static으로 만들어야한다.
- static 메소드에서는 non-static 필드를 사용할 수 없으므로 스프링의 `@Autowired`로 주입받은 객체를 static으로 변경해야한다.
- 그러나 `@Autowired`로 의존객체를 주입받는 시점은 최소한 생성자가 실행될 때(생성자 주입의 경우)이기 때문에 static으로 변경한다해도 static 메소드내에서 해당 필드를 사용하는 시점에는 주입이 되지 않아 NullPointerException이 발생한다.

## 해결
- `@BeforAll` 메소드에서 스프링 ApplicationContext를 파라미터로 받을 수 있다. [참고](https://github.com/junit-team/junit5/issues/455#issuecomment-241397709)
```java
@BeforeAll
static void setUp(ApplicationContext applicationContext) {
  CategoryRepository categoryRepository = applicationContext.getBean(CategoryRepository.class);
  ...
}
```
