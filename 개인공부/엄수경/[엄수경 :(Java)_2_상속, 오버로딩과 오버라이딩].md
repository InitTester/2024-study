### 6. 상속 (is-a)
'is-a'관계를 갖는 것. 부모-자식 관계를 만드는 것.  
기존 클래스를 재사용하여 새로운 클래스를 만드는 것이다. 
사람 extends 동물 (사람 is a 동물)

### 7. 오버로딩과 오버라이딩

- 오버로딩 : 같은 이름의 메서드를 선언하는 것
1. 메서드의 이름이 같아야 한다.
2. 매개변수의 타입이나 개수가 달라야 한다.
3. 반환타입은 관련 없다.

- 오버라이딩 : 조상으로부터 상속받은 메서드의 내용을 자신에게 맞게 변경하는 것
1. 메서드의 선언부가 같아야 한다.
2. 조상보다 더 많은 예외를 선언할 수 없다.
3. 조상보다 접근제어자의 범위가 더 좁을 수 없다.

```java
public class Latte {  
    String espresso;
    String milk;
    //에스프레소 추출하기
    void extract(String espresso) {
        this.espresso = espresso;
        System.out.println(espresso + "Espresso 추출하기");
    }
    //스팀하기
    void steam(String milk) {
        this.milk = milk;
        System.out.println(milk + "우유 스팀하기");
    }
}
```

```java
public class LightLatte extends Latte {
    @Override
    void extract(String espresso){
        System.out.println("연하게 Espresso 추출하기");
    }
}
```
오버라이딩 - 조상메서드와 선언부가 같아야 한다.

```java
public class MocaLatte extends Latte {
    String milkForm;
    String chocoSyrup;
    //오버로딩
    void topping(String milkForm) {
        this.milkForm = milkForm;
        System.out.println("밀크폼 올리기");
    }
    void topping(String chocoSyrup, int count) {
        this.chocoSyrup = chocoSyrup;
        System.out.println(chocoSyrup + "시럽 " + count + "번 추가");
    }
}
```
오버로딩 - 매개변수의 타입 또는 개수가 달라야 한다.

LightLatte는 Latte이다. (is-a 관계 성립)
MocaLatte는 Latte이다. (is-a 관계 성립)
