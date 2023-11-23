![](https://velog.velcdn.com/images/yeomyaloo/post/0bdfcc56-6e47-47dd-a5a2-71a16f1d5b55/image.png)
# [4장 클래스와 인터페이스]
## 아이템 20. 추상 클래스보다는 인터페이스를 우선하라
- 추상 클래스와 인터페이스를 차이를 알아보자.
- 그리고 우리는 왜 인터페이스를 더 우선으로 해야하는지 근본적 이유를 살펴보자.

> **[핵심 정리]**<br>
일반적으로 다중 구현을 할 때는 인터페이스가 적합하다. 상속은 다중 구현을 금지하기 때문이다.<br> 
복잡한 인터페이스라면 골격 구현을 이용해서 수고를 덜어보자<br>
골격 구현의 경우엔 가능한 한 인터페이스의 디폴트 메서드를 제공해 인터페이스를 구현한 모든 곳에서 활용할 수 있도록 하는 것이 좋다.

## [추상 클래스와 인터페이스]
### 1. 코드 비교
``` java
public abstract class AbstractClass {

	private String field1;
	
	void method1() {
	}
	
}
```
- 추상 클래스의 경우엔 메서드 바디가 필요하다.
- 추상 클래스는 멤버 필드를 가질 수 있다.
- `extends` 키워드를 사용해 해당 추상 클래스를 기준으로 구현체를 구현할 수 있다.


``` java

public interface InterfaceClass {

	void method1();
}

```
- 인터페이스는 멤버 필드를 가질 수 없다.
- 인터페이스의 경우엔 메서드 바디를 작성하면 안 된다.
- `implement` 키워드를 사용해 해당 인터페이스를 구현할 수 있다.

### 2. 설계도로 비유해보자
- 추상 클래스의 경우엔 완성된 설계도라고 생각하면 된다. 기본 골격이 짜여져 있어서 이를 extends 할 경우에 이에 맞게 기능을 구현해주어야 한다.
- 반면 인터페이스의 경우엔 미완성 설계도를 생각하면 되는데 이는 메서드 바디도 구현하고자 하는 개발자가 전부 작성하고, 필드도 필요한 것만 추가해서 사용하면 된다.

## [인터페이스의 디폴트 메서드 제공]
### 1. java 8부터 인터페이스에 디폴트 메서드 사용을 제공한다.
- 기존 상속에서만 가능하던 디폴트 메서드를 `자바8`부터는 인터페이스에서도 제공을 한다. 
- 이는 상속과 인터페이스가 유사해진다는 의문을 가져올 수 있지만 자바의 상속은 다중 구현을 금지하고 있기 때문에 큰 변화가 있다고 볼수 있다.
- 인터페이스에 디폴트 메서드가 들어감으로 인터페이스를 implements 하는 경우에 모든 메서드를 구현해야 한다는 번거로움이 사라지게 된것이다.
- 결과적으로는 인터페이스에 디폴트 메서드 기능을 제공함으로서 인터페이스의 단점인 모든 기능을 구현하는 것을 이겨내고 인터페이스는 상속의 단점인 다중상속을 장점으로 함께 가져간다.
  - 단일 상속이 단점이 되는 이유는 새로운 타입의 정의를 제약하고 있기 때문이다.
  - 인터페이스를 사용해서 구현한 경우라면 다른 어떤 클래스라도 같은 타입으로 취급한다.

## [인터페이스의 장점]
### 1.기존 클래스에 인터페이스를 구현해 넣을 수 있다.
- 기존 클래스가 있다고 할때 이 클래스가 인터페이스를 implements 하고 싶다면 하면 된다. 
- 이때 기존 클래스 내에서는 인터페이스에서 구현하고자 하는 기능이 없어야 한다.
```java
public class BasicClass implements interfaceClass{

	//기존 클래스의 멤버들..
    
    //구현하고자 하는 인터페이스의 기능들.. (이미 기존 클래스에 있으면 안 된다)
}
```

**[기존 클래스에 인터페이스를 구현해 넣은 예시 상황]*
- Comparable, Iterable, AutoCloseable 인터페이스가 추가 됐을 때 표준 라이브러리의 수많은 기존 클래스들이 위의 해당하는 인터페이스들을 구현한 채로 릴리스(=배포) 한 경우

**[기존 클래스에 추상 클래스를 확장한다면?]**
- 기존 클래스가 어떠한 추상 클래스를 확장하길 원한다면 해당 기존 클래스의 하위 클래스 역시 모두 추상 클래스를 확장해야 하는 문제가 생긴다.
- 이는 적절하지 않은 상황에서도 강제가 되기 때문에 적절하지 않다. 

### 2. 인터페이스는 믹스인이 안성 맞춤이다.
#### 2-1. 믹스인(Mixed in)이란?
- 다른 클래스에서 사용할 목적으로 만들어진 클래스를 믹스인이라고 표현한다.
- 포함의 개념으로 상속과는 다른 개념이다.
- 코드의 재사용성을 높여주고 상속의 단점을 해결할 수도 있다.
- 다중 상속 제한이 없는 인터페이스로 구현하기에 용이하다.

~~~ java
public interface SInger {

}
~~~

~~~java

public interface SongWriter {

	Song compose(int chartPosition);
	
}

~~~

~~~ java
public interface SingerSongWriter{

	 AutoClip sing(Song s);

}
~~~

### 3. 인터페이스는 계층 구조 없는 타입 프레임워크를 만들 수 있다.
#### 3-1. 타입을 게층적으로 표현할 때
- 타입을 계층적으로 표현할 때 수많은 개념을 구조적으로 잘 표현할 수 있지만 현실에서는 계층을 엄격히 구분하기 어려운 개념도 있다.

#### 3-2. 위의 Singer, SongWriter, SingerSongWriter가 타입을 계층적으로 표현하지 않은 코드 예제이다.
- 이 경우엔 가수가 작곡도하고 노래도 부르는 경우가 많기 때문에 엄격하게 작곡가와 가수를 나눌 수 없는 상황을 떠올려보자.
- 이 경우엔 모두를 다 확장하여 사용할 수 있을 정도로 유연해진다.

### 4. 인터페이스는 기능을 향상시키는 안전하고 강력한 수단이 된다.
- 앞서 살펴본 상속보다는 컴포지션을 사용해라는 챕터를 떠올리면 래퍼 클래스가 상속보다 훨씬 안전하고 성능이 좋은 것을 알수 있다.
- 또한 상속을 위해서는 무조건 메서드를 문서화 하는 작업을 진행해야 하는데 인터페이스는 구현하지 않아도되는 메서드를 디폴트 메서드화 시켜서 해당 문제를 해결했다.

## [인터페이스와 추상 골격 구현]
### 1. 인터페이스와 추상 클래스의 장점을 모두 취하는 방법
- 인터페이스로 타입을 정의, 필요에 따라 디폴트 메서드를 몇개 제공하고 골격 구현 클래스로는 나머지 메서드를 구현하면 된다.
- 이렇게 해두면 단순히 골격 구현을 확장하는 것만으로 인터페이스를 구현하는 일이 대부분 완료 된다.
~~~java
// 골격 인터페이스
interface Recipe {
    void cookRecipe(); // 템플릿 메서드
}

// 추상 골격 클래스
abstract class AbstractRecipe implements Recipe {
    // 템플릿 메서드 정의
    @Override
    public void cookRecipe() {
        prepareIngredients();
        cook();
        serve();
    }

    // 추상 메서드들
    protected abstract void prepareIngredients();
    protected abstract void cook();
    
    // 기본 구현된 메서드
    protected void serve() {
        System.out.println("Serve the dish!");
    }
}

// 구체적인 레시피 클래스
class PastaRecipe extends AbstractRecipe {
    @Override
    protected void prepareIngredients() {
        System.out.println("Gather pasta, sauce, and cheese.");
    }

    @Override
    protected void cook() {
        System.out.println("Boil pasta, add sauce, sprinkle cheese, and cook for 10 minutes.");
    }
}

class PancakeRecipe extends AbstractRecipe {
    @Override
    protected void prepareIngredients() {
        System.out.println("Gather flour, eggs, milk, and sugar.");
    }

    @Override
    protected void cook() {
        System.out.println("Mix ingredients, pour onto pan, cook each side until golden brown.");
    }
}

public class Main {
    public static void main(String[] args) {
        Recipe pastaRecipe = new PastaRecipe();
        pastaRecipe.cookRecipe(); // 템플릿 메서드 호출

        System.out.println();

        Recipe pancakeRecipe = new PancakeRecipe();
        pancakeRecipe.cookRecipe(); // 템플릿 메서드 호출
    }
}
~~~
- 해당 작업을 진행하면 Interface의 타입을 정의하고 골격 구현 클래스에서 나머지 메서드를 구현해 해당 인터페이스의 구현에 필요한 일이 대부분 완료된다.
- 골격 구현 클래스는 추상 클래스 처럼 구현을 돕는 동시에 추상 클래스로 타입을 정의할 때 따라오는 심각한 제약(= 다중 상속을 막는 등의 일)에서는 자유롭다.
- 인터페이스의 구현을 골격 구현 클래스가 대신하고 있음을 알수 있다.

#### 1-1. 골격 구현 클래스
- 추상 클래스에 원하는 타입의 인터페이스를 확장한다. 
- 메서드의 구현을 진행한다.
- 구현이 필요하지 않은 메서드라면 디폴트 메서드화 시켜서 골격 구현 클래스에서 이를 신경쓰지 않아도 되게 한다.


> 인터페이스의 경우엔 다른 인터페이스를 확장하고자 할때 상속 받을 때 사용하는 키워드를 사용해야 한다. 