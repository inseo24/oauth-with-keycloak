- reference
    [OktaDev OAuth 2.0 and OpenID Connect](https://www.youtube.com/watch?v=996OiexHze0&ab_channel=OktaDev)


- Access Token, ID Token, Refresh Token
    1. Access Token
        1. JWT or opaque token, 일반적으로 다음과 같은 데이터가 포함된다.
            - iss (발행자): 토큰을 발행한 인증 서버의 식별자
            - aud (수신자): 이 토큰이 전달되어야 하는 리소스 서버
            - exp (만료 시간): 토큰의 만료 시간을 나타내는 UNIX 시간
            - iat (발행 시간): 토큰이 발행된 시간을 나타내는 UNIX 시간
            - scope (범위): 이 토큰이 부여하는 권한 범위
    2. ID Token
        1. ID Token은 OpenID Connect 프로토콜을 사용하여 사용자를 인증하는 데 사용되는 토큰
        2. 항상 JWT 형식으로 제공되며, 일반적으로 다음과 같은 데이터를 포함한다.
            - iss (발행자): 토큰을 발행한 인증 서버의 식별자
            - aud (수신자): 이 토큰이 전달되어야 하는 클라이언트
            - exp (만료 시간): 토큰의 만료 시간을 나타내는 UNIX 시간
            - iat (발행 시간): 토큰이 발행된 시간을 나타내는 UNIX 시간
            - sub (주제): 사용자를 고유하게 식별하는 값
            - nonce (난스): 클라이언트에서 생성한 난수로, 인증 요청을 인증 응답과 연결하는 데 사용
            - auth_time (인증 시간): 사용자가 인증된 시간을 나타내는 UNIX 시간
    3. Refresh Token
        1. 현재 토큰이 만료될 때 새로운 access token을 얻기 위해 사용
        2. 일반적으로 exp 이 길고 type은 정의되어 있지 않아 JWT or opaque token. 
        3. access token 만료시 애플리케이션에선 refresh token을 이용해 사용자가 다시 인증할 필요 없이 새로운 access token을 요청함
- OpenID Connect
    
    OpenID Connect (OIDC)는 OAuth 2.0 프로토콜을 기반으로 한 인증 및 사용자 정보 공유를 위한 인터넷 표준이다. 이 프로토콜은 사용자 인증을 위한 안전하고 확장 가능한 솔루션을 제공함. 
    
    OIDC는 아래의 주요 기능을 포함함
    
    1. ID Token: ID 토큰은 JSON Web Token (JWT) 형식으로 사용자에 대한 인증 정보를 포함하며 클라이언트에 반환되어 사용자 **인증**을 확인한다. 
    2. UserInfo endpoint: OIDC는 사용자 정보를 더 얻기 위한 UserInfo 엔드포인트를 제공한다. 클라이언트는 access token을 사용하여 이 엔드포인트에 요청을 보낼 수 있다. 이 엔드포인트는 사용자의 프로필 정보 (이름, 이메일 주소 등)와 같은 추가 정보를 반환한다.
    3. Standard set of scopes: OIDC는 표준화된 범위 집합을 정의하여 클라이언트가 요청할 수 있는 사용자 정보를 제한다. scope는 'openid', 'profile', 'email', 'address', 'phone' 등을 포함하며, 각 범위가 요청하는 사용자 정보에 대한 접근 권한을 부여한다. 클라이언트는 필요한 scope를 요청하여 사용자의 동의를 얻고, 해당 scope로 제한된 사용자 정보에만 접근할 수 있다.
    4. Standardized Implemenation: OIDC는 인증 및 사용자 정보 공유에 대한 표준화된 프로토콜을 제공한다. 이를 통해 개발자들은 서로 다른 인증 provider와의 호환성을 높일 수 있으며, 보안 및 개인정보 보호 관련 best practice를 따르는 것이 쉬워진다.
    
    요약하면, OpenID Connect는 OAuth 2.0 프로토콜을 확장하여 사용자 인증 및 사용자 정보 공유를 위한 안전하고 확장 가능한 표준을 제공함.