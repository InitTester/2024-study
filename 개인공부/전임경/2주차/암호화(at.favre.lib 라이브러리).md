이전 회원가입을 하면서 비밀번호는 암호화가 되지 않는 상태에서 저장했다. 그렇게 하면 개인 정보보안에 좋지 않기 때문에 암호화를 이용해보도록하자

사용 라이브러리는 at.fare.lib 

https://mvnrepository.com/artifact/at.favre.lib/bcrypt

참고 사이트 : https://www.tabnine.com/code/java/classes/at.favre.lib.crypto.bcrypt.BCrypt

1. pom.xml 에 라이브러리 추가
    
    ```xml
        <!-- https://mvnrepository.com/artifact/at.favre.lib/bcrypt -->
        <dependency>
            <groupId>at.favre.lib</groupId>
            <artifactId>bcrypt</artifactId>
            <version>0.10.2</version>
        </dependency>
    ```
    
2. JoinService 구현
    
    ```java
    	public int join(HashMap<String,String> params) {
    	
    		String passwd = params.get("passwd");
    		String encPasswd  = BCrypt.withDefaults().hashToString(12,passwd.toCharArray());
    		System.out.println("encPasswd >>>>>>>>> " + encPasswd);
    		
    		BCrypt.Result result = BCrypt.verifyer().verify(passwd.toCharArray(), encPasswd);
    		System.out.println("result.verified >>>>>>> " + result.verified);
    		
    		params.put("passwd", encPasswd);
    		
    		return joinDao.join(params);
    	}
    ```
    
    - BCrypt.withDefaults().hashToString(12,passwd.toCharArray());
        
        <br>BCrypt 해싱 알고리즘을 사용해서 해시값으로 변환하는 방법
        
        - **`BCrypt.withDefaults()`**: BCrypt의 기본 설정을 사용하여 BCrypt 해시 알고리즘을 생성합니다. 이는 일반적으로 해시를 생성하기 전에 사용되는 설정을 나타냅니다.
        - **`hashToString(12, passwd.toCharArray())`**: 주어진 비밀번호를 BCrypt 해시로 변환합니다. 첫 번째 매개변수는 해시 라운드(rounds)의 수를 지정하는데, 이는 해시를 생성할 때 사용되는 작업 수를 나타냅니다. 라운드 수가 높을수록 해시 생성에 소요되는 시간이 더 많이 소요되지만 보안성이 높아집니다. 두 번째 매개변수는 해시할 비밀번호를 char 배열로 전달합니다.
    - BCrypt.Result result = BCrypt.verifyer().verify(passwd.toCharArray(), encPasswd);
        
        <br>BCrypt를 사용해서 주어진 암호와 암호화된 암호를 비교하는 방법
        
        - **`BCrypt.verifyer()`**: BCrypt의 검증기(Verifier)를 생성합니다. 이는 일반적으로 비밀번호의 검증에 사용됩니다.
        - **`verify(passwd.toCharArray(), encPasswd)`**: 주어진 비밀번호를 암호화된 해시와 비교하여 인증을 수행합니다. 첫 번째 매개변수는 비교할 비밀번호를 char 배열로 전달하고, 두 번째 매개변수는 비교할 해시 문자열(encPasswd)을 전달합니다.
       
![](https://velog.velcdn.com/images/initsave/post/f26821e8-08a4-4561-9358-16867121ed34/image.png)

```bash
2024-05-02 13:29:10.074[0;39m [35m[http-nio-8080-exec-19][0;39m [39mDEBUG[0;39m [36morg.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping[0;39m - [33mMapped to com.portfolio.www.controller.LoginController#join(HashMap)
[0;39mencPasswd >>>>>>>>> $2a$12$xH0WVsUxOYYxjs167aGs7uxy2H3QC/3yMFkEMtU9Gz/H9arrKB58u
result.verified >>>>>>> true
[32m2024-05-02 13:29:10.581[0;39m [35m[http-nio-8080-exec-19][0;39m [39mDEBUG[0;39m [36mcom.portfolio.www.dao.JoinDao[0;39m - [33mExecuting SQL update [INSERT INTO forum.`member` ( member_id, passwd, member_nm, email, auth_yn, pwd_chng_dtm, join_dtm) VALUES( 'null', '$2a$12$xH0WVsUxOYYxjs167aGs7uxy2H3QC/3yMFkEMtU9Gz/H9arrKB58u', '', 'abcd@abc.com', '', '', DATE_FORMAT(now(),'%Y%m%d%H%i%s'))]
```

회원가입을 하면 위와 같이 해당 메서드의 값이 출력되는 것을 볼 수 있다. 또한 db에서도 해시값으로 변환된 비밀번호가 저장될 것이다.
