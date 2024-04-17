<aside>
💡 input → servlet → ouput (입력  - 처리 - 출력)
- *인풋에서 파라미터를 보내면 아웃풋에서 파라미터를 사용할 수 있다, 하지만 그 과정에서 처리해주는 서블릿이 필요하다.
- 파라미터가 어디에 있는지 아는 것이 중요!!!
- html에서 무조건 servlet에서 파라미터로 넘어오면 파라미터를 무조건 출력을 하려면 servlet에서 보내주어야 한다, 받는 것도 중요하지만 보내는 것도 중요하다.*

</aside>

# Controller → View  데이터 넘기기

- Controller
    - input/output 입력/동작에 대한 제어를 처리하거나 데이터를 모델에 담아 넘기기 위한 코드
        
        ```java
        @RequestMapping("/output.do")
        	public ModelAndView outputPage(@RequestParam HashMap<String, String> params) {
        		System.out.println(params);
        		
        		ModelAndView mv = new ModelAndView();
        		mv.setViewName("output");
        		mv.addObject("measureDate", "2024-01-11");
        		return mv;
        	}
        ```
        
- View
    - 입력/출력 보여주거나 값 전달을 위한 코드
    
    ```jsp
          // **HttpServletRequest**
    			request 값은 ? :  <%= request.getParameter("measureDate") %> <br>
    			// **ModelAndView**
    			ModelAndView 값은 ? : ${measureDate}
    ```
    


💡 값을 전달 하기 위한 2가지 방법
>1. HttpServletRequest
  request 객체는 jsp페이지에서 사용할 수 있는 기본 객체 중 하나 이기 때문에 선언하지 않아도, HttpServletRequest 의 값을 사용할 수 있다. (기본 객체는 request, response, pageContext, session, application, out, config, page, exception이 있다.)
   - input 요소 name 값으로 전달
2. ModelAndView
  컨트롤러가 처리한 결과 데이터와 해당 데이터를 보여줄 뷰에 대한 정보를 담아서 전달한다. View 에서는 el 로 값을 불러올 수 있다.
   - 메서드에서 ModelAndView 를 통해 ViewName과 addObject로 뷰와 데이터 전달!
![](https://velog.velcdn.com/images/initsave/post/3e460e3b-013f-444b-bf18-9698673597b4/image.png)



# LoginController, resources

- login.jsp 생성 (한글로 된 부분이 깨지는 것은 EUC-KR → UTF-8 변경)    
![](https://velog.velcdn.com/images/initsave/post/0c3a35d9-97fd-4537-8d6a-b85cd1a1128d/image.png)
    
- [LoginController.java] → “/loginPage.do”  연결
    
    ```java
    package com.portfolio.www.controller;
    
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    
    @Controller
    public class LoginController {
    	
    	@RequestMapping("/loginPage.do")
    	public String loginPage() {
    		return "login";
    	}
    }
    ```
    
![](https://velog.velcdn.com/images/initsave/post/03414901-a1e8-485f-9085-465559def1ad/image.png)|![](https://velog.velcdn.com/images/initsave/post/8e885a8e-840f-4681-bf68-82674d988f9e/image.png)|
-|-|
 - **왼쪽과 같은 화면이 나오는 이유는 css, js 파일이 없기 때문이다.** 또한 jsp 파일에서 link  href경도 해당 프로젝트 경로에 맞춰 주어야 한다.
        - [WebContent] → [resources] → [css]/[js]
        - <**link** rel=*"stylesheet"* href=*"/02_1/resources/css/style.css"*>
       

💡**mvc:resources**
- servlet.xml 에 mvc:resources 이고, 현재 login.jsp에서 css를 불러 주는 경로가 지정되어 있다. 
- mapping과 location 은 한 세트이고 대부분 동일하게 기입한다.**

```basic
- src
  - main
    - webapp
      - resources
        - css
          - style.css
        - js
          - script.js
        - img
          - logo.png

```
![](https://velog.velcdn.com/images/initsave/post/68ee5bd0-40f0-43bc-a84e-191bada9395c/image.png)WebContent → “/”(루트 경로) Deploy 되면 루트 경로로 지정되어 있다.


만약 mvc:resources를 지우고 새로고침을 한다면…![](https://velog.velcdn.com/images/initsave/post/3a6d70c2-759f-48d9-825b-ccc4a490db39/image.png)


<tip. 캐시 비우기 및 강력 새로고침>
![](https://velog.velcdn.com/images/initsave/post/3bb753a6-8b37-4772-90f6-4a2c5a8be077/image.png)


프로젝트에서 시작을 하면 index.html → [index.do] 호출→ index.do의 영역은 servlet의 영역이고 uri가 url-pattern에 정의 된 기준으로 받아지게 된다.
![](https://velog.velcdn.com/images/initsave/post/5bcfd5f3-43ac-400e-8eda-cd15b3bd9ee5/image.png)


실제로 서블릿 파일들은 servlet-mapping으로 정해져 있으니까 js, css 같은 정적(static)파일은 찾아 갈 수 있게 따로 “mvc:resources(static 파일을 찾아 가기 위한 태그)”로 지정했다. 

<tip>

```jsx
 <link rel="stylesheet" href="./resources/css/style.css">
 // href 경로에서 처음에 "./" 의 경우 현재위치에서 찾아가게 된다.
```
