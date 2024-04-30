디비버는 데이터베이스 관련 시스템을 다양하게 지원해주는 오픈 소스 소프트웨어이다.

디비버의 주요 특징

- 다양한 데이터베이스 지원
- 시각적 쿼리 작성
- 데이터베이스 객체관리
- 데이터 시각화
- 팀 협업 및 버전관리
- 플러그인 지원

디비버 다운로드 : https://dbeaver.io/download/

*~~아직 디비버로는 mysql만 사용해봤지만 기존 UI보다는 좀 더 깔끔한 느낌이 든다~~*

셋팅방법(MySQL기준)은  

1. [새 데이터 베이스 연결] 아이콘 클릭 또는 Ctrl +Shift +N
    
    ![](https://velog.velcdn.com/images/initsave/post/8f84f082-db05-40a6-9ab5-d5e54efbd3e2/image.png)
    
2. [MySQL 선택] (만약 목록에서 보이지 않는다면 “Type part of databasw/driver name tofilter”에 검색) → [다음]
    
    ![](https://velog.velcdn.com/images/initsave/post/5a10017e-0f8e-4c42-8633-f61e2bd9d070/image.png)
    
3. [Connect to a database] 값 설정 → [Test Connection] → [완료] DB 연결 완료 
- 기존에 도커 컨테이너에서 설정한 값을 참고해서 입력해주자
    
    ```bash
    # for Windows
    docker run --name mysql-lecture -p 
    53306:3306 -v c:/dev/docker/mysql:/etc/mysql/conf.d -e 
    MYSQL_ROOT_PASSWORD=admin_123 -d 
    mysql:8.3.0
    
    - Port, UserName, Password ..
    ```
    
    ![](https://velog.velcdn.com/images/initsave/post/53225fc6-1a2d-431e-93d5-45e89fd1e84b/image.png)

    - 만약 Test Connection 중 “Public Key Retrieval is not allowed” 라는 에러 메세지가 뜨면
        
        아래 처럼 [allowPublicKeyRetrieval] 값을 [TRUE] 로 변경해주면 해결된다.
        ![](https://velog.velcdn.com/images/initsave/post/f71a9ec3-bd48-42ba-8257-c68d5a6c5013/image.png)

    
4. DB[forum] 기본 셋팅
    - [SQL 편집기 열기(F3)] → 스크립트 붙여넣기 → [SQL 스크립트 실행(Alt + X)]
        ![](https://velog.velcdn.com/images/initsave/post/074625bf-c970-4021-83ae-9338e22c4e00/image.png)

        - 스크립트 실행 버튼        
        ![](https://velog.velcdn.com/images/initsave/post/d4d2c57f-afd8-4996-84f7-7732368dfc4a/image.png)
        
        **<span style="color:red">주의사항</span> 스크립트 실행 버튼을 누루기 전에 트랜잭션을 <span style="color:red">수동설정</span> 후 실행 할 것** 
        
        1. 트랜잭션이 **자동인** 상태            
            ![](https://velog.velcdn.com/images/initsave/post/760208c2-2cec-4c24-b314-bbbbc55cfbd8/image.png)
            
        2. 트랜잭션이 **수동인** 상태            
            ![](https://velog.velcdn.com/images/initsave/post/7a057fe4-bab4-4e3b-8098-00ecdca2196c/image.png)
            
    
    모든 작업이 끝나고 아래 처럼 데이터베이스 [forum]이 생성 되면 DB 생성이 완성된 것이다.    
    ![](https://velog.velcdn.com/images/initsave/post/ff7448b9-a09f-4295-b3df-fa8607919424/image.png)
    
    **마지막으로 DB에 설정 해줘야 할 것은 기본 AI(Auto Increment)는 설정이 되지 않기 때문에 각 테이블 마다 재설정 해주어야 한다.**
