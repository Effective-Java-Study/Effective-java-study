# item_10
## equals 는 일반 규약을 지켜 재정의 하라

```java
/*  기본적으로 equals를 재정의 하지 마라
    
    equals 는 기본적으로 객체의 내용이 동일한지 검증하는 메소드 이다 -> Call By Value 
    ==은 메모리 주소를 비교하는 기능이다 -> Call By Reference 
    그렇기에 객체간 논리적 값이 동일한지 비교하는 경우에 재 구현 여부를 따져 볼수 있는데 
    일반적으로 String, Integer 등의 값 객체가 아니라면 굳이 구현할 필요 없이 
    object.equals 메소드로도 충분히 내용을 비교 할 수 있다    
    하지만 private 나 패키지 전용 클래스 라서 외부에서 호출 되면 안되는 경우는 재정의 해 호출 방지를 작성할 순 있다.
         
    기본적으로 equals 는 다음과 같은 원칙을 따른다고 가정 할 수 있다
 */

// 대칭성: x와 y가 같다면 y와 x도 같다
// x.equals(y) = true
// y.equals(x) = true 
// 이 조건을 만족 해야 한다 

// 반사성: 자기 자신을 비교 할경우 언제나 참이어야 한다
// x.equals(x) = true 

// 추이성: x equals y, y equals z 라면 x equals z = true이다 
// x.equals(y) = true 
// x.equals(z) = true 
// y.equals(z) = true 

// 일관성: 몇번을 반복하든 같은걸 비교한다면 같은 결과를 내보내야 한다.
    Integer data = 0; 
    for(int i; i=0; i;){
        data.equals(i); // always true
            }

// Null이 아님: null이 아닌 x를 대상으로 모든 참조값 x에 대해 null과 비교는 항상 false다  
// x.equals(null) = false // 비교연산이 null값과 동일 하면 안된다.
```