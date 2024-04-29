우리가 자주 듣는 클래스, 객체, 인스턴스는 무엇일까? 

자바에서 이야기하는 **클래스는** 객체를 만들어 내기 위한 설계도, 변수와 메서드로 이루어진 집합이고 **객체는** 클래스에 사용하여 객체를 생성할 때는 “new” 키워드를 사용한다. 이 과정을 인스턴스화라고 하며 이 과정을 통해 메모리에 할당되고 이 결과로 해당 클래스의 **인스턴스**(객체)가 생성된다.

그렇다면 클래스는 인스턴스 인가 ? 아니다 클래스는 단순히 클래스이고 인스턴스화를 할수 있다는 전제 조건이 있다. 인스턴스는 클래스가 있어야 인스턴스를 만들 수 있다. 클래스는 인스턴스를 만들기 위해 존재 하는 것이다. 만약 “new” 키워드를 사용하지 않으면 클래스는 인스턴스가 되지 않는다.

~~클래스, 객체, 인스턴스는 동일 하지만 조금 다르다~~

```java

// Cat 이라는 클래스
class Cat{
    private String name;
    private int age;

     Cat(String name, int age){
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "이름은 '" + name + '\'' +
                "이고, 나이는 " + age +"살 고양이 입니다." ;
    }
}

public class main {
    public static void main(String[] args) {
        Cat 첫째 = new Cat("아치",8); // new 를 통해 인스턴스 생성
        Cat 둘째 = new Cat("코야",7);

				// 인스턴스의 toString() 메서드 출력
        System.out.println("첫째의 " + 첫째);
        System.out.println("둘째의 " + 둘째);
    }
}

```
