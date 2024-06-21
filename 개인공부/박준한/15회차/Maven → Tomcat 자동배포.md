
![스터디](https://github.com/InitTester/2024-study/assets/148026641/e66af90e-2c3e-4048-a2f6-3a568a427240)

> # 자동배포 

> ## EC2 보안그룹
![스크린샷 2024-06-21 214929](https://github.com/junani0v0/pf_jun01/assets/148026641/9eda7ff0-7a06-4a7e-aeb8-be929f991281)

8080포트를 내IP로만 접근가능하게 생성   
![스크린샷 2024-06-21 220707](https://github.com/junani0v0/pf_jun01/assets/148026641/99a75510-1626-4da7-b6fe-e5b9ea7da179)   
이제 `퍼블릭 IPv4 DNS주소:8080` 하면 이렇게 톰캣이 나오는데요 여기서 `Manager App` 클릭   

![스크린샷 2024-06-21 220752](https://github.com/junani0v0/pf_jun01/assets/148026641/5dd8c2e7-47c8-46c2-b0cd-674679ea9754)   

> ## 모바텀

![스크린샷 2024-06-21 221528](https://github.com/junani0v0/pf_jun01/assets/148026641/0da2bbbc-7e5b-4aca-a799-db79cab340f4)   
원격 톰캣이 깔려있는 `conf/tomcat-users.xml`로 가줍니다  

```
cd tools/apache-tomcat-9.0.89/conf
```
로 가보시면 아까 찾으려한 `tomcat-users.xml`이 있는걸 확인할 수 있습니다   
그러면 `pwd`하여 경로가 나오게하고 이 경로를 복사
```
/home/ec2-user/tools/apache-tomcat-9.0.89/conf
```
좌측 상단 `/home/ec2-user` 에 붙여넣고 `Enter`

![스크린샷 2024-06-21 221820](https://github.com/junani0v0/pf_jun01/assets/148026641/0285b696-69d4-435a-9540-23f12fd8311b)   
그러면 좌측에 `conf`안의 파일들이 보입니다   
`tomcat-users.xml`우클릭 > `Open with default text edit`을 클릭하면 편집기가 나옵니다   
![스크린샷 2024-06-21 222008](https://github.com/junani0v0/pf_jun01/assets/148026641/1273d846-09e8-4611-ab8d-66453dc90f1d)   
맨 아래에 한칸띄우고 저장하고 닫으면
![스크린샷 2024-06-21 222048](https://github.com/junani0v0/pf_jun01/assets/148026641/0be7b0e1-3615-4b37-8148-9d359a4769f1)   
이렇게 자동으로 업로드를 할것인지 물어봅니다 > Yes   

![스크린샷 2024-06-21 222908](https://github.com/junani0v0/pf_jun01/assets/148026641/01304721-5003-40a9-8de3-401c162311b2)   
`ll`해보면 `tomcat-users.xml`의 날짜가 바뀐것을 확인 할 수 있습니다 

이렇게 윈도우에서는 `vi`를 사용하지 않아도 모바텀에서 편집이 가능합니다

그럼 다시 `tomcat-users.xml`를 열어봅니다

```
<role rolename="manager-script"/>
<role rolename="manager-gui"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<user username="admin" password="admin" roles="manager-gui,manager-script,manager-status,manager-jmx"/>
```
그러고 위의 코드를 주석이 아닌곳에 붙여넣어줍니다   

![스크린샷 2024-06-21 223622](https://github.com/junani0v0/pf_jun01/assets/148026641/d741a7ef-e5ed-4531-bb08-35594adfb0f9)   
저장 > Yes > 닫기 하면 됩니다

```
cd tools/apache-tomcat-9.0.89/conf
```
![스크린샷 2024-06-21 224636](https://github.com/junani0v0/pf_jun01/assets/148026641/f3df5eca-5dec-426b-b9d7-69b28d36749f)   
에서 `ll` 하면 `Catalina` 가 있습니다

![스크린샷 2024-06-21 224804](https://github.com/junani0v0/pf_jun01/assets/148026641/07681d5c-0126-440b-a4e8-ed7bfc846889)   
`cd Catalina` > `ll` > `cd localhost` > `ll` 에 가서 파일이 있는지 확인합니다. 저는 0임으로 하나 생성하겠습니다   
```
vi manager.xml
```
그리고 들어가지만 아무것도 하지 않고 `:wq`로 저장하고 나옵니다   
![스크린샷 2024-06-21 230058](https://github.com/junani0v0/pf_jun01/assets/148026641/fb035c16-3c79-49a2-bf03-d46a3dbef194)   
`pwd`해서 나온 경로를 복사해 아까처럼 좌측 상단에 복사해 편집기를 열어줍니다
![스크린샷 2024-06-21 230123](https://github.com/junani0v0/pf_jun01/assets/148026641/cd76e182-ff0a-45d1-ab16-aaa6660708f8)   

```
<Context privileged="true" antiResourceLocking="false" docBase="${catalina.home}/webapps/manager">
    <Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="^.*$"/>
</Context>
```
![스크린샷 2024-06-21 230442](https://github.com/junani0v0/pf_jun01/assets/148026641/5d5c80d3-a8bf-474e-afc9-500a70f496f3)   
그리고 위의 코드를 복사해 붙여넣고 저장 > 적용을 해줍니다

> ## pom.xml
```xml
<!-- 배포 -->
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
		    <update>true</update>
        <url>http://localhost:8080/manager/text</url>
        <server>TomcatServer</server>
        <path>/${project.build.finalName}</path>
        <username>admin</username>
        <password>admin</password>
    </configuration>
</plugin>
```
`pom.xml`에 `plugin`을 붙여놓는곳에 붙여넣어줍니다

이때 url을 자신에게 맞는 주소로 바꿔줍니다
그리고 여기있는 `username`과 `password`는 위에서 `tomcat-users.xml`에 넣은 값과 맞춰야합니다

![스크린샷 2024-06-21 230732](https://github.com/junani0v0/pf_jun01/assets/148026641/566b5d26-b380-417f-a0ae-1e9483cb81f4)   

서버 tomcat 재시작

> ## tomcat 매니저
그다음 아까 
![스크린샷 2024-06-21 220752](https://github.com/junani0v0/pf_jun01/assets/148026641/5dd8c2e7-47c8-46c2-b0cd-674679ea9754)

![스크린샷 2024-06-21 231320](https://github.com/junani0v0/pf_jun01/assets/148026641/2add072a-b90f-4955-8035-108d0c4938b4)  
새로고침하면 로그인창이 나오고 설정한 아이디 비밀번호를 입력해줍니다

![스크린샷 2024-06-21 231557](https://github.com/junani0v0/pf_jun01/assets/148026641/7f979703-f6e2-4e9f-86d3-e42ecac20fe3)   
그러면 이제 이런 매니저 창이 나오는데요 여기서 껏다 켰다를 할 수 있습니다 
아래 WAR파일 배포도 있습니다

메이븐 빌드를 해봅니다
패키지 폴더에서 `cmd` > `mvn clean package -Pdev`하고 

![스크린샷 2024-06-21 231557](https://github.com/junani0v0/pf_jun01/assets/148026641/dcb208ee-b266-4a92-b437-105c15d1624a)   

더 완벽하게 하기위해 `tomcat 매니저`에서 `/pf`를 중지하고 삭제해줍니다   

> ## cmd

![스크린샷 2024-06-21 232526](https://github.com/junani0v0/pf_jun01/assets/148026641/d4f1d6e7-e38d-4b70-8aba-6aecb59c499e)   

cmd에서 `mvn clean package -Pdev tomcat7:deploy`를 해보면 `pf`가 없어 실패했다고 나옵니다  

> ## tomcat 매니저

![스크린샷 2024-06-21 232649](https://github.com/junani0v0/pf_jun01/assets/148026641/233d6676-0bf4-4fc4-8b52-a7e75f376e4f) 

그러면 `tomcat 매니저`아래 WAR파일을 선택해 불러오고 배치를 누르면 위에 다시 `/pf`가 추가됩니다

> ## cmd

![스크린샷 2024-06-21 233327](https://github.com/junani0v0/pf_jun01/assets/148026641/5f125932-a676-47aa-a9b4-f39f60d074f2)

하지만 그래도 에러가 나옵니다

> ## .m2/settings.xml
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
   http://maven.apache.org/xsd/settings-1.0.0.xsd">

   <servers>
    <server>
      <id>TomcatServer</id>
      <username>admin</username>
      <password>admin</password>
    </server>
   </servers>
 </settings>
```
윈도우 > 사용자 > 사용자명 > .m2 > 메모장으로 settings.xml을 만들어 위 코드 복사 후 저장

> ## cmd

```
mvn clean package -Pdev tomcat7:redeploy
```
cmd에서 `mvn clean package -Pdev tomcat7:redeploy`를 넣고 엔터
![스크린샷 2024-06-21 233608](https://github.com/junani0v0/pf_jun01/assets/148026641/e926d05b-c3dd-44d8-9a88-c7c82249975a)   

> ## tomcat 매니저

그러고 `tomcat 매니저`를 새로고침하면   

![스크린샷 2024-06-21 233831](https://github.com/junani0v0/pf_jun01/assets/148026641/48db6f39-03b2-4b92-a9c9-da1bb879fe0e)   
![스크린샷 2024-06-21 233853](https://github.com/junani0v0/pf_jun01/assets/148026641/dfa75447-bda4-4a16-b887-ef2c06946d62)   
이렇게 pf가 바뀐것을 확인할 수 있습니다 


여기서 배치에 따라 cmd에 실행하는 코드가 다릅니다
```
# 없으면
mvn clean package -Pdev tomcat7:deploy

# 있으면
mvn clean package -Pdev tomcat7:redeploy
```
----------------------

> # 정리

이제 배포할때는 그냥 아래 코드를 작성하면 war파일을 생성하고 배포까지 자동으로 해줍니다
```
mvn clean package -Pdev tomcat7:redeploy
```
