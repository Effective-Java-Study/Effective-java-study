# Effective Java Chap.2

<br/>

## 01. 생성자 대신 정적 팩토리 메소드를 고민하라.

타 언어와 다를 것 없이, 생성자를 통한 생성 방식을 취하고 있다.

#### Sample

-  How to Create

```java

public class Bank {
	private String name;
	private String code;

	public Bank(String name, String code) {
		this.name = name;
		this.code = code;
	}
	
	public buy(String action) {
		// Do Something...
	}
}

```

- How to Use

```java

Bank ShinhanBank = new Bank("신한은행", "0032")

Bank WooriBank = new Bank("우리은행", "0021")

```

<br/>

물론, 위와 같은 방법도 좋지만 **정적 팩토리 메소드** 방식을 이용한다면 좀 더 유연한 생성을 할 수 있다.

- How to Create

```java

public class Bank {
	private String name;
	private String code;

	private Bank(String name, String code) {
		this.name = name
		this.code = code
	}

	public static createBankwithCode(string code) {
		if something...

		return new Bank(...something with logic)
	}
}

```

- How to Use

```java

Bank ShinhanBank = Bank.createBankwithCode("0032")

Bank WooriBank = Bank.createBankwithCode("0021")

```

<br/>

사실, 위 처럼만 놓고 보면 여러가지 의문이 생길 수 밖에 없다.

> 뭐가 유연하다는 건지?

> 그래서 저렇게 하면 뭐가 장점인건지?

> 오히려 복잡한 체이닝이 일어나는건 아닌지?

<br/>

이 의문에 대한 일부 해답은 아래 조건들을 추가해서 클래스를 생성해야할 때 알 수 있다.

- 생성에 필요한 정보가 유동적일 때
	- 은행 코드만 가지고 생성할 때
	- 은행 이름만 가지고 생성할 때 등등...
- 자주 생성되는 객체이나 특정 조건에 의해 특정 값을 지닌 형태가 반복적으로 생성될 때
	- 객체 생성을 제한(제어)할 수 있다.
- 하위 객체에 대한 별개 타입의 객체를 반환해야 할 때

<br/>

그리고, 객체 생성 자체를 **캡슐화**한다는 측면에서 접근하면, 위와 같은 방식이 깔끔하게 느껴질 것이다.

또한, 위와 같은 정적 팩토리 메소드에는 일반적인 네이밍 컨벤션이 정해져 있으니 해당 내용을 숙지하는 것도 좋다.

- from : 하나의 매개변수를 받아 생성
- of : 여러개의 매개변수를 받아 생성
- genInstance | instance : 인스턴스 생성, 이전에 반환한 것과 같을 수 있음
- newInstance | create : 새로운 인스턴스 생성
- get[OtherType] : 다른 타입의 인스턴스 생성, 이전에 반환한 것고 같을 수 있음
- new[OtherType] : 다른 타입의 새로운 인스턴스를 생성

<br/>

추가로, 위와 같은 방식을 고려하기 전에, 해당 방식을 도입하고자 하는 클래스가 **어떤 책임**을 지고 있는지 판단하는 것이 좋다.

단순히 객체 생성만을 책임지고 있는 클래스라면 위와 같은 방법이 좋은 선택지가 될 수 있으나, 그보다 더 복잡한 비즈니스 로직을 품고 있는 경우 **단순한 접근 방식**을 지양해야 한다.

<br/>

## 02. 생성자에 매개변수가 많다면 빌더를 고려하라.

자바에서 클래스를 띄울 때, 생성자의 매개변수는 일반적으로 다음과 같이 정의된다.

```java

public class Bus {
	private String driverName;
	private String busName;
	private String busCode;

	public Bus(String driverName, String busName, String busCode) {
		this.driverName = driverName
		this.busName = busName
		this.busCode = busCode
	}

}

```

굉장히 무난해 보인다.

자, 여기서 몇개를 더 추가해보자.

```java

public class Bus {
	private String driverName;
	private String busName;
	private String busCode;
	private String busCompany;
	private String driverCompany;
	private String driverLicenseCode;
	private String driverLicenseName;
	private String budLicenseCode;
	private String buyType;

	public Bus(...생략) {...생략}
}

```

생성자에 주입되어야 할 녀석들이 굉장히 많아졌다.

