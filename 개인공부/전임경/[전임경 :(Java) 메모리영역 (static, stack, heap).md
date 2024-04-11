참조 : [개인공부/전임경/[전임경 :(Java) Q. JAVA는 왜 Stack과 Heap을 쓸까.md](https://github.com/InitTester/2024-study/blob/main/%EA%B0%9C%EC%9D%B8%EA%B3%B5%EB%B6%80/%EC%A0%84%EC%9E%84%EA%B2%BD/%5B%EC%A0%84%EC%9E%84%EA%B2%BD%20%3A(Java)%20Q.%20JAVA%EB%8A%94%20%EC%99%9C%20Stack%EA%B3%BC%20Heap%EC%9D%84%20%EC%93%B8%EA%B9%8C.md)


몇 일 전에 정리한 메모리 영역 중  static(method area)에 대한 설명은 하지 않았다. 멤버변수 중 클래스 변수(static variable)에 대해서 이야기 할때 “클래스가 메모리에 로드될 때 생성된.. “ 라고 정리한 적이 있다. Static(Method) 영역은 클래스 변수, static으로 선언된 것들이 메모리영역에 저장된다.

그렇다면 어떤 특징들이 있을까 ? 

- JVM이 실행되고 클래스가 메모리에 로드될 때 생성(프로그램이 종료할때 메모리에서 해제)
- 클래스변수, 생성자, 메소드 같은 것들을 저장
- Static 영역에 있는 것은 어디서든 접근 가능(static 메서드인 경우 에서 객체 생성 없이 사용 가능)

```java
public class staticArea {
    public static void main(String[] args) {
        staticMethod();
    }
    static void staticMethod(){
        System.out.println("static 메서드는 new 생성 없이 사용 가능 !!");
    }
}

```
