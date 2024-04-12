객체지향 프로그래밍(Object-Oriented Programming, OOP)에서 중요한 핵심 4가지 개념은 

**추상화, 캡슐화, 상속, 다형성**이 있다. 여기서 **OOP**는 어떤걸까? 순서보다는 기능(메서드)을 중요시하고 특정 기능(메서드)을 수행하는 대상(인스턴스) 사이의 관계가 중요하다.

- 추상화(Abstraction) : 행위 자체는 동일하지만 구현이 다를 경우 공통된 특성을 추출해서 정의한다. 추상화를 통해서 코드의 이해와 유지보수가 쉬워진다.
    
    ~~객체의 공통성과 본질을 모아 추출하여 정의~~
    
    ```java
    abstract class 동물 {
        abstract void 먹다();
    }
    
    class 사람 extends 동물 {
        @Override
        void 먹다() {
            System.out.println("밥을 먹습니다.");
        }
    }
    
    class 고양이 extends 동물 {
        @Override
        void 먹다() {
            System.out.println("사료를 먹습니다.");
        }
    }
    ```
    

- 캡슐화(Encapsulation) : 정보위변조 방지를 위해 직접 접근을 막는다. 직접접근을 막기 위해 private 사용한다. 캡슐화는 외부직접접근을 막지만 getter/setter를 사용한다.
    
    ~~클래스 안에 서로 연관있는 속성과 기능들을 하나의 캡슐로 만들어 데이터를 외부로부터 보호하는 것~~
    
    ```java
    class 지갑 {
        private int 금액;
    
        public int get금액() {
            return 금액;
        }
    
        public void set입금(int 입금액) {
            this.금액 += 입금액;
        }
    
        public void set출금(int 출금액) {
            if(this.금액 >= 출금액) {
                this.금액 -= 출금액;
            }else {
                System.out.println("잔액이 부족합니다.");
            }
        }
    }
    ```
    

- 상속(Inheritance)
    
    상속은 직접적인 연관관계( is ~ a )가 있어야 하고 자식클래스가 부모클래스를 상속하는 경우 부모클래스의 코드를 재사용하고 코드중복을 줄일 수 있고 클래스 간의 관계를 명확하게 할 수 있다.
    
    ~~기존의 클래스를 재활용하여 새로운 클래스를 작성하는 것~~
    
    ```java
    class 동물 {
        void 소리() {
            System.out.println("소리를 낸다.");
        }
    }
    
    class 고양이 extends 동물{
        @Override
        void 소리() {
            System.out.println("야오오야오오");
        }
    }
    ```
    

- 다형성(Polymorphism) :  is~a관계가 기본이다, 다형성은  오버로딩과 오버라이딩을 통해 구현되며, 같은 이름의 메서드가 다른 동작의 구현을 수행할 수 있다.
    
    ~~어떤객체의 속성이나 기능이 상황에 따라 여러가지 형태를 가질 수 있는 성질~~
    
    ```java
    class 동물 {
    	void 소리() {
    	System.out.println("소리를 낸다.");
    	}
    }
    
    class 고양이 extends 동물 {
    		@Override
    		void 소리() {
    		System.out.println("야오오야오오");
    	}
    }
    
    class Main {
    	public static void main(String[] args) {
    		동물 animal = new 고양이();
    		animal.소리(); // 다형성에 의해 고양이 클래스의 소리() 메서드가 호출됨
    	}
    }
    ```
    

# 인터페이스

인터페이스는 추상메서드의 집합이고, 작업지시서에 가깝다.  상속과 달리 여러개의 클래스 상속을 지원한다. 행위의 존속 관계성(선언만 되어 있고 구현부는 없다)이 없고 선언부가 동일하며 인터페이스의 추상메서드는 무조건 구현해야 한다.

```java
interface 동물 {
    void 소리(); // 추상 메서드
}

class 고양이 implements 동물 {
    @Override // 무조건 구현
    public void 소리() {
        System.out.println("야옹");
    }
}

class 개 implements 동물 {
    @Override // 무조건 구현
    public void 소리() {
        System.out.println("왈왈왈");
    }
}

class Main {
    public static void main(String[] args) {
        동물 animal1 = new Cat();
        동물 animal2 = new Dog();
        animal1.소리(); // 야옹
        animal2.소리(); // 왈왈왈 
    }
}
```