이제 여러분은 Bus를 생성할 때 마다 매번 적합한 값을 적합한 순서에 맞게 넣어줘야 한다.

> 괜찮아요!

라고 대답할 사람도 물론 있겠지만, 내가 짠 코드가 아닌 **남이 짠 코드**여도 괜찮을까?

내가 짠 코드를 **남이 봤을 때**도 괜찮을까?

확신할 수 없다면, 생성 패턴을 바꿔보자.

### 잠깐!

> **빌더 패턴 생성 방식은 여러가지가 있으니, 클래스에 빌더 패턴을 생성하는 방식에 관한 설명은 제하겠습니다.**

```java

// 기존 방식대로 했을 경우 생성 방식

Bus originBus = new Bus("22", "33" ... etc)


// 빌더 방식으로 했을 경우 생성 방식

Bus newBus = new Bus("Essential").driverName("Park").(...).build()


```

두 코드를 보았을 때, 인자가 많을 경우에는 확실히 아래 체이닝 방식이 가독성과 명확함 측면에서 유리함을 알 수 있다.

추가적으로, 빌더 패턴의 핵심 중 두가지를 짚고 넘어가자.

- **필수 인자**와 **추가 인자**를 구분할 수 있다.
- 기본값 할당이 편리해서, **확장 및 변경**이 편리하다.

물론, 단점도 짚고 넘어가자.

- 코드가 복잡해질 수도 있다.
- 일반적인 생성 방식보다 성능이 떨어진다 (미미하게)

<br/>

## 03. private 생성자나 열거 타입으로 싱글턴임을 보증하라.

이 부분은 크게 강조할 것이 없어 간단하게 짚고 넘어가자.

자바에서 싱글턴을 사용하는 몇몇 방식들이다.

```java

public class Dog {
	public static final Dog doggy = new Dog();

	private Dog() {}
}

```

```java

public class Dog {
	public static final Dog doggy = new Dog();
}

	private Dog() {
		// someThing with Block !
	}
```

```java

public class Dog {
	private static final Dog doggy = new Dog();
	
	public static Dog getInstance() {
		return doggy
	}

	private Dog() {}
}
```

```java

public enum Dog {
	doggy;
}

```

각 방식들 마다, 장단점이 존재하며, 해당 장단점을 커버 하고자 추가된 것들이 존재한다.

하지만 언제나 그렇듯이, 완벽한 해답은 없다.

<br/>

## 04. 인스턴스화를 막으려거든 private 생성자를 사용하라.

인스턴스화를 막으려면 private을 갖다 쓰자!

```java

public class Dog {
	private Dog() {
		throw new AssertionError();
	}
}

```

별 내용 없다.

<br/>

## 05. 자원을 직접 명시하지 말고, 의존 객체 주입을 사용하라.


클래스가 내부적으로 하나 이상의 특정 자원(Resource)에 의존한다면, 해당 자원을 외부에서 주입하는 방식을 고려하는 것이 좋다.

> 그게 뭔가요!

코드를 보면 간단하게 이해될 수 있다.

```java

public class Engine {
	private String name;
	private String type;

	public Engine(String name, String type) {
		this.name = name
		this.type = type
	}
}

```


```java

public class Car {
	private Engine engine;
	private String name;

	public Car(Engine engine, String name) {
		this.engine = engine
		this.name = name
	}
}

```

Car 객체가 필요로하는 인자를 Engine으로 정의하여 넘겨주었다.

지금은 단순히 인자값만 정의하여 넘겨주었지만, 비즈니스 로직과 같이 복잡한 로직들이 얽히게 될 경우, 위와 같은 방식을 사용한다면 **의존 관계 역전**과 같은 다분히 **OOP**적인 개념들을 이용할 수 있다.

즉, 간단한 예시를 들자면, 필요 인자값을 외부에 의존함으로써 하나의 객체가 **생성 검증**과 **로직 수행**이라는 두개의 책임을 지게 되는것을 회피할 수 있다는 것이다.

<br/>

## 06. 불필요한 객체 생성을 피해라.

클래스 내에서 불필요하게 생성되는 객체들을 주의해야 한다.

```java

public class Man {
	// 지속 생성
	String letterFactory = new String("wow")

	// 단일 유지
	String letterSingleton = "wow"
}

```

단순히 고정된 값을 지속 사용하는 것이라면, 불필요한 new 사용을 피하는 것이 좋다.

<br/>

## 07.
