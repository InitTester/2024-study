

## nginx 설정
> ### nginx 찾기
```
yum search nginx
```
> ### nginx 설치
```
sudo yum install nginx
```
설치 하고 `y`로 설치 동의
nginx는 서비스로 등록됨

> ### 업그레이드
```
sudo dnf upgrade --releasever=2023.4.20240528
```
설치 후 업그레이드 하라고 `dnf...`나오면 `sudo`를 붙여 실행 그다음 `y`로 동의

> ### 실행
```
sudo service nginx
```
하면 `start, stop, restart, status`등의 commend supports가 나옴
```
sudo service nginx status
```
`status`로 상태 확인   
`Active: inactive(dead)`라고 나오면 죽어있기에 스타트 필요
```
sudo service nginx start
```
스타트하고 다시 `status`하면 `Active: active(running)`이라고 나옴

> ### 포트가 열려 있는지 확인
```
netstat
```
열려있는 포트 확인
```
netstat -tnl | grep 80 
```
`netstat`만 사용할경우 너무 많은게 나오기에 조건을 걸어 찾아줍니다   
- `netstat -tnl` : TCP프로토콜을 사용하여 주소를 숫자 형태로 표시하고, 대기 중인 연결만 보여주는 명령  
- ` |(파이프)` : 한 명령어의 출력을 다음 명령어의 입력으로 전달
- `grep` : Unix 및 Unix기반 시스템에 사용되는 텍스트 검색 도구(주어진 텍스트에서 해당 패턴을 찾아 출력하거나 필터링에 사용)

