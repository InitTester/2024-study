변수의 3가지는 클래스 변수, 인스턴스 변수, 지역 변수가 있다.  선언한 위치에 따라서 스코프(영역)가 달라진다.  

**멤버 변수는 무엇인가 ?**

멤버 변수는 클래스 변수와 인스턴스 변수가 있고 클래스 내에 선언된다. 

**클래스 변수**는 static 키워드를 사용한 변수 이고, 모든 인스턴스간에 공유가 되기 때문에 해당 클래스로 생성된 모든 객체에서 동일한 값을 공유한다.

static 키워드를 사용했기 때문에 클래스가 메모리에 로드될 때 생성되고, 프로그램 종료 될 때까지 유지된다.(객체 생성 없어도 사용가능)

**인스턴스 변수**는 클래스 내에 선언된 객체의 상태를 나타내는 변수이다. 객체가 새로 생성될 때마다 별도의 메모리가 할당된다.

```java
public class test {
    static int cnt; // 클래스 변수

    public static void main(String[] args){
        test t = new test(); // 객체 생성

        int cnt2 = t.cnt; // 인스턴스 변수로 변경

        cnt++;
        System.out.println("cnt = " + cnt);
        System.out.println("cnt2 = " + cnt2);

        System.out.println("cntAdd() = " + cntAdd());
        System.out.println("cnt = " + cnt);
    }

    static int cntAdd(){
        return ++cnt;
    }
}
```

**지역 변수는 무엇인가?**

지역 변수는 메서드 안에서 선언되고 선언된 코드 블록 영역에서만 사용 가능하고 해당 메서드 실행이 완료되면 메모리에서 삭제 된다.

```java
public class test {
    int cnt = 5;
    public static void main(String[] args){
        test t = new test();
        t.변수값확인메서드();
    }

    public void 변수값확인메서드(){
        int cnt = 10;
        System.out.println("지역 변수  = " + cnt);
        System.out.println("인스턴스 변수  = " + this.cnt);
    }
}
```
