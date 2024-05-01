SpringJDBC란 ?  

Native JDBC(JAVA 표준 API)를 효율적으로 사용하기 위해 스프링 프레임워크에서 제공하는 기능 중 하나이다.

- 간편한 데이터베이스 접근
- 객체-관계 맵핑(ORM) 지원
- 트랜잭션 관리
- DataSource 추상화
- 예외처리 및 리소스 관리

1. pom.xml 라이브러리 추가
    - Spring Web 5.3.34
    - Spring WebMvc 5.3.34
    - Spring Context 5.3.34
    - Spring Beans 5.3.34
    - Spring JDBC 5.3.34
    - mysql-connector 8.3.0
        
        
2. web.xml 추가
    
    ```xml
    	<!-- Spring configuration start -->
    	<listener>
    		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    	</listener>
    
    		<!-- 루트 어플리케이션컨텍스트 영역 -->
    	<!-- 시작점을 의미 한다 -->
    	<context-param>
    		<param-name>contextConfigLocation</param-name>
    		<param-value>classpath*:/context*.xml</param-value>
    	</context-param>
    	<!-- Spring configuration end -->
    ```
    
    - contextConfigLocation : 이 속성은 Spring ApplicationContext가 로드해야 하는 xml 설정 파일의 위치를 지정해준다. (context-root.xml, context-datasource.xml을 사용하기 위해)
    - ContextLoaderListener : IoC 컨테이너를 초기화하기 위해 사용된다. 이 리스너는 Spring의 ApplicationContext를 로드하고 초기화 하는 역할(로드해서 설정된 모든 빈(bean)을 생성하고 관리)을 한다.
    

