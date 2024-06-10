# 인증 아키텍처

#### ## 인증 프로세스의 주요 클래스들

1. 인증 - Authentication
   * 인증을 할때 인증 필터가 로그인 정보를 기반으로 Authentication 객체를 만든다
2. 보안 컨텍스트 - SecurituContest & SecurityContextHolder
   * 인증에 성공한 객체를 저장해 인증상태를 유지한다
3. 인증 관리자 - AuthenticationManager
   * 인증 필터가 인증을 시도할 때 가장 먼저 인증 처리를 맡기는 곳
   * Authentication가 인자로 들어간다
   * 인증 시도를 관리한다 (총괄)
4. 인증 제공자 - AuthenticationProvider
   * AuthenticationManager로부터 인증처리를 다시 위임받는다
   * 인증 시도를 위한 시도를 총괄한다 (실질적 처리)
5. 사용자 상세 서비스 - UserDetailsService
   * 사용자 정보를 가져올 수 있음
6. 사용자 상세 - UserDetails
   * 유저 객체를 담을 수 있는 타입
