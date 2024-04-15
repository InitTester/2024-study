선언부와 구현부에 대해서 알아 보기 위해 우선 메서드에 대해서 알아보자.

**메서드란 ?** 

  행위하는 것

- 중복코드 제거
- 유지보수(재사용성)
- 프로그램의 구조화

메서드의 구조는 선언부와 구현부로 이루어져있다. 그 중 선언부는 접근제어자, 반환타입, 메서드명, 매개변수(파라미터)로 이루어져 있다. 

- 접근제어자는 내가 객체를 호출할 시 호출이 가능한가 아닌가의 가능여부가 되고, private, public, defeault, protected 로 구성되어 있다. 일반적으노 public과 private를 많이 사용 된다. protected의 경우 스프링을 설계하거나 할때 사용되기도 한다.
- 반환타입은 메서드의 수행 결과인 반환값의 타입을 결정해준다. 만약 반환값이 없는 경우는 void를 사용하면 된다.
- 메서드명은 일반적으로 행위를 하는것이기 때문에 동사로 네이밍한다
- 매개변수(파라미터)의 개수는 제한이 없고, 순서대로 들어간다, 매개변수는 지역변수이다.

**구현부란?**

선언부 뒤 { } 사이 구현된 것을 말한다. 선언부에 값 또는 반환타입에 따라 수행할 구현 문장을 적는다. 반환타입이 없는 경우 return 반환값이 필요하다. return 값은 단하나의 값만 반환이 가능하다.

 또한 if~else의 경우는 각 구현 부분에 return 값을 해줘야 한다. 

```java
int method(){
	if(true){
		return 0;
	}else{
		return -1;
	}
}
```

선언부와 구현부 중에는 선언부를 잘 선언해도 구현하기가 쉽기 때문에 선언부가 더 중요하다고 생각된다. 

```java
    public static void main(String[] args) {
    /* void와 void가 아닌 경우의 메서드로 선언부와 구현부를 코드화 했다
    */
        program("선언부와구현부",1); 
        System.out.println(program("선언부와구현부",1,"no void"));
    }

    static private void program(String name, int programNum){ // 선언부
        System.out.println("프로그램명 : " + name + ", 프로그램번호 : " +programNum); // 구현부
    }

    static private String program(String name, int programNum, String type) { // 선언부
        return "프로그램명 : " + name + ", 프로그램번호 : " +programNum+ ", 타입 : " + type; // 구현부
    }
```
