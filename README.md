# 스프링 부트 연습

### 프로젝트 구성
* 자바 11
* boot 2.4.0
* h2 데이터베이스 1.4.199
* querydsl
* spring data jpa
* lombok
* spring security5
* mybatis

## 타임리프 레이아웃 재사용 라이브러리
* [링크](https://ultraq.github.io/thymeleaf-layout-dialect/)

## HTML5 BolierPlate
* [링크](https://html5boilerplate.com)

## 챕터별 설명
 - 챕터 0 : 시큐리티 맛보기
    - 기본적으로 시큐리티 설정 후 부트 실행시 하나의 사용자 정보를 가지로록 세팅되어 있음
       - 이때 아이디는 'user'이며 비번은 다음과 같이 부트실행시 로그에 출력됨
         - Using generated security password: e6763f16-aa00-40aa-8adc-ad6df9457c90
    - 인메모리 인증, jdbc를 이용한 인증 구현은 SecurityConfig 클래스 참고
    - 모든 인증매니저는 결과적으로 UserDetails 타입의 객체를 반환
       - 인증 매니저를 커스터마이징하려면 UserDetailsService 인터페이스를 구현하여 사용
       - 구현은 UserService 클래스 참고
    - UserDetail 타입의 User 클래스를 상속받아서 인증관련 정보를 담을 객체를 커스터 마이징 가능하다.
       - 구현은 vo패키지의 SecurityUser 클래스를 참고
    - 스프링 시큐리티 정보를 Thymeleaf에서 사용하기 위해서는 Spring Security Dialect를 사용해야한다
       - [링크](https://github.com/thymeleaf/thymeleaf-extras-springsecurity)
       - 사용하고 있는 시큐리티 버전에 맞춰서 mvnrepository에서 다운 받는다
       - 이때 스프링 시큐리티 네임스페이스는 sec이다
          - 사용 예제는 admin.html 참고
    - remember-me 처리
       - 쿠키에 저장된 사용자 로그인 정보를 이용하여 유저정보 입력없이 자동로그인하기
       - 로그인 폼에 name 프로퍼티 값이 remember-me인 체크박스를 추가해줘야함
       - 설정 내용은 SecurityConfig 참조
       - 쿠키가 아닌 데이터베이스를 이용해서 구현할수도 있음
          - 이미 구현된 JdbcTokenRepositoryImpl을 그대로 사용하는 방법이 있음
              - 이때 생성해야하는 테이블은 security.sql을 참고
          - PersistenceTokenRepository를 구현하는 방법이 있음 [참고](https://github.com/spring-projects/spring-security/blob/master/web/src/main/java/org/springframework/security/web/authentication/rememberme/JdbcTokenRepositoryImpl.java)
              - PersistenceTokenRepository JPA를 사용하여 구현한 것은 RememberMeTokenService를 참고할 것
    - @Secured 어노테이션을 활용하여 컨트롤러등 메소드에 권한을 제한 할 수 있다.
       - @Secured 어노테이션 활성화를 위해 구성 클래스에 @EnableGlobalMethodSecurity(securedEnabled=true)를 설정한다.
    - 암호화를 위한 PasswordEncoder를 사용 다음과 같은 다양한 구현체가 있으며 여기서는 BCryptPasswordEncodeer를 별도의 빈으로 등록하여 사용
       - AbstractPasswordEncoder
       - BCryptPasswordEncodeer
       - NoOpPasswordEncoder
       - Pbkdf2PasswordEncoder
       - NoOpPasswordEncoder
       - SCryptPasswordEncoder
       - StandardPasswordEncoder
    - 회원가입 처리 구현
          