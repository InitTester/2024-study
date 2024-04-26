![숙제](https://github.com/InitTester/2024-study/assets/148026641/580747b5-de89-4f2f-85c1-a5d7189b8e2e)

## 과제 02
### NullPointException 처리하기

이제 인증을 완료하면 auth_yn이 "Y"로 바뀌었기 때문에 인증완료한 인증메일로 다시 접속하면 auth_yn = "N"라는 조건에서 걸리기에 500에러 화면이나오며 NullPointException이 발생합니다
![](https://github.com/InitTester/2024-study/assets/148026641/ee64ccb4-3e11-4a9e-bc2c-ea3a8fc7f3e3)
![스크린샷 2024-04-26 165116](https://github.com/InitTester/2024-study/assets/148026641/1002f4ec-c884-4148-83cd-10ba04d13d55)

그렇기에 이제 이 NullPointException 예외처리가 필요합니다

## JoinService
예외처리는 간단하게 try-catch를 사용해 보겠습니다
```java
public boolean emailAuth(String uri) {
	boolean result;
	try {	// true반환
		MemberAuthDto memberAuthDto = memberAuthDao.getMemberAuthDto(uri);
		//인증 유효성 검사
		Long now = Calendar.getInstance().getTimeInMillis();
		
		result = now < memberAuthDto.getExpireDtm();
		if(result) {
			// member_auth -> auth_yn : Y 로 바꾸기
			memberAuthDao.updateAuthInfo(memberAuthDto);
		}
	}catch(NullPointerException e){	//NullPointerException발생하면 false반환
		result = false;
		
	}
	return result;
}
```
이 코드는 예외가 발생하지 않으면 try안의 코드가 전과 같이 작동하여 result값이 true가 반환되고    
예외가 발생하면 catch로 가서 result값을 false로 해서 리턴합니다    
## LoginController
```java
@RequestMapping("/emailAuth.do")	//회원 인증메일 링크
public ModelAndView emailAuth(@RequestParam("uri") String uri) {
	ModelAndView mv = new ModelAndView();
	mv.setViewName("login");	//login.jsp로 연결
//		joinService.emailAuth(uri);	
//uri로 검색하여 auth_uri 맞는지와 auth_yn=n인지 확인
	
	boolean result = joinService.emailAuth(uri);	
	//uri로 검색하여 auth_uri 맞는지와 auth_yn=n인지 확인
	mv.addObject("result", result);
	String msg = result ? "인증 성공" : "인증 실패";
	mv.addObject("msg", msg); 
	return mv;
}
```
그러면 JoinService에서 반환한 return값으로
LoginController에서 login.jsp로 보내고 "인증 실패" 알림을 띄워줍니다 

![](https://github.com/InitTester/2024-study/assets/148026641/ace80bb8-5842-48ee-b2e5-b6bebb84daf9)
이렇게 해줌으로써 고객은 500에러가 안보이고 에러를 처리할 수 있습니다
