# item_12
## toString 은 항상 재정의 해라 
    toString을 재정의 하지 않으면 
    객체_이름@16진수_해시코드(메모리_주소) 만 나와 의미 없는 정보가 나온다 
    
    디버깅을 위해서 의미 있는 정보를 출력하도록 재정의 해라
```java
static class Test {
    public String name; 
    public String age; 
    
    @Override 
    public String toString() {
        return String.format("이름: %s 나이:%s", name, age)
    } // -> 유의미한 정보 제공 Good!
}
```