- conf 설정
    - DB, hostname
    
    ```bash
    cd conf
    vi keycloak.conf
    ```
    
    ```bash
    db=mysql
    db-username=seoin
    db-password=tjdls@1234
    db-url=jdbc:mysql://localhost:3307/keycloak?useSSL=false&allowPublicKeyRetrieval=true
    hostname=localhost
    ```
    
    - local에서 실행하기 위한 추가 세팅
        - 권장이 HTTPS여서 로컬에선 HTTP도 가능하게 하려면 추가 설정이 필요함
        
        ```bash
        # HTTP
        http-enabled=true
        hostname-strict-https=false
        ```
        
    
- 실행
    
    ```bash
    ./bin/kc.sh start
    ```