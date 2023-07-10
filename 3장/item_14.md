# Comparable 을 구현할지 고려해라 

## 순서를 고려해야 하는 값 클래스의 경우는 꼭 Comparable 인터페이스를 구현 하라 
    ompareTo 메서드에서 필드의 값을 비교할 때 부등호 연산자(<,>)는 사용하면 안된다
    대신 Wrapper 클래스가 제공하는 static compare 메서드나 
    Comparator 인터페이스가 제공하는 비교자 생성 메서드를 사용하자

## compareTo의 일반 규약 
    1. compareTo는 동치성 비교뿐만 아니라 순서 까지 비교할 수 있다
    2. 제네릭 타입 이기때문에 형변환에 대해서 부담이 없다 
    위 두가지를 제외하고 equals의 규약과 비슷하다고 볼 수 있다. 
    반사성 x = y , y = x 
    대칭성 x = y , y = x -> x = z 
    추이성 x > y , y > z -> x > z 
    이 항목은 필수는 아니지만 이 항목들을 지키지 않을경우 이상하게 동작 할수 있다. 

    비교해야할 대상이 여러개라면 핵심 적인 대상부터 비교해보자 

## e.g.
```java
public class Person implements Comparable<Person>{
    private Integer age;
    
    @Override 
    public int compareTo(Person person) {
        return Integer.compare(this.age, person.age)
    }
    
    // or 
    public static final Comparator<Person> COMPARATOR = 
            comparing((Person person) -> person.age)
                    .thenComparing(person -> person.someting));

    @Override
    public int compareTo(Person person) {
        return COMPARATOR.compare(this, person);
    }
        }
```