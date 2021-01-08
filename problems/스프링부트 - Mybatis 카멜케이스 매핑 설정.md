# 스프링부트 - Mybatis 카멜케이스 매핑 설정

## 문제
- mybatis-spring-boot-starter로 스프링부트 환경에서 mybatis 사용, DB에서 데이터는 받아오지만 매핑 객체에 특정 프로퍼티만 매핑이 안되는 문제 발생
- db 컬럼명은 snake case, 객체 프로퍼티명은 camel case이므로 매핑 설정이 필요함

## 해결
application.properties에 설정 추가
```properties
mybatis.configuration.map-underscore-to-camel-case=true
```