<p>$\huge{\rm{\color{#5ad7b7}~~xml은 컴파일 할 것이 없기 때문에 클래스 패스로 간다.~~}}$</p>

3. context*.xml 파일 src/main/resources 위치에 저장
    
    <p>$\huge{\rm{\color{#5ad7b7}~~resources에 넣으면 클래스패스로 가기 때문에 가능하다.~~}}$</p>
    
    - context-root.xml
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
        	xmlns:context="http://www.springframework.org/schema/context"
        	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        	xmlns:tx="http://www.springframework.org/schema/tx"
        	xmlns:mvc="http://www.springframework.org/schema/mvc"
        	xmlns:util="http://www.springframework.org/schema/util"
        	xsi:schemaLocation="
        		http://www.springframework.org/schema/beans
        		http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
        		http://www.springframework.org/schema/mvc
        		http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd
        		http://www.springframework.org/schema/tx
        		http://www.springframework.org/schema/tx/spring-tx-4.1.xsd
        		http://www.springframework.org/schema/context
        		http://www.springframework.org/schema/context/spring-context-4.1.xsd">
        </beans>
        ```
        
    - context-datasource.xml
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
        	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        	xsi:schemaLocation="http://www.springframework.org/schema/beans 
        	http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">
        
        	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
        		<property name="url" value="jdbc:mysql://localhost:53306/forum?serverTimezone=Asia/Seoul" />
        		<property name="username" value="root"/>
        		<property name="password" value="admin_123"/>
        	</bean>
        
        </beans>
        ```
        
        - id : 식별할 수 있는 유일한 값
        - class : 해당 bean이 어느 클래스로부터 생성되는지를 지정

1. JoinService, JoinDao 작성
    - JoinService
        
        ```java
        package com.portfolio.www.service;
        
        import java.util.HashMap;
        import com.portfolio.www.dao.JoinDao;
        import at.favre.lib.crypto.bcrypt.BCrypt;
        
        public class JoinService {
        	
        	private JoinDao joinDao;
        	
        	public void setJoinDao(JoinDao joinDao) {
        		this.joinDao = joinDao;
        	}
        	
        	public int join(HashMap<String,String> params) {
        		String passwd = params.get("passwd");
        		
        		String encPasswd  = BCrypt.withDefaults().hashToString(12,passwd.toCharArray());
        		System.out.println("encPasswd >>>>>>>>> " + encPasswd);
        		
        		BCrypt.Result result = BCrypt.verifyer().verify(passwd.toCharArray(), encPasswd);
        		System.out.println("result.verified >>>>>>> " + result.verified);
        		
        		params.put("passwd", encPasswd);
        		
        		return joinDao.join(params);
        	}
        }
        
        ```
        
    - JoinDao (member 테이블의 insert 쿼리 작성)
        
        ```java
        package com.portfolio.www.dao;
        
        import java.util.HashMap;
        import javax.sql.DataSource;
        import org.springframework.jdbc.core.JdbcTemplate;
        
        public class JoinDao extends JdbcTemplate {
        	
        	public JoinDao() {}
        	
        	public JoinDao(DataSource ds) {
        		super(ds);
        	}
        	
        	public int join(HashMap<String,String> params) {
        		
        		String sql = "INSERT INTO forum.`member` "
        				+ "( member_id, passwd, member_nm, email, auth_yn, pwd_chng_dtm, join_dtm) "
        				+ "VALUES( '" + params.get("memberId") 
        				+ "', '" + params.get("passwd")
        				+ "', '', '" + params.get("email")
        				+ "', '', '', DATE_FORMAT(now(),'%Y%m%d%H%i%s'))";
        		
        		return update(sql);
        	}
        }
        
        ```
        
        - JdbcTemplate에서 생성자를 통해 필수적으로 DataSource를 넣어야 하기 때문에 context-root.xml 에서 수동 bean 을 등록 할때 constructor-arg로 필수적인 주입을 해준다.

1. Spring Bean 수동 등록 방법
    - [ ]  Bean 정의 시 property, constructor-arg 차이점 알기
        - property(프로퍼티)
            
            빈의 멤버 변수에 값을 주입할때 사용되고 이 방법은 기본생성자를 사용해서 빈을 생성 한 후에 프로퍼티 값을 설정할 때 사용한다.
            
        - constructor-arg(생성자 인자)
            
            빈의 생성자를 통해 값을 주입할수 있다. 생성자를 호출 할때 사용, 이 방법은 빈을 생성할 때 필수적으로 주입되어야 하는 값이 있을 때 사용한다.
            
        
        **property 와 constructor-arg의 차이점**은 값이 주입되는 시점이다. 빈이 생성된 후에 값을 주입하거나 빈이 생성될 때 생성자를 통해 값을 주입하느냐에 따라서  주입되는 값의 필수 여부와 주입 시점에 따라 결정된다.
        
    - [ ]  value, ref 속성에 대해 알기
        - value
            
            value는 프로퍼티에 직접 값을 설정할때,문자열 형태이고 주로 기본데이터 타입이나 문자열과 같은 값으로 주입
            
        - ref
            
            빈의 프로퍼티에 다른 빈을 참조할 때 사용, 다른 빈을 주입할 때 !! ref 속성을 쓰면 다른 빈의 id를 지정해서 해당 빈은 참조 할수 있다 (ref : dataSource(context-datasource에 정의 된 수동 빈 , joinDao)
            
    - context-root.xml 에 등록
        
        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
        	xmlns:context="http://www.springframework.org/schema/context"
        	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        	xmlns:tx="http://www.springframework.org/schema/tx"
        	xmlns:mvc="http://www.springframework.org/schema/mvc"
        	xmlns:util="http://www.springframework.org/schema/util"
        	xsi:schemaLocation="
        		http://www.springframework.org/schema/beans
        		http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
        		http://www.springframework.org/schema/mvc
        		http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd
        		http://www.springframework.org/schema/tx
        		http://www.springframework.org/schema/tx/spring-tx-4.1.xsd
        		http://www.springframework.org/schema/context
        		http://www.springframework.org/schema/context/spring-context-4.1.xsd">
        
        	<bean id="joinService" class="com.portfolio.www.service.JoinService" >
        	 	<property name="joinDao" ref="joinDao" />
        	 </bean>
        	
        	<bean id="joinDao" class="com.portfolio.www.dao.JoinDao" >
        		<constructor-arg ref="dataSource" />
        	</bean>
        	
        </beans>
        ```
        
    - <p>$\huge{\rm{\color{#5ad7b7}만약  bean이 생성 되지 않았다면 이전에 설정해둔 logback의 기록에서 생성되었는지 확인해보자}}$</p>
    - <p>$\huge{\rm{\color{#5ad7b7}자동으로 내가 bean을 설정하게 되면 위에 처럼 설정파일로 bean을 넘길 수 없다.}}$</p>
    
2. login.jsp 수정
    - 기존의 form signup 안에 구현된 div 안에는 중요한 속성이 빠져있다.
        
        “name” 이라는 속성인데 그렇다면 해당 속성이 **필요한 이유**는 무엇일까 ? 대부분 ***폼 데이터 전송, 폼데이터 접근, 라벨과의 연결, 서버 측 유효성 검사, 서버 측 프로세싱*** 등 작업을 할때 중요하다.  
        지금 필요한 이유는 바로 폼 데이터 전송을 위한 name이 필요하다. 그러므로 각 요소에 필요한 name 속성을 입력해 주자
        
        <p>$\huge{\rm{\color{#5ad7b7}servlet에서 값을 가져올 수 없기 때문에 중요하다.}}$</p>
        
    - 그 다 음 폼 데이터 전송을 위한 위치 지정
        
        <form action =”#”> 의 경우 해당 페이지에 아무런 변경 없이 그대로 유지하겠다는 의미이다. 그러므로 <form action =”/03/join.do”>로 변경해주어야 한다.
        
        경로의 의미는 {contextroot}/{데이터를 전송받을 컨트롤러 메서드 맵핑uri}
        
    - 클릭 이벤트 처리
        
        form action 을 지정해도 현재 버튼에 이벤트 처리가 되어 있지 않아 작동이 되지 않을 것이다.
        
        이 부분은 간단하게 onclick=”submit();” 이벤트를 추가해주자 
        
        <p>$\huge{\rm{\color{#5ad7b7}짜여진 틀이 있기 때문에 버튼 타입은 가급적 button 요소를 사용(button(x) , input type=button  (o) 기존에 input 으로 구현되어 있다면.. !}}$</p>
        
3. LoginController 수정
    
    ```java
    @Autowired // 현재 Autowired 한 행위를 스프링에서는 di라고 한다.
    	private JoinService joinservice;
    
    @RequestMapping("/join.do")
    public String join(@RequestParam HashMap<String, String> params) {
        System.out.println(params);
        return "join";
    }
    ```
    
    - DI : 객체 간의 의존성을 관리하고 객체를 생성하고 조립하는 방법, 현재 bean을 정의 했고 @Autowired를 통해 의존성을 주입했다.
    
    컨트롤러 까지 코드 수정 후 회원 가입 버튼을 누루면 아래와 같은 데이터가 넘어가는 것을 확인 할 수 있다.
    
   ![](https://velog.velcdn.com/images/initsave/post/aedb441f-c68d-4481-b57a-6c1d4826c5ad/image.png)


- [ ]  resources 추가 (js, css)
- [ ]  login.jsp 추가
- [ ]  controller 작성
- [ ]  [loginPage.do](http://loginPage.do) 확인
- [ ]  JoinService 작성

jsp → controller → service → dao → db
