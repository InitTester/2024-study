
스터디 중 Stack과 Heap에 대해 이야기가 나왔다. 일반적으로 Stack과 Heap은 메모리 관리와 객체의 생명 주기를 관리 하기 위해 사용한다고 한다. 그렇다면 메모리 관리는 왜 필요한 걸까? 메모리 관리는 시스템의 자원을 효율적으로 활용하고 프로그램의 안전성과 신뢰성을 보장해주기 위해 필요하다. 쉽게 이야기하면 누군가 만든 앱을 구형이든 신형이든 사양에 상관없이 마구마구 메모리를 사용한다면 구형 기기를 가진 사람들은 해당 프로그램을 제대로 사용하지 못할 것이다.

그럼 다시 처음으로 돌아가서 JAVA는 어떻게 사용 할 수 있는 것일까? 왜 Stack과 Heap을 쓸까? 어디에 쓰이는 걸까? JAVA를 배우면서 JVM, 메모리, Stack, Heap 이라는 이야기를 많이 들어봤다.



그럼 우리가 배우고 있는 JAVA의 장점은 무엇인가 ? 플랫폼에 독립적인 언어(OS 영향을 받지 않는다)라는 점이 있다.

JVM은 나무위키 설명처럼 JAVA로 개발한 프로그램을 구동하기 위한 가상 기계이다. 구조에서 보면 클래스 로더가 컴파일된 바이트 코드를 메모리 영역(메소드, 힙, 자바스택, PC레지스터, 네이티브 메소드 스택)에 로드 하게 된다.



**JVM(Java Virtual Machine)**
> 
개요 
Java Virtual Machine(자바 가상 머신)은 Java로 개발한 프로그램을 컴파일하여 만들어지는 바이트코드를 실행시키기 위한 가상머신이다. JRE(Java Runtime Environment)에 포함되어 있으며, Java 컴파일러가 프론트엔드를 담당한다면 Java 가상 머신은 코드 최적화와 백엔드를 담당한다(웹이 아닌 컴파일러의 프론트엔드/백엔드를 말하는 것이다).
구조
![](https://velog.velcdn.com/images/initsave/post/0c074b0b-9b4e-4170-b7b6-3dab0ac8ee74/image.webp)
https://namu.wiki/w/Java%20Virtual%20Machine



그렇다면 메모리 영역 중 Stack과 Heap은 무엇인가 ?


**Stack이란 ?**

스택은 한 쪽 끝에서만 자료를 넣거나 뺄 수 있는 선형 구조(LIFO - Last In First Out)으로 되어 있다. 자료를 넣는 것을 '밀어넣는다' 하여 푸쉬(push)라고 하고 반대로 넣어둔 자료를 꺼내는 것을 팝(pop)이라고 하는데, 이때 꺼내지는 자료는 가장 최근에 푸쉬한 자료부터 나오게 된다. 이처럼 나중에 넣은 값이 먼저 나오는 것을 LIFO 구조라고 한다.

참조 : https://ko.wikipedia.org/wiki/스택



스택은 쌓는다는 의미이고, 데이터를 쌓는 이유는 계산하기 쉽기 때문이다.

스택은 **정적으로** 할당된 메모리 영역이고 메서드가 호출되고 처리하는 영역은 Stack에서 처리된다. 또한 Heap에 저장된 객체의 주소값을 저장하고 있다. 메서드가 수행을 마치고 나면 사용했던 메모리를 반환하고 스택에서 해당 메서드는 제거된다. 스택은 접근이 빠르고 메모리 할당 및 해제가 간단하고 컴파일 타임에 크기가 결정된다.

스택의 메모리는 쓰레드당 하나씩 할당된다. 하지만 스택을 사용하는 프로그램을 오래 실행하다 보면 점차 가비지 컬렉션 활동과 메모리 사용량이 늘어나 결국 성능 저하가 될 것이다.

```
public class StackTest {
    public static void main(String[] args) {
        메서드호출();
    }

    static void 메서드호출(){
        메서드호출2();
    }

    static void 메서드호출2(){
        System.out.println("메서드 호출중");
    }
}
```


참조 : https://www.betsol.com/blog/java-memory-management-for-java-virtual-machine-jvm/
참조 : 이펙티브 자바



**Heap이란 ?**

힙은 **동적으로** 생성된 객체를 저장하는데 사용되고, 클래스가 인스턴스화 하면서 new키워드를 사용하면 힙에 올라가게 된다. 힙은 자유롭게 메모리에 할당/해제가 가능하고 동적 메모리 영역으로, 객체의 크기나 생명 주기가 동적으로 변할 수 있다. 사용가능 메모리공간은 스택보다 크고 런타임시 크기가 결정된다. 일정 시간이 지나 Heap영역에 있는 인스턴스를 참조하지 않으면 GC에서 처리한다.

힙은 스레드 개수와 무관하며, 개수는 1개이다. Object 타입의 데이터가 저장되고 레퍼런스 변수는 stack에 저장된다.
```
public class HeapTest {
    public static void main(String[] args) {
        /*
        * HeapClass를 동적으로 생성해서 힙영역에 할당,
        * new(인스턴스화)를 통해 새로운 객체는 힙영역에 저장
        */

        // 생성된 객체의 메서드 호출
        HeapClass hc = new HeapClass();
        hc.printMsg();
    }
}
```

```
class HeapClass{
    public void printMsg(){
        System.out.println("Heap!!");
    }
}
```

Stack과 Heap은 클래스 수준의 정보는 스택에 저장되지만 객체의 인스턴스 값은 힙영역에 생성된다. 스택영역에는 힙영역의 주소값(메서드)을 저장한다.



**Stack / Heap 강제 에러 내기**

에러 부분은 참조 사이트에서 그대로 가져 왔다. 테스트를 해볼 수는 있지만 노트북 성능을 고려해서 .. 예시를 보는 것으로 만족한다!



**Stack**
```
public class StackOverflowErrorExample
{
    private static void addItself(int i)
    {
        addItself(i+i);   //calling itself with no terminating condition
    }
     
    public static void main(String[] args) 
    {
        addItself(10);
    }
}
```

**Heap**
```
public class OutOfMemoryErrorExample 
{   
    public static void main(String[] args)
    {   
        //Creating new objects in never ending loop
         
        for (int i = 1; i > 0; i++)
        {
            new Object();
        }
    }
}
```

![](https://velog.velcdn.com/images/initsave/post/335002ed-0694-4faf-8f36-b13f85bb16ee/image.png)






참조 : https://javaconceptoftheday.com/stackoverflowerror-vs-outofmemoryerror-in-java/





혹시나 이 글을 누군가 읽으시고 틀린점이 있으면 언제든지 가르쳐주시면 감사하겠습니다!
