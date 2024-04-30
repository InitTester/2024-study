로거는 프로그램이 실행 중 발생하는 이벤트를 기록하고 추적하는데 사용되고 주고 디버깅, 모니터링, 오류 추적 등에 활용된다.  java에서 println() 메서드와 비슷한 역할을 하지만, 여러가지 면에서 더 유연하고 더 성능이 좋다


- 로그 기록 : 버그, 정보, 경고, 오류 등 다양한 로그로 기록
- 로그 수준 설정 : TRACE, DEBUG, INFO, WARN, ERROR 등등
- 로그 형식 지정 :  메세지에 시간, 로그 수준, 클래스/패키지 이름 등 포함 가능
- 로그 출력 : 콘솔, db, 파일 등 다양한 출력 대상에 기록
- 런타임 변경

로거를 통해 문제를 좀더 빠르게 파악하고 해결하기에 도움을 받는다. 자바에서 로깅 프레임워크로는 Log4j, LogBack, java.util.logging 등이 있다.

1. pom.xml 에 라이브러리 추가
    - logback-classic 1.5.5
    - logback-core 1.5.5
    - slf4j-api  2.0.13
    
    ```
    <!-- <https://mvnrepository.com/artifact/ch.qos.logback/logback-classic> -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.5.5</version>
    </dependency>
    
    <!-- <https://mvnrepository.com/artifact/ch.qos.logback/logback-core> -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
        <version>1.5.5</version>
    </dependency>
    
    <!-- <https://mvnrepository.com/artifact/org.slf4j/slf4j-api> -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>2.0.13</version>
    </dependency>
    
    ```
    
2. logback 설정 파일 생성 후 resources 폴더에 저장
    - logback.xml 파일
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration scan="true" scanPeriod="30 seconds">
    
    	<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    		<encoder>
       			<pattern>[%d{yyyy-MM-dd HH:mm:ss}:%-3relative][%thread] %-5level %logger{35} - %msg%n</pattern>
    			<charset>UTF-8</charset>
    		</encoder>
    	</appender>
    	
    	<logger name="org.springframework" level="DEBUG" additivity="true"/>
    
      <root level="DEBUG">
        <appender-ref ref="STDOUT" />
      </root>
    </configuration>
    ```
    
- 개인 적으로 좋아하는 패턴
    
    <**pattern**>%green(%d{yyyy-MM-dd HH:mm:ss.SSS}) %magenta([%thread]) %highlight(%5level) %cyan(%logger) - %yellow(%msg%n)</**pattern**>
    
 ![](https://velog.velcdn.com/images/initsave/post/d912e5e7-471c-4dd9-b4ff-ab8cb348af53/image.png)
