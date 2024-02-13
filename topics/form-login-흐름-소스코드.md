# form login 흐름 소스코드

### 상세
```Java
//AbstractAuthenticationProcessingFilter.java

	private void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
			throws IOException, ServletException {
			//요청 url이 맞지 않으면 다음 필터로 넘겨버린다
		if (!requiresAuthentication(request, response)) {
			chain.doFilter(request, response);
			return;
		}
		try {
		    //인증 시도 -> UsernamePasswordAuthenticationFilter로 이동
		    //UsernamePasswordAuthenticationFilter 에서 userName password 추츨해서 Authentication 생성
			Authentication authenticationResult = attemptAuthentication(request, response);
			if (authenticationResult == null) {
				// return immediately as subclass has indicated that it hasn't completed
				return;
			}
			this.sessionStrategy.onAuthentication(authenticationResult, request, response);
			// Authentication success
			if (this.continueChainBeforeSuccessfulAuthentication) {
				chain.doFilter(request, response);
			}
			//성공하면 successfulAuthentication호출
			successfulAuthentication(request, response, chain, authenticationResult);
		}
		catch (InternalAuthenticationServiceException failed) {
			this.logger.error("An internal error occurred while trying to authenticate the user.", failed);
			unsuccessfulAuthentication(request, response, failed);
		}
		catch (AuthenticationException ex) {
			// Authentication failed
			unsuccessfulAuthentication(request, response, ex);
		}
	}

```

```Java
//UsernamePasswordAuthenticationFilter.java
//
	@Override
	public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
			throws AuthenticationException {
		if (this.postOnly && !request.getMethod().equals("POST")) {
			throw new AuthenticationServiceException("Authentication method not supported: " + request.getMethod());
		}
		String username = obtainUsername(request);
		username = (username != null) ? username : "";
		username = username.trim();
		String password = obtainPassword(request);
		password = (password != null) ? password : "";
		
		//username과 password를 추출해서 인증 객체 생성
		UsernamePasswordAuthenticationToken authRequest = new UsernamePasswordAuthenticationToken(username, password);
		// Allow subclasses to set the "details" property
		setDetails(request, authRequest);
		
		//getAuthenticationManager에게 인증 처리를 맡긴다 AuthenticationManager의 구현체가 ProviderManager
		return this.getAuthenticationManager().authenticate(authRequest);
	}
```

```Java
public Authentication authenticate(Authentication authentication) throws AuthenticationException {
//ProviderManager.java
...
//인증 검증 -> 성공하면 createSuccessAuthentication로 이동
result = provider.authenticate(authentication);
...
}
```

```Java
//AbstractUserDetailsAuthenticationProvider.java
//userName과 password를 다시 생성한다
	protected Authentication createSuccessAuthentication(Object principal, Authentication authentication,
			UserDetails user) {
        
		UsernamePasswordAuthenticationToken result = new UsernamePasswordAuthenticationToken(principal,
				authentication.getCredentials(), this.authoritiesMapper.mapAuthorities(user.getAuthorities()));
		result.setDetails(authentication.getDetails());
		this.logger.debug("Authenticated user");
		return result;
	}

```

```Java
//AbstractAuthenticationProcessingFilter.java
    //인증 성공 후 이동
	protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response, FilterChain chain,
			Authentication authResult) throws IOException, ServletException {
			//security context에서 저장
		SecurityContextHolder.getContext().setAuthentication(authResult);
		if (this.logger.isDebugEnabled()) {
			this.logger.debug(LogMessage.format("Set SecurityContextHolder to %s", authResult));
		}
		this.rememberMeServices.loginSuccess(request, response, authResult);
		if (this.eventPublisher != null) {
			this.eventPublisher.publishEvent(new InteractiveAuthenticationSuccessEvent(authResult, this.getClass()));
		}
		//successHandler 동작
		this.successHandler.onAuthenticationSuccess(request, response, authResult);
	}
```