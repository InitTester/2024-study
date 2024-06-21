![스터디](https://github.com/InitTester/2024-study/assets/148026641/e66af90e-2c3e-4048-a2f6-3a568a427240)

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
