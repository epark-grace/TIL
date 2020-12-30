# 스프링부트에서 JSP 사용하기

## 스프링부트에서 JSP를 사용할 경우 제약 사항
- 스프링부트를 사용한다면 기존의 서버에 `war`로 배포하는 대신 내장 서버를 이용해 `jar`로 배포할 수 있음
- `jar`를 통해 배포할 경우 JSP는 지원하지 않기 때문에, JSP를 사용하고 싶다면 별도의 서버에 `war`로 배포해야함

## 스프링부트 & JSP 설정 방법

### 1. war 패키징 설정

#### Maven 프로젝트
```xml
<!-- pom.xml -->
<packaging>war</packaging>
```

#### Gradle 프로젝트
```groovy
// build.gradle
apply plugin: 'war'
```

### 2. JSP 엔진 의존성 추가

#### Maven 프로젝트
```xml
<!-- pom.xml -->
<dependencies>
    <dependency>
        <groupId>org.apache.tomcat</groupId>
        <artifactId>jasper</artifactId>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

#### Gradle 프로젝트
```groovy
// build.gradle
dependencies {
    providedRuntime 'org.apache.tomcat.embed:tomcat-embed-jasper'
}
```

### 3. JSP 경로 설정
```properties
# application.properties
spring.mvc.view.prefix=/WEB-INF/jsp경로
spring.mvc.view.suffix=.jsp
```

### 4. 메인클래스 `SpringBootServletInitializer` 상속하고 `configure` 메소드 오버라이딩
```java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```
