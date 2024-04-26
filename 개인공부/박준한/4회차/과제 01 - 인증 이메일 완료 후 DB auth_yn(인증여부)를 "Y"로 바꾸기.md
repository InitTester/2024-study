![숙제](https://github.com/InitTester/2024-study/assets/148026641/580747b5-de89-4f2f-85c1-a5d7189b8e2e)


## 과제 01
### 인증 이메일 완료 후 DB auth_yn(인증여부)를 "Y"로 바꾸기

전에 만든 회원가입 인증메일을 받고 인증하기를 하였을 경우 "인증 완료" 메시지는 나오지만 DB에 있는 인증여부(auth_yn)는 바꾸지 않았습니다 이걸 과제로 변경해 보겠습니다

## MemberAuthDao
MemberAuthDao에 update쿼리를 작성해보겠습니다  
처음에 WHERE 조건을 uri로 하려했다기 너무 길기도해서 seq로 했는데 문제 없이 작동 잘되었습니다.  

쿼리문은 간단하게 작성하였습니다   
- forum의 member_auth 테이블에서 member_seq이 같으면 auth_yn = 'Y'로 변경
```java
	//인증 완료되면 auth_yn='y'로 바꾸기
	public int updateAuthInfo(MemberAuthDto memberAuthDto) {
		String sql = "UPDATE forum.member_auth  "
				+ "SET auth_yn = 'Y' "
				+ "WHERE member_seq=" + memberAuthDto.getMemberSeq();
			  //+ "WHERE auth_uri=" + memberAuthDto.getAuthUri();
		
		return update(sql);
	}
```
이렇게 쿼리문을 작성 후 이제 service에서 실행하도록 바꿔보겠습니다

## JoinService
다행히도 전에 insert 등을 return update(sql)로 처리를 해봐서 수월하게 할 수 있었습니다
```java
	public boolean emailAuth(String uri) {
		MemberAuthDto memberAuthDto = memberAuthDao.getMemberAuthDto(uri);
		//인증 유효성 검사
		Long now = Calendar.getInstance().getTimeInMillis();
		
		boolean result = now < memberAuthDto.getExpireDtm();
		if(result) {
			// member_auth -> auth_yn : Y 로 바꾸기
			memberAuthDao.updateAuthInfo(memberAuthDto);
		}
		return result;
	}
```
![스크린샷 2024-04-26 164229](https://github.com/InitTester/2024-study/assets/148026641/4390f392-6ba2-43c5-b00a-85ae489ac775)

인증 메일 링크접속 후 auth_yn만 "Y"로 잘 바뀐걸 확인할 수 있습니다
