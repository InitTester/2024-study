![스터디](https://github.com/InitTester/2024-study/assets/148026641/e66af90e-2c3e-4048-a2f6-3a568a427240)

> # Jasypt 암호화
> ## pom.xml에 Jasypt 라이브러리 적용
```xml
<!-- 암호화 -->
<dependency>
    <groupId>org.jasypt</groupId>
    <artifactId>jasypt-spring31</artifactId>
    <version>1.9.3</version>
</dependency>
```
pom.xml에 Jasypt 라이브러리 추가   
(jasypt은 Spring 3.0까지 지원,jasypt-spring31은 Spring 3.1 이상을 지원)

> ## 암호화 사이트를 활용한 암호화
https://www.devglan.com/online-tools/jasypt-online-encryption-decryption   

![스크린샷 2024-06-17 000648](https://github.com/junani0v0/pf_jun01/assets/148026641/ccb90f07-a101-4c03-bd67-1b3c95f20388)   

간단하게 위 사이트를 사용하여 암호화 & 복구화가 가능   

> ### 암호화
암호화는 사진의 왼쪽 붉은 부분을 입력
- `Enter Plain Text to Encrypt` : 변환시키고 싶은 값을 입력
- `Select Type of Encryption` : 복구화도 해야 하기에 `Two Way ENcryption`로 선택
- `Enter Secret Key` : 암호화에 사용할 Key 값 입력
- `Encryption` : 암호화
- `Jasypt Encrypted String` : `Encryption`을 완료하면 암호화된 값이 생성됨

> ### 복구화
복구화는 사진의 오른쪽 오렌지 부분을 입력
- `Enter Jasypt Encrypted Text` : 암호화된 값 입력
- `Select Action Type` : `Decryption Password`로 선택
- `Secret Key Used during Encryption` : 암호화에 사용한 Key 값 입력
- `Match/Decryption` : 복구화
- `Result` : 암호화로 변환시켰던 값 복구되어 생성

> ## context-bean.xml Jasypt bean등록 
```xml
<!-- Jasypt(암호화) Start -->
    <bean id="encryptorConfig" class="org.jasypt.encryption.pbe.config.EnvironmentPBEConfig">
    	<!-- 사용할 암호화 알고리즘 -->
        <property name="algorithm" value="PBEWithMD5AndDES" />
        <property name="password" value="암호화에 사용한 Key값"/>
    </bean>

    <bean id="encryptor" class="org.jasypt.encryption.pbe.StandardPBEStringEncryptor">
        <property name="config" ref="encryptorConfig" />
    </bean>

    <bean class="org.jasypt.spring31.properties.EncryptablePropertyPlaceholderConfigurer">
        <constructor-arg ref="encryptor" />
        <property name="locations">
            <list>
                <!-- application.properties 파일 경로 -->
                <value>classpath:/application.properties</value>
            </list>
        </property>
    </bean>
<!-- Jasypt End -->
```

> ##  암호화값 사용
암호화된 값을 사용하는것은 기존에 사용하던곳에 `ENC(암호화된 값)`으로 바꿔 넣어주기만 하면됩니다   
![스크린샷 2024-06-17 003237](https://github.com/junani0v0/pf_jun01/assets/148026641/aa264448-48f9-41a2-8e43-57052e3fe950)   
저는 application.properties를 만들어 그곳에 따로 값을 선언하고 
```xml
<property name="username" value="${email.username}" />
<property name="password" value="${email.password}" />
```
이러한 형식으로 값을 가져와 사용하였습니다

> ##  암호화에 사용한 Key값 환경변수 등록(local)
![스크린샷 2024-06-17 002139](https://github.com/junani0v0/pf_jun01/assets/148026641/b001b70a-277c-40b7-b421-849f78ed8a12)
![스크린샷 2024-06-17 002218](https://github.com/junani0v0/pf_jun01/assets/148026641/07da63d3-8f0e-41bf-a392-c7acaaf10abb)

암호화에 사용한 Key값이 전에는 노출되어 있기에 누구나 복구화가 가능하여 보안에 취약하였습니다   
그렇기에 환경변수로 key값을 등록하여 외부노출을 차단해줍니다   
### Run > Run Configuration > 서버 선택 > Environment > add > name과 key값 등록 > apply   

> ## context-bean.xml Jasypt bean등록 수정
```xml
<!-- Jasypt(암호화) Start -->
    <bean id="encryptorConfig" class="org.jasypt.encryption.pbe.config.EnvironmentPBEConfig">
    	<!-- 사용할 암호화 알고리즘 -->
        <property name="algorithm" value="PBEWithMD5AndDES" />
        <!-- 환경 변수를 사용하여 ASOG_ENCRYPTION_KEY 설정 -->
	    <property name="passwordEnvName" value="ASOG_ENCRYPTION_KEY" />
    </bean>

    <bean id="encryptor" class="org.jasypt.encryption.pbe.StandardPBEStringEncryptor">
        <property name="config" ref="encryptorConfig" />
    </bean>

    <bean class="org.jasypt.spring31.properties.EncryptablePropertyPlaceholderConfigurer">
        <constructor-arg ref="encryptor" />
        <property name="locations">
            <list>
                <!-- application.properties 파일 경로 -->
                <value>classpath:/application.properties</value>
            </list>
        </property>
    </bean>
<!-- Jasypt End -->
```
```xml
<property name="passwordEnvName" value="ASOG_ENCRYPTION_KEY" />
```
key값을 대체할 환경변수에 등록한 이름을 넣어줍니다 이때 `property이름`도 `password`가 아닌 `passwordEnvName`로 변경해야합니다

> # 원격 톰캣의 Jasypt 환경변수 설정

![스크린샷 2024-06-19 131929](https://github.com/junani0v0/pf_jun01/assets/148026641/ed44634e-2632-4a4b-b7f0-4841d21cf6b5)   
이클립스 상단 `RUN` > `Run Configuration` > `Arguments` >`VM arguments` > `- Djasypt.encryptor.password=key값`입력(그냥 타이핑하면 입력가능)

![스크린샷 2024-06-17 002218](https://github.com/junani0v0/pf_jun01/assets/148026641/2d790c20-4097-41b1-81ce-0095fab700ea)   

`Arguments` > `Environment` > `add` > Variable(Name) : 복구화키를 불러올 key값 입력, Value : 복구화키 입력 > apply

### # 최종 경로 /usr/lib/jvm/java-11-amazon-corretto.x86_64
```
cd /usr/lib/jvm
```
`java-11-amazon-corretto.x86_64`가 있는 `/usr/lib/jvm`로 이동
## java 환경변수 설정
```
vi ~/.bash_profile
```
- `vi` : 텍스트 편집기
- `~/` : 홈 디렉토리 경로
- `.bash_profile` : 사용자 로그인 시 실행되는 스크립트 파일, 사용자의 환경 설정 및 사용자 정의 명령어등을 포함


```
i
```
insert모드로 들어가
```
JAVA_HOME=최종 경로

export path=$PATH:$JAVA_HOME/bin
```
```
JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64

export path=$PATH:$JAVA_HOME/bin
```
### Jasypt추가
```
JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64
export APP_ENCRYPTION_PASSWORD=jun
export path=$PATH:$JAVA_HOME/bin
```
![스크린샷 2024-06-19 132603](https://github.com/junani0v0/pf_jun01/assets/148026641/ada43709-9c9c-4cdd-a32f-70c2210c67f0)

`fi`아래 최종경로 추가해서 추가
(띄어쓰기도 인식하니 조심)
```
:wq 
```
- `:wq` : 저장 후 나가기
- `:q` : 나가기
- `:q!` : 강제 나가기

```
source ~/.bash_profile
```
- `source` : 변경사항 즉시 적용
- `source ~/.bash_profile` : 현재 셸 세션에서 .bash_profile 파일을 실행(자동로그인 실행되지 않음)

> ## Jasypt 톰캣 환경변수 설정 - catalina.sh 파일을 수정
![스크린샷 2024-06-19 133258](https://github.com/junani0v0/pf_jun01/assets/148026641/c0c1142d-e994-48c3-81b1-83d3942ce972)   

`cd tools/apache-tomcat-9.0.89/bin` > `vi catalina.sh`   

![스크린샷 2024-06-19 133211](https://github.com/junani0v0/pf_jun01/assets/148026641/e0824eac-0c8e-4154-834f-90b842511357)


`/키워드`+Enter : 원하는 키워드 검색, n을 누르면 다음 키워드 검색   
`/JAVA_OPTS` > security위치에 `JAVA_OPTS="$JAVA_OPTS -Djasypt.encryptor.password=복구화키값"` > esc > :wq

```
tomcat start
```
톰캣 실행
