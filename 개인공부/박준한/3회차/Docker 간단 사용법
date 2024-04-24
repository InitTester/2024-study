[##_Image|kage@bG08RJ/btsGFqD1Hz2/AtknFhpVndPAHW7nYtcrX1/img.jpg|CDM|1.3|{"originWidth":1024,"originHeight":1024,"style":"alignCenter","filename":"_45dc6c54-2497-4020-8fc4-9081d7621038.jpg"}_##][##_Image|kage@tJMEX/btsGGeDiiNM/kEpbQ6Ar86qGRNxAXobIB0/img.png|CDM|1.3|{"originWidth":1583,"originHeight":1080,"style":"alignCenter","filename":"0.png"}_##]

먼저 docker desktop을 실행하고

[##_Image|kage@bzv0tk/btsGEYgSNzQ/qBzWTmHrWgmGpFIRpfnQ10/img.png|CDM|1.3|{"originWidth":1133,"originHeight":1155,"style":"alignCenter","filename":"1.png"}_##]

Windows PowerShell 실행하고 

[##_Image|kage@chC5Ii/btsGGOEjJxW/x4xgsxKG1avvKMvKojJVL0/img.png|CDM|1.3|{"originWidth":1844,"originHeight":780,"style":"alignCenter","filename":"2.png"}_##]

```
docker run --name mysql-lecture -p 53306:3306 -v c:/dev/docker/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=admin_123 -d mysql:8.3.0
```

입력하면 다운로드를 시작하고 마지막줄에 이상한 해쉬값이 나오면 성공 

[##_Image|kage@dZJYHn/btsGFCxsUXv/uXDhfhuNWjACeQNhLZl5N0/img.png|CDM|1.3|{"originWidth":1579,"originHeight":1079,"style":"alignCenter","filename":"3.png"}_##]

docker desktop에 추가된걸 볼 수 있습니다

[##_Image|kage@Fwh2A/btsGFg2Co90/azVOIKOPjcE1WbL7NGqR30/img.png|CDM|1.3|{"originWidth":1864,"originHeight":236,"style":"alignCenter","filename":"3-1.png"}_##]

```
docker ps
```

docker ps 입력하면 현재실행하고 있는 docker의 정보가 나오며 맨 끝에있는 mysql-lecture가 컨테이너 이름입니다

현재 mysql은 mysql-lecture라는 이름으로 docker image가 돌고있다는 이야기입니다

이 컨테이너 이름을 알면 이제 아래 코드로 동작을 조작할 수 있습니다

```
docker stop 컨테니어 이름

docker start 컨테니어 이름
```

[##_ImageGrid|kage@C4I38/btsGHuyr7I8/498p8nX06qbPH0YZRMqCek/img.png,kage@bbSU5o/btsGG83EvY8/v2ypZpv4z2ueYqpFG7TOo1/img.png|data-is-animation="false" data-origin-width="1063" data-origin-height="91" data-filename="4.png" style="width: 48.8928%; margin-right: 10px;" data-widthpercent="49.47",data-is-animation="false" data-origin-width="1062" data-origin-height="89" data-filename="6.png" style="width: 49.9444%;" data-widthpercent="50.53"|_##][##_Image|kage@d11VWc/btsGFjLNJum/PpBgvP4kOdMA1EcfWLhCg1/img.png|CDM|1.3|{"originWidth":1135,"originHeight":161,"style":"alignCenter","filename":"5.png"}_##]

docker를 stop하면 docker desktop 컨테이너 상태가 Exited된걸 확인할 수 있습니다

```
docker images
```

현재 내 컴퓨터에 다운로드된 이미지가 나옴

[##_Image|kage@b5pC63/btsGFDwpBii/L7DOa6rBGDefVI0wcLKHmk/img.png|CDM|1.3|{"originWidth":1342,"originHeight":189,"style":"alignCenter","filename":"9.png"}_##]

이미지 하나로 n개의 컨테이너를 만들 수 있다(이미지는 소스라 생각하고 그걸 확장해서 사용하는 것)

```
docker rmi 이미지 이름(REPOSITORY):태그(TAG)
```

이미지 삭제에는 이미지 이름(REPOSITORY)과 태그(TAG)를 같이 입력해줘야한다

[##_Image|kage@cXePAM/btsGFsIAIcD/SZ6WX9ypkD6RQEibAhE6WK/img.png|CDM|1.3|{"originWidth":1870,"originHeight":1192,"style":"alignCenter","filename":"10.png"}_##]

해쉬값이 쭉나오면 성공 

[##_Image|kage@9UmNu/btsGG6kqkc6/CxZyaLkE6PNWpJQCGRkL0k/img.png|CDM|1.3|{"originWidth":1337,"originHeight":142,"style":"alignCenter","filename":"11.png"}_##]

docker images를 입력해 제대로 삭제되었는지 확인

```
docker rm 컨테이너 이름
```

컨테이너 삭제

[##_Image|kage@bz7G8i/btsGHix9r0o/i9BLKEn2npsk8h6XrPN3a0/img.png|CDM|1.3|{"originWidth":1840,"originHeight":134,"style":"alignCenter","filename":"7.png"}_##]

컨테이너 삭제 전에 컨테이너를 stop해야한다 아니면 아래처럼 에러 나온다

[##_Image|kage@bkbIUk/btsGFBk81VE/JMsPk61LofbpsqVCy5DiCK/img.png|CDM|1.3|{"originWidth":997,"originHeight":96,"style":"alignCenter","filename":"8.png"}_##]

제대로 stop 후 컨테이너를 삭제하면 컨테이너 이름이 나오며 성공

```
docker run --name mysql-lecture -p 53306:3306 -v c:/dev/docker/mysql:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=admin_123 -d mysql:8.3.0
```

다시 입력하면 다시 컨테이너와 이미지가 생성된다

Tip.

db운영시 docker 잘 사용하지 않는다

이유는 docker는 실시간 트렌젝션에 계속 들어가기에 데이터를 영구적으로 저장하기 힘들기 때문
