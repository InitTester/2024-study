주소참조와 값참조는 메서드의 매개변수에 어떤 값을 복사/주소값을 참조 하는지에 따라서 달라진다. 쉽게 보면 매개변수의 타입이 **기본형** 데이터 라면 값을 참조하는거고 **참조형** 데이터면 인스턴스화 된 주소값을 참조해서 사용하게 된다.

자바의 데이터 타입

- 기본형(primitive type)
    
    원시타입은 실제 데이터 값이 저장하는 타입인 int, long, double, float, boolean, byte, short, char 이 있다. 스택(Stack) 메모리에 저장
    
- 참조형(reference type)
    
    참조타입은 메모리 번지 값을 통해 객체를 참조하는 타입인 Integer, Long, Double, Float, Boolean, Byte, Short, Character(래퍼클래스 : wrapper class)가 있다. 힙(Heap) 메모리에 저장, GC가 돌면서 메모리 해제
    

주소참조(Call by reference)란 ? 

데이터를 전달할때 주소 값을 던져주고 그 주소 값안에 있는 데이터를 사용한다.

값참조(Call by value) 란 ?

기본형인 값을 던져서 사용한다.

```java
public class test {
    public static void main(String[] args) {
    // 값 참조 
        int a = 1;
        int b = a;
        System.out.println("a = " + a);
        a = 20;
        System.out.println("a = " + a+ ", b = " + b);

		// 주소 참조  
        refer ref = new refer(1);
        refer ref2 = ref;

        System.out.println("ref.value = " + ref.value);
        ref.value = 2;
        System.out.println("ref.value = " + ref.value+ ", ref2.value = " + ref2.value);
    }
}

class refer{
    int value;
    public refer(int value){
        this.value = value;
    }
}
```
