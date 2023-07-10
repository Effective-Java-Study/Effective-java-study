# item_11
## equals 를 재정의 했다면 hashcode 도 재정의 하라
    equals를 재정의 했다면 hashcode도 재정의 해야 한다 
    그렇지 않으면 인스턴스를 HashMap이나 HashSet같은 Hash컬렉션의 원소로 사용할때 문제가 발생한다 

## hashcode 일반 규약 
    *equals 비교에 사용되는 정보가 변경되지 않았다면, hashCode 도 변하면 안 된다.
        -> 애플리케이션을 다시 실행한다면 이 값이 달라져도 상관 없음

    equals가 두 객체가 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환한다.
        -> 논리적으로 같은 객체는 같은 해시코드를 반환해야 한다.

    equals가 두 객체를 다르다고 판단했더라도, hashCode는 꼭 다를 필요는 없다.
        -> 하지만, 다른 객체에 대해서는 다른 값을 반환해야 해시테이블의 성능이 좋아진다.
        -> 같은 해쉬(키) 안에 여러값이 존재하는 경우를 의미

## hashcode 팁 
    물리적으로 다른 두 객체를 논리적으로 같다(값이 같다)라고 할때 hashcode는 다른 값을 반환한다 
    
```java
Map<String, Integer> map = new HashMap<>();
String value = "test";
map.put(value, 1) // e.g. hash값 = 30 value = 1 
map.get("test") // e.g. hash값 = 40 value = null
```
    논리적으로 value와 "test"가 논리적 동치라도 Hash함수는 기본적으로 서로 다른 엔트리 끼리는 
    동치성 비교를 하지 않도록 최적화 되있다 
    e.g. 
    @Override
    public hashcode() {
        return 30; 
    } -> 항상 같은 hash값을 반환하게 되어 한 hash에 모든 값이 삽입되어 
    O(1) -> O(N)으로 탐색시간이 길어진다
