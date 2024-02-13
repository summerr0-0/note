# form login 흐름

### form 로그인 할 때

* client가 특정 페이지에 접근
* 인증이 안된 상태라면 server에서 로그인 페이지로 리다이랙트
* POST방식으로 client 로그인 요청
* spring scurity에서 세션을 생성
    * 세션에 최종 성공한 인증 결과, 인증 결과를 담은 인증 토큰, 인증 객체 생성(security context)가 담긴다
* 인증을 받은 후 클라이언트가 자원에 접근 시도
* 사용자가 가진 세션에 인증 토큰이 있는지 확인
* 세션에 저장된 인증 토큰이 있으면 접근 허가

###  form Login 인증의 API

```java
http
      .formLogin() //로그인 기능 활성화
      .loginPage("loginPage")//로그인 페이지
      .defaultSuccessUrl("/") //성공시 이동
      .failureUrl("/login") //실패시 이동
      .usernameParameter("userId") //default is "username"
      .passwordParameter("password") //default is "password"
      .loginProcessingUrl("/login") //로그인 프로세스 액션 url
      .successHandler(new AuthenticationSuccessHandler() {
          @Override
          public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
              System.out.println("authentication" + authentication.getName());
              response.sendRedirect("/");
          }
      }) //성공할 때 (커스텀)
      .failureHandler(new AuthenticationFailureHandler() {
          @Override
          public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException exception) throws IOException, ServletException {
              System.out.println("exception" + exception.getMessage());
              response.sendRedirect("login");
          }
      }) //실패 할 때 (커스텀)
      .permitAll() //login page로 온 요청은 모두 접근 가능
```



### Login form 인증 할 때 내부 로직
![image_1.png](image_1.png)
1. 사용자 인증 시도
2. `UsernamePasswordAuthenticationFilter` 가 요청을 받음
    * 사용자가 인증 처리를 담당당하고 관련된 요청을 처리한다
3. 요청 url이 `/login` 이 맞는지 확인한다
    * 일치하면 인증 과정 진행
    * Url 정보는 custom하게 변경 가능하다 `loginProcessingUrl`설정으로
    * 일치 하지 않으면 `chain.doFilter` 로 다음 필터로 요청을 넘긴다
4. 사용자 이름과 비밀번호 기반으로 `Authentification` 객체 생성
5. `AuthenticationManager`
    * 인증처리를 맡는 부분
    * 내부적으로 `AuthenticationProvider`가 있음
        * 실질적인 인증처리 담당
        * 이 곳에서 인증의 성공 / 실패를 리턴
        * 실패하면 `AuthenticationException` 발생
6. 인증 성공 후 `Authentication` 객체 생성
    * User, 권한 정보 등
7. 인증 객체 Security Context에 저장
8. 인증 성공 후 successHandler 동작