EC2 > 보안 그룹 > `8080 -> 80`으로 변경
이제  public DNS를 주소창에 입력하면 
![스크린샷 2024-06-08 212405](https://github.com/InitTester/2024-study/assets/148026641/64b42c76-a736-4985-b3de-a8ff4a94e910)     
이러한 페이지가 나오면 됨


> ## EC2 삭제
![스크린샷 2024-06-08 213420](https://github.com/InitTester/2024-study/assets/148026641/6ae6644b-276a-4e5a-af27-939da48fdcaa)
   
인스턴스 > 작업 > 종료 방지 기능 변경 > 비활성화 > 저장   
인스턴스 > 작업 > 중지 방지 변경 > 비활성화 > 저장   
인스턴스 > 인스턴스상태 > 중지   
인스턴스 > 인스턴스상태 > 종료

> ## EC2 생성
![스크린샷 2024-06-05 211334](https://github.com/junani0v0/pf_jun01/assets/148026641/9061ae42-4ef0-4d8d-9443-02c275abcd74)   
이름을 원하시는 걸로 입력하시고 `Amazon Linux`의 `Amazon Linux 2023 AMI`를 선택합니다.    
이때 `프리 티어 사용 가능`이 무료임으로 잘 확인하세요 
![스크린샷 2024-06-05 211731](https://github.com/junani0v0/pf_jun01/assets/148026641/5428192a-8eed-4dd8-9e48-35be4e52a0c8)   
아키텍처에 자신에가 맞는 비트를 선택시면 되며   
인스턴스 유형에 `t2.micro`를 선택합니다 이것도 `프리 티어 사용 가능`을 확인하세요
그다음 `새 키 페어 생성`을 클릭 (이전에 했던 SSH key를 만든것처럼 해주면 됩니다 차이점이라하면 저희 key를 등록하는게 아닌 아마존이 key를 저희에게 만들어 줍니다)

![스크린샷 2024-06-05 212227](https://github.com/junani0v0/pf_jun01/assets/148026641/ed006317-d102-4e8a-a0cb-cd38f3e20074)
- 키 페어 이름 : 노트북 이름으로 하겠습니다 전 gram이기에 gram으로 작성하겠습니다   
- 키 유형 : RSA
- 프라이빗 키 파일 형식 : .pem  

키 페어 생성을 클릭하면 key파일이 다운로드 됩니다   
잃어버리면 키 생성을 새로 해야함으로 프로젝트 있는 곳에 key폴더를 만들어 잘 보관해 두세요

![스크린샷 2024-06-05 212726](https://github.com/junani0v0/pf_jun01/assets/148026641/301251b9-5fed-4729-978e-c725fe8fc28e)   
그러면 위에 키로 설정한 이름이 등록된게 확인이 되실겁니다   
그 다음으로 네트워크 설정에 `다음에서 SSH 트래픽 허용`에서 처음에 `위치 무관`으로 되어있는데요    
보안에 매우 취약함으로 꼭 `내 IP`로 변경하세요.  
나머지 아래 체크박스도 체크 해주세요   
![스크린샷 2024-06-05 213256](https://github.com/junani0v0/pf_jun01/assets/148026641/44188fd3-640c-488a-9cfb-1b0315780ed1)   
스토리지 구성은 30기가까지 무료임으로 30으로 변경해 주시고요   
IOPS는 입출력을 최대한 사용할 수 있게 하며 파일의 입출력이 많을 경우 사용합니다. 하지만 유료임으로 저희는 gp3을 사용할 것입니다

> ## MobaXterm 재설정
![스크린샷 2024-06-08 214617](https://github.com/InitTester/2024-study/assets/148026641/71b7138f-5203-4921-8818-cbae6010c801)   
Session > SSH > Remote host : [프라이빗 IPv4 주소] > Specify username : [EC2 이름] > Use private key : [본인 SSH Key] > OK > Accept

> ## Java 설치
MobaXterm에서
```
yum search java | grep amazon
```
- `yum` : 패키지 관리자
- `yum search java` : 시스템에서 java관련 패키지 검색

`yum search java`결과값 중 `amazon`이라는 문자열을 찾아 표시

![스크린샷 2024-06-08 215725](https://github.com/InitTester/2024-study/assets/148026641/2e2767b5-0328-4044-91aa-085043f25644)

```
yum install java 11-amazon-corretto.x86_64
```
`java 11-amazon-corretto.x86_64`설치

![스크린샷 2024-06-08 220001](https://github.com/InitTester/2024-study/assets/148026641/0294d134-e737-4147-9454-69707068e27f)
   
그러면 이와같은 권한이 없어 에러가 발생
```
sudo yum install java-11-amazon-corretto.x86_64
```
`sudo`를 붙여줘서 다시 설치   
설치할지 물어보면 `y`
> ### java 위치 찾기
```
whereis java
```
![스크린샷 2024-06-08 220409](https://github.com/InitTester/2024-study/assets/148026641/abbc2c2a-b7ce-456f-a9d4-b444ff8ebc49)
   
java 경로 확인   
```
cd /usr/bin
```
java가 있는 `/usr/bin`으로 이동
```
ll
ls -al
ll | grep java
pwd
```
- `ll` : 파일 및 디렉토리의 상세정보를 보기위해 사용 (ls -l의 축약형)
- `ls -al` : 현재 디렉토리의 모든 파일과 디렉토리를 상세하게 보기위해 사용
- `pwd` : 현재 작업 중인 디렉토리의 경로 출력("print working directory"의 약자)
### #/usr/bin 으로 이동 후 바로가기 찾기
![스크린샷 2024-06-08 100455](https://github.com/InitTester/2024-study/assets/148026641/6d45f794-4e87-4b5f-afa2-f1693afd5db7)

### # /usr/bin/java ---> /etc/alternatives/java 로 연결되어 있으니 이동
```
cd /etc/alternatives
```
위에서 `ll | grep java`으로 나온 java가 있는 경로로 이동
```
ll | grep openjdk
```
![스크린샷 2024-06-08 100617](https://github.com/InitTester/2024-study/assets/148026641/3f5fe2a7-f53b-423b-a5ef-469759148968)

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
> ## tomcat 설치
https://tomcat.apache.org/download-90.cgi   

![alt text](<스크린샷 2024-06-08 224849.png>)
```
cd
```
![alt text](image-34.png)   
홈 디렉토리(~)로 이동
```
cd tools
```
![alt text](image-33.png)   
이렇게 없을 경우(홈 디렉토리에서 해야됨-사진 수정예정)
```
mkdir ~/tools
```
tools 디렉토리 생성
```
cd tools
```
tools로 이동
```
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.89/bin/apache-tomcat-9.0.89.tar.gz
```
wget [복사한 톰캣 url] 로 톰캣 다운로드
- `wget` `url` : 인터넷 상에서 파일 다운로드 명령 
```
ll
```
톰캣 설치 확인
```
gzip -d apache-tomcat-9.0.89.tar.gz
tar -xf apache-tomcat-9.0.89.tar
```
- `gzip -d` : `gz` 압축 원래 형식(d)으로 압축 해제
- `tar` : `tar`압축 해제
- `-x`: 압축을 해제하는 옵션
- `-f`: 파일을 지정하는 옵션(압축 해제할 파일)
```
pwd
ls -al
```
경로 확인 & 파일 확인(~/tools/apache-tomcat-9.0.89/bin/catalina.sh)
```
vi ~/.bashrc
```
- `.bashrc` : 사용자 환경설정   

> `.bash_profile`와 `.bashrc` 차이
- `.bash_profile` : 사용자가 로그인할 때 한 번 실행(로그인 할때만 실행)
- `.bashrc` : 사용자가 새로운 셸 세션을 시작할 때마다 실행(로그인 안해도 실행)
```
alias tomcat="~/tools/apache-tomcat-9.0.89/bin/catalina.sh"
```
![스크린샷 2024-06-08 231415](https://github.com/InitTester/2024-study/assets/148026641/58ba4ca0-5f04-4fc0-9137-317c191c6cbc)
    
아래 `fi`와 `unset`사이에 `alias` 추가
```
:wq
```
저장하고 나가기
```
source ~/.bashrc
```
변경사항 적용
```
tomcat start
```
톰캣 실행
```
netstat -tnl | grep 80
```
톰캣 포트 열려있는지 확인

> ## 8080 접속을 위한 EC2 보안 그룹 설정
EC2 > 보안 그룹 > 인바운드 규칙 편집   
![스크린샷 2024-06-08 111302](https://github.com/InitTester/2024-study/assets/148026641/19b69206-f291-4554-8b64-674ba9ec62bb)


## EC2 삭제 후 재 생성시 RDS 설정에서 EC2 연결 설정 변경
RDS > 데이터베이스 > RDS명 > 연결된 컴퓨팅 리소스

![Untitled](https://github.com/InitTester/2024-study/assets/148026641/1652ffab-d4b0-4b71-820a-362a30c1fdff)

`EC2 연결 설정` > EC2 인스턴스 선택(활성화 된것만 나타남) > 설정

![스크린샷 2024-06-08 103100](https://github.com/InitTester/2024-study/assets/148026641/fcd29e5f-f6e1-4523-a534-1e8080131c12)


## MobaXterm Tunneling 재설정
![스크린샷 2024-06-08 232845](https://github.com/InitTester/2024-study/assets/148026641/2f2a24b7-8a34-442a-997e-c59203ce60de)
![스크린샷 2024-06-08 232918](https://github.com/InitTester/2024-study/assets/148026641/0a641ad4-8711-4f4b-9985-60c6dc61602f)
  

`퍼블릭 IPv4 DNS 주소`와 `인스턴스명` 수정

## war파일 생성
프로젝트 폴더 경로에서 cmd 실행
```
mvn clean package -Pdev
```
Maven을 사용하여 프로젝트 정리, 패키지하여,dev프로파일을 활성화하고 해당 프로파일에 대한 설정을 사용하여 빌드
![스크린샷 2024-06-08 233617](https://github.com/InitTester/2024-study/assets/148026641/e8e0ed95-81e7-43ca-a6ea-692042d900cb)
   
마지막 Building war에 war파일 경로 나옴

## FileZilla
![스크린샷 2024-06-08 233756](https://github.com/InitTester/2024-study/assets/148026641/44fbc6e2-f4b5-4b9d-aaca-78e9157eae56)

사이트 관리자 > 호스트 : `퍼블릭 IPv4 DNS 주소` > 사용자 : `인스턴스명` > 키 파일 : aws key파일 > 연결

dev > 톰캣 > webapps 안에 war파일 넣고 새로고침   
![스크린샷 2024-06-08 234737](https://github.com/InitTester/2024-study/assets/148026641/08d7bd68-b0a9-49a9-921c-7e5363338520)


`퍼블릭 IPv4 DNS 주소`:8080/pf 접속 확인

> ### nginx 설치 & 실행
```
sudo yum install nginx
```
설치 하고 `y`로 설치 동의
nginx는 서비스로 등록됨
```
sudo service nginx start
```
실행

EC2 > 보안 그룹 > `8080 -> 80`으로 변경
이제  public DNS를 주소창에 입력하면   
![스크린샷 2024-06-08 212405](https://github.com/InitTester/2024-study/assets/148026641/64b42c76-a736-4985-b3de-a8ff4a94e910)   
이러한 페이지가 나오면 됨


## nginx
80 웹서버 - 사용자는 앞에 소통하는 것만 있으면 되고 결과만 주면됨 웹 서버 사용자 요청 다 받고 백엔드는 사용자가 몰라도 된다

그래서 앞에는 웹서버 뒤에는 톰캣으로 세트로 사용
- 80 하나에 톰캣 개수제한 없음(포트만 다르면 가능)

pf.com:80 --(프록시 proxy 서버)--> 8080 (톰캣)으로 보내줘야한다

## 프록시 설정
MobaXterm에서
```
cd /etc/nginx
pwd
ll -al
```
`/etc/nginx`로 이동 후 경로 확인 & 파일 확인

![스크린샷 2024-06-09 000213](https://github.com/InitTester/2024-study/assets/148026641/e6038ddd-d521-43d6-a84f-4c195fe4a40c)

```
cd conf.d
pwd
ll -al
```
```
vi /etc/nginx/nginx.conf
```
![스크린샷 2024-06-09 000455](https://github.com/InitTester/2024-study/assets/148026641/14b2b66c-bc8a-4c47-86be-a021a4fb70e4)
   
서비스가 실행됬을때 에러가 발생하면 `/var/log/nginx/error.log`에 에러가 기록됨   
`nginx`는 서비스 이름   
![스크린샷 2024-06-09 000713](https://github.com/InitTester/2024-study/assets/148026641/6c6ede20-a703-476b-8205-e7a28da29ade)
   
엑세스 로그가 찍히는 곳 main은 바로 위에 log_format으로 선언되어 있음
![스크린샷 2024-06-09 000907](https://github.com/InitTester/2024-study/assets/148026641/fad061b1-b862-451f-a67a-e6f96360bf53)
   
`/etc/nginx/conf.d/`경로에서 `*.conf`형식의 파일을 만들면 다 읽겠다는 의미(절대 이 파일을 건들면 X)
```
:q
```
나가기(경로 저장)
```
vi /etc/nginx/conf.d/pf.conf
```
```
i
```
insert모드
```
server {
    listen       80;
    server_name  (ipv4_public_dns주소);

    charset UTF-8;
    access_log  /var/log/nginx/pf/pf.access.log;
    error_log   /var/log/nginx/pf/pf.error.log;

    location /pf {
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-Scheme $scheme;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://127.0.0.1:8080/pf;
    }

    location ~ /\.ht {
        deny  all;
    }
}
```
해보면 권한이 맞지않아 나갈 수 없음
```
:q!
```
강제로 나가기
```
cd ..
ll
```
![스크린샷 2024-06-09 001742](https://github.com/InitTester/2024-study/assets/148026641/bc5d4403-04fc-49bc-8cbc-35f5e8abad99)
   
사용자 root, 사용자 그룹 root 인걸 확인할 수 있다 일반사용자는 수정할 수 없으므로 sudo를 사용해 실행해야 한다
```
cd conf.d
```
다시 이동
```
sudo vi pf.conf
```
```
i
```
insert모드
```
server {
    listen       80;
    server_name  (ipv4_public_dns주소);

    charset UTF-8;
    access_log  /var/log/nginx/pf/pf.access.log;
    error_log   /var/log/nginx/pf/pf.error.log;

    location /pf {
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-Scheme $scheme;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://127.0.0.1:8080/pf;
    }

    location ~ /\.ht {
        deny  all;
    }
}
```
`proxy_pass`에서 뒤에 /webContext? 잊지않고 붙이기(여기로 넘겨주겠다는 의미)   
그리고 location뒤에도 /pf 붙여줘야함 
원래 location만하면 도메인 주소만 치면 pf로 가지는데 지금은 안됨...(이상하게 pf가 2개 붙어져 나옴)
`listen` : 포트번호

```
:wq
```
```
sudo service nginx reload
```
![스크린샷 2024-06-09 002218](https://github.com/InitTester/2024-study/assets/148026641/fa1fcef5-d203-497a-a036-c375409ab124)
   
에러가 발생하는데 access_log의 위치를 바꿔줘야 함

```
cd /var/log/nginx
pwd
ll -al
ll
ls -al
sudo su
```
![스크린샷 2024-06-09 002440](https://github.com/InitTester/2024-study/assets/148026641/f94c84c4-d875-4926-8943-ce5beec8cea6)
   
안에 파일을 확인하려지만 권한이 없다고 나와 `sudo su`로 관리자 권한으로 진입 앞에가 `root`로 변한걸 확인할 수 있음
```
pwd
ll
```
![스크린샷 2024-06-09 002642](https://github.com/InitTester/2024-study/assets/148026641/37026581-67c4-4b5d-9619-8649f18e3817)
   
이제 이렇게 안에 파일이 보입니다   
하지만 절대 root권한으로 파일실행 금지 !!

```
mkdir pf
```
- `mkdir` : 새로운 디렉토리(폴더) 생성
```
exit
```
![스크린샷 2024-06-09 003034](https://github.com/InitTester/2024-study/assets/148026641/ab614366-dbab-4a7b-a48e-a6220e52cc35)
   
하면 다시 user로 바뀜
```
sudo service nginx reload
```
```
nginx -t
```
nginx 설정파일을 다 검사

![스크린샷 2024-06-09 003259](https://github.com/InitTester/2024-study/assets/148026641/f39bb833-8efb-4091-92a3-1b312ad805f8)
   
에러가 발생
```
sudo su
cd /var/log/nginx
ll
```
관리자 권한으로 실행   
경로이동 & 파일 확인
> ### /var/log/nginx/ 소유자 변경
```
vi /etc/nginx/nginx.conf
chown nginx:nginx * -R
ll
```
Nginx의 주요 설정 파일경로로 이동   
현재 디렉토리의 모든 파일 및 디렉토리 소유자와 그룹을 변경
- `chown` : 파일 또는 디렉토리의 소유자와 그룹을 변경하는 명령
- `*` : 전체
- `-R` : 지정 디렉토리와 그 하위 디렉토리 및 파일의 소유자와 그룹을 재구적으로 변경하는 옵션
- `nginx:nginx` : 소유자와 그룹을 `nginx`로 변경한다는 의미

![스크린샷 2024-06-09 003646](https://github.com/InitTester/2024-study/assets/148026641/ccedf602-6524-4891-9d8a-933a834ee52a)
   
이렇게 사용자와 사용자그룹이 nginx로 바뀜
```
nginx -t
```
![스크린샷 2024-06-09 004659](https://github.com/InitTester/2024-study/assets/148026641/73eb3f89-57c3-4385-ae20-1446aaf0eddc)

```
nginx: [emerg] could not build server_names_hash, you should increase server_names_hash_bucket_size: 64
nginx: configuration file /etc/nginx/nginx.conf test failed
```
하나 삭제된거 확인 이제 남아있는 에러가 있는 경로로 이동하여 파일에 수정
```
vi /etc/nginx/nginx.conf
```
하지만 별다른게 없어 다시 이동
```
cd /etc/nginx
pwd
ll
```
![스크린샷 2024-06-09 004947](https://github.com/InitTester/2024-study/assets/148026641/9ce21134-187b-4663-801b-01ebc1ec204e)

```
vi nginx.conf.default
```
여기도 없음
```
sudo vi /etc/nginx/nginx.conf
```
```
server_names_hash_bucket_size 128;
```
![스크린샷 2024-06-09 005310](https://github.com/InitTester/2024-study/assets/148026641/f7926155-517d-4618-b562-8b13e731b246)
   
http안에 access_log 아래 추가
저장하고 나와서 
```
nginx -t
```
해보면 이제 에러가 사라짐   
![스크린샷 2024-06-09 005450](https://github.com/InitTester/2024-study/assets/148026641/47f05908-1112-4ce9-a939-3a4e55f1ae10)
   
```
exit
```
다시 관리자에서 사용자로 나옴
```
sudo service nginx reload
```
![스크린샷 2024-06-09 005632](https://github.com/InitTester/2024-study/assets/148026641/36a29a94-d69f-45b7-b77c-ff28940bbc6e)
   
정상작동 확인

## 배포 링크 생성
https://bitly.com/   
Sign up Free > Free > 구글 로그인 > remind me later > Create new > Link   

![스크린샷 2024-06-09 010540](https://github.com/InitTester/2024-study/assets/148026641/5f2670dc-2ff3-4122-8213-a08a0f45dc71)
![스크린샷 2024-06-09 010648](https://github.com/InitTester/2024-study/assets/148026641/853400a4-b320-4010-b9ca-2a7e80642057)
   
`Destination` : 프로젝트 메인주소 전부(index.do까지)   
`Title` : 제목 하고 뒤에 괄호하고 날짜작성   
생성하면 `bit.ly/...`주소가 나오는데 그게 이제 배포용 짧은 링크

긴주소 넣으면 이력서등에서 짤릴 확률이 높기에 짧은 주소 사용   
제목에 날짜를 넣는 이유는 ip주소가 바뀌기 때문에 관리용으로 날짜를 넣음


--------------------------------
> ## tip.  
service나 설정을 바꿀 경우 무조건 `sudo`사용
- `sudo` : 일반 사용자가 슈퍼 유저 또는 다른 사용자의 권한으로 명령을 실행하게 해줌
(Unix 및 Unix 기반 시스템에서 사용되는 명령어, "Superuser Do"의 약자)
- `netstat -tnl` : TCP프로토콜을 사용하여 주소를 숫자 형태로 표시하고, 대기 중인 연결만 보여주는 명령  
- ` |(파이프)` : 한 명령어의 출력을 다음 명령어의 입력으로 전달
- `grep` : Unix 및 Unix기반 시스템에 사용되는 텍스트 검색 도구(주어진 텍스트에서 해당 패턴을 찾아 출력하거나 필터링에 사용)
- `yum` : 패키지 관리자
- `yum search java` : 시스템에서 java관련 패키지 검색
- `ll` : 파일 및 디렉토리의 상세정보를 보기위해 사용 (ls -l의 축약형)
- `ls -al` : 현재 디렉토리의 모든 파일과 디렉토리를 상세하게 보기위해 사용
- `pwd` : 현재 작업 중인 디렉토리의 경로 출력("print working directory"의 약자)
- `vi` : 텍스트 편집기
- `~/` : 홈 디렉토리 경로
- `.bash_profile` : 사용자 로그인 시 실행되는 스크립트 파일, 사용자의 환경 설정 및 사용자 정의 명령어등을 포함
- `.bashrc` : 사용자 환경설정   
- `source` : 변경사항 즉시 적용
- `source ~/.bash_profile` : 현재 셸 세션에서 .bash_profile 파일을 실행(자동로그인 실행되지 않음)
- `wget` `url` : 인터넷 상에서 파일 다운로드 명령 
- `gzip -d` `압축파일` : `gz` 압축 원래 형식(d)으로 압축 해제
- `tar` : `tar`압축 해제
- `-x`: 압축을 해제하는 옵션
- `-f`: 파일을 지정하는 옵션(압축 해제할 파일)
- `mkdir` : 새로운 디렉토리(폴더) 생성
- `chown` : 파일 또는 디렉토리의 소유자와 그룹을 변경하는 명령
- `-R` : 지정 디렉토리와 그 하위 디렉토리 및 파일의 소유자와 그룹을 재구적으로 변경하는 옵션
- `nginx:nginx` : 소유자와 그룹을 `nginx`로 변경한다는 의미

### `.bash_profile`와 `.bashrc` 차이
- `.bash_profile` : 사용자가 로그인할 때 한 번 실행(로그인 할때만 실행)
- `.bashrc` : 사용자가 새로운 셸 세션을 시작할 때마다 실행(로그인 안해도 실행)

### 환경변수
- `:wq` : 저장 후 나가기
- `:q` : 나가기
- `:q!` : 강제 나가기
- `#` : 주석
- `dd` : 한줄 삭제
