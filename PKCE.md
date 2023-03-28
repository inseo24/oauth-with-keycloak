### PKCE(Proof Key for Code Exchange)

- 여기서는 Keyclaok JS Adapter를 사용한 PKCE 과정에 대한 설명을 OAUTH 2.0 PKCE 설명과 섞어 썼음

1. 클라이언트 애플리케이션은 code_verifier를 생성하고 이를 변환하여 code_challenge를 만듭니다.
2. 클라이언트 애플리케이션은 인가 요청을 보낼 때 code_challenge와 함께 요청합니다.
3. 인증 서버는 인가 요청을 처리하고, 인가 코드를 클라이언트 애플리케이션에 반환합니다. 동시에 인증 서버는 code_challenge 값을 기록해둡니다.
4. 클라이언트 애플리케이션은 반환된 인가 코드와 code_verifier를 사용하여 액세스 토큰을 요청합니다.
5. 인증 서버는 code_verifier를 사용하여 code_challenge를 검증하고, 검증이 성공하면 Access Token과 ID Token을 클라이언트 애플리케이션에 반환합니다.

------ 

- Keycloak JS Adapter
    - 클라이언트에서 Auth Server로 요청
        - GET API : 클라이언트 애플리케이션에서 인증 서버로 사용자를 리다이렉션함
        - 부득이하게 아래 url은 [oauth playground](https://www.oauth.com/playground/authorization-code-with-pkce.html?state=W0-pAmMECzfQ704M&code=yFwFy2nR7oYyiHUyTo_pkrMtQsddxf_ji6OpiiVutfBJ6rtP) 에서 가져옴
        - ```
           https://authorization-server.com/authorize?
            response_type=code
            &client_id=ERsOI4b-MmcjbslNe_s0hPB5
            &redirect_uri=https://www.oauth.com/playground/authorization-code-with-pkce.html
            &scope=photo+offline_access
            &state=W0-pAmMECzfQ704M
            &code_challenge=aqD_wZk9G_qwlSFuiU-yGpyRRNCegfxJChwAy2_c2GQ
            &code_challenge_method=S256
        
          ```
    - 유저가 username & password로 로그인(인증)
    - 인증 성공시 Redirect Uri로 Auth code와 state return
        - POST API 302 Redirect : 사용자가 로그인 정보를 제출한 후, 키클록 서버에서 사용자 인증을 처리하는 과정. 
        - 사용자 인증이 성공적으로 이뤄지고 키클록 서버가 인증 코드를 생성해 이를 클라이언트로 전달
    
    - ```
        POST https://authorization-server.com/token

        grant_type=authorization_code
        &client_id=ERsOI4b-MmcjbslNe_s0hPB5
        &client_secret=a2KWe6YyfE_ZxGL5w41vmoMlTb27cSdy_geuidsDWFPDB4Ty
        &redirect_uri=https://www.oauth.com/playground/authorization-code-with-pkce.html
        &code=yFwFy2nR7oYyiHUyTo_pkrMtQsddxf_ji6OpiiVutfBJ6rtP
        &code_verifier=jaNON3BJpX9P9509ekeozghcedQJkz4aJIPY-lL981UnHhdu
      ```
        - 304 NOT Modified : 클라이언트가 인증 코드를 사용해 액세스 토큰을 요청하는 과정. Keycloak JS Adapter가 액세스 토큰을 요청하는 과정
    
    - 위에서 각각 302, 304 status code가 사용된 이유
        1. 302 Found (이전에는 Moved Temporarily): 302 상태 코드는 요청한 리소스가 일시적으로 다른 위치에 있음을 나타낸다. 클라이언트는 이 새로운 위치로 리디렉션되어 리소스에 액세스해야 한다. 이 경우, Keycloak에서 인증을 완료한 후 클라이언트를 다시 리디렉션하는 과정에서 302 상태 코드가 사용
        2. 304 Not Modified: 304 상태 코드는 클라이언트가 이미 가지고 있는 캐시된 리소스가 여전히 유효한 경우 사용된다. 이 코드를 받았을 때 클라이언트는 로컬 캐시에서 해당 리소스를 사용해야 한다. 이 경우, 클라이언트가 이미 인증 코드를 받았기 때문에 304 상태 코드가 사용
        
        - 요약하면, 302 상태 코드는 Keycloak 서버에서 클라이언트로의 리디렉션을 처리하는 데 사용되며, 304 상태 코드는 클라이언트가 이미 캐시된 인증 코드를 가지고 있어 추가적인 요청을 할 필요가 없음을 나타낸다.
        
    
    - 위 3가지 요청을 통해 키클록 서버와 클라이언트 사이 2번의 요청이 발생함
        - 첫 번째 요청은 사용자를 인증 서버로 리디렉션
        - 세 번째 요청은 인증 코드를 사용해 액세스 토큰을 요청함. 이 과정에서 브라우저는 인가 코드를 받고, Keycloak JS Adapter를 통해 액세스 토큰을 요청함