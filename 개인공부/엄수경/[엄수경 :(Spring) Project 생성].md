### 1. Web Project 생성하기
<img src="https://velog.velcdn.com/images/eunoia73/post/f289ffd5-ebb4-4ab7-9944-c4fbd5faa277/image.png" width="60%" height="60%">
1-1. web module version을 4.0으로 맞춘다.
<img src="https://velog.velcdn.com/images/eunoia73/post/67dcc561-4fd6-4972-808d-e5ba1ae6a1b7/image.jpeg" width="60%" height="60%"> 
1-2. src/main/resources를 추가해야 한다.
<img src="https://velog.velcdn.com/images/eunoia73/post/b12196c6-d8a0-409e-922e-e463cebe0b22/image.jpeg" width="60%" height="60%">

1-3. 편의상 Content directory를 'WebContent'로 맞춘다.  
1-4. Generate web.xml deployment descriptor에 체크한다.  
Context root는 Properties > Web Project Settings에서도 변경 가능.

<img src="https://velog.velcdn.com/images/eunoia73/post/bb52d8df-b3e5-49ff-8bc2-b08bf3f58c3f/image.jpeg" width="60%" height="60%">

### 2. configure -> Convert to Maven Project
maven project로 변환한다. -> pom.xml이 생김

<img src="https://velog.velcdn.com/images/eunoia73/post/95af9959-2b1d-4244-93f9-1d6cbcd32908/image.jpeg" width="60%" height="60%">

### 3. pom.xml 설정
3-1. pom.xml에 spring 라이브러리 추가(web, context, webmvc)
https://mvnrepository.com/ 에서 'spring-web'검색

<img src="https://velog.velcdn.com/images/eunoia73/post/8ae7cd11-43cb-4362-873f-3de58c922ff3/image.jpeg" width="60%" height="60%">

3-2. maven update
pom.xml에 추가하면 maven update를 해줘야한다.

<img src="https://velog.velcdn.com/images/eunoia73/post/224477cd-4ee1-4009-a3ff-31cebdc25044/image.jpeg" width="60%" height="60%">


  
### 4. web.xml에 Servlet 정의
WebContent>WEB-INF>web.xml

<img src="https://velog.velcdn.com/images/eunoia73/post/71bf23ee-7a50-4b70-9e75-31340d047c7b/image.jpeg" width="80%" height="80%">

```code
<!-- Spring servlet start -->
	<servlet>
		<servlet-name>pf</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>pf</servlet-name>
		<url-pattern>*.do</url-pattern>
	</servlet-mapping>
  
```

* 프로젝트 실행되거나 톰캣에 올라가면 web.xml에서 시작된다.  
web.xml은 프로그램이 무엇으로 구성되어있는지 간략하게 알려준다.   
(여기에 스프링이 있다. 서블릿이 있다. 설정파일은 저기에 있다.)  
== 건축물대장(지하 1층에 기계실, 전기실 등이 있다)  

* < welcome-file >로 감싸져 있는 index.html은  가장 처음에 띄워주는 페이지

### 5. Servlet 설정파일 생성
WebContent>WEB-INF>pf-servlet.xml 파일 생성
코드 붙여넣기

```code
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans     
        http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx-4.1.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.1.xsd">

	<!-- The controllers are auto-detected POJOs labeled with the @Controller 
		annotation. -->
	<context:component-scan
		base-package="com.portfolio.www" use-default-filters="false">
		<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
		<context:include-filter type="annotation" expression="org.springframework.stereotype.Component" />
	</context:component-scan>

	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/" />
		<property name="suffix" value=".jsp" />
	</bean>

	<mvc:resources mapping="/resources/**" location="/resources/" />

	<mvc:annotation-driven />

</beans>
```

### 6. index.html 파일 생성
WebContent아래 index.html파일 생성

* 코드 넣기 (context root 주의!)
```code
<script>
window.onload = function(){
	location.href="/001/index.do";
}
</script>
```

### 7. jsp 파일이 들어갈 views 폴더 생성, index.jsp 파일 만들기  
WebContent > WEB-INF > views 폴더 생성  
views에 jsp파일들이 들어갈거다.  
index.jsp 파일 만들기  

### 8. index.do 서블릿 클래스 작성
Java Resources > src/main/java > com.portfolio.www.controller 패키지 생성  
패키지 밑에 IndexController.java 클래스 생성  

<img src="https://velog.velcdn.com/images/eunoia73/post/75d742db-13e4-403b-b573-0adcbb4eb59f/image.png" width="70%" height="70%">


```java
package com.portfolio.www.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class IndexController {
	public IndexController() {
		System.out.println("============================생성됨============================");
	}
	
	@RequestMapping("/index.do")
	public String indexPage() {
		return "index";
	}
}

```

### 9. context root 변경하기
Properties > Web Project Settings에서 변경 가능

<img src="https://velog.velcdn.com/images/eunoia73/post/d58a65fe-f0df-444f-8dc9-433d4636ef82/image.jpeg" width="70%" height="70%">

### 10. tomcat 연결하기
Servers Tomcat에서 우클릭 > Add and Remove > 해당프로젝트 Add

<img src="https://velog.velcdn.com/images/eunoia73/post/1cd5e88c-3464-4201-9bad-9cb7f607a4d6/image.jpeg" width="70%" height="70%">

<img src="https://velog.velcdn.com/images/eunoia73/post/4acea900-3899-40d1-ba9e-a418b36ad9df/image.jpeg" width="70%" height="70%">
