![](https://velog.velcdn.com/images/yeomyaloo/post/41225c00-4755-445b-810a-2f466ced74cf/image.png)

# [4장 클래스와 인터페이스]

## 아이템 17. 변경 가능성을 최소화하라

- 불변 클래스를 만들기 위해서는 `final`키워드를 탑 레벨 클래스에 접근 제한자 뒤에 붙여 사용하자
- 앞서 살펴본 캡슐화와 관련된 개념은 클래스,인터페이스의 접근 제한자 / 멤버의 접근 제한자 사용의 내용이라면 해당 불변 객체와 관련된 개념은 `final` 키워드를 붙여서 사용하는 개념이라고 생각하고 들어가자.

> **[핵심 정리]**
> 불변 클래스를 사용하면 여러 이점이 있다 단점이라곤 잠재적 성능 저하라는 문제뿐, 그럼에도 불구하고 우리는 모든 클래스를 불변으로 만들어서 사용할 순 없다. 그렇다면 우리는 변경 가능한 부분을 최소화시켜야 한다. 별다른 이유가 없다면 멤버의 필드는 `private final` 형태로 작성하자

---

## [불변 클래스]

```java
public final class ImmutableClass {
	// 기본 불변 객체를 만드는 틀입니다.
    // final 키워드를 해당 클래스에 붙여주면 해당 객체는 불변 객체가 됩니다.
}

```

### 1. 불변 클래스란?

- 인스턴스 내부 값을 수정할 수 없는 클래스이다.
- 불변 클래스는 간직된 정보는 고정되어 객체가 파괴되는 순간까지 절대 변하지 않는다.

### 2. 불변 클래스 예시

- 자바에서는 BigInteger, BigDecimal등이 불변 클래스 중 하나이다.

### 3. 불변 클래스의 장점

- 가변 클래스에 비해 구현, 설계가 쉽다.
- 오류가 생길 가능성이 적다.
- 스레드 안전하여 따로 동기화가 필요하지 않다.
- 불변 객체는 변할 가능성이 없기 때문에 안심하고 공유할 수 있다.
- 불변 클래스의 경우 한번 만든 인스턴스를 최대한 재활용하기 좋다.

### 4. 불변 클래스를 만드는 5가지 방법

1. 객체 상태를 변경하는 메서드(변경자)를 제공하지 않는다.
   - setter와 같은 변경자 메서드를 제공하지 않으면 된다.
   - getter은 단순 접근자 메서드이기 때문에 제공해도 된다.
2. 클래스 확장을 막는다.
   - 이 말은 클래스의 접근 제한자 뒤에 final 키워드를 붙여주면 된다.
   - 상속을 막는 대표적인 방법이다.(클래스를 불변 객체로 만드는 것)
3. 모든 **필드**를 final로 선언한다.
4. 모든 **필드**를 private로 접근 제한자를 사용해 접근 범위를 가장 좁게 만든다.
   - 해당 필드가 가변 객체를 참조하고 있는 경우에 해당 객체를 클라이언트에서 직접 접근해서 수정하는 일을 막아준다.
   - 기본 타입이나 불변 객체를 참조하고 있는 필드를 public final로 필드를 만들어줄 경우엔 불변 객체가 되지만 다음 릴리즈(배포)에서 내부 표현을 개발자가 바꾸지 못하기 때문에 private를 사용하자
5. 자신 외에는 내부의 가변 컴포넌트에 접근할 수 없도록 한다.
   - 가변 객체를 참조하고 있다면 클라이언트가 해당 객체의 참조를 얻을 수 없도록 해야 한다.

---

## [예시 코드로 보는 불변 클래스]

### 1. 불변 복소수 클래스 코드

```java
// final 키워드로 불변 객체를 만들자.
public final class Complex {
    private final double re;
    private final double im;


    private Complex(double re, double im){
        this.re = re;
        this.im = im;
    }

    public static Complex valueOf(double re, double im){
        return new Complex(re, im);
    }

    public double realPart() {
        return re;
    }

    public double imaginaryPart(){
        return im;
    }

    public Complex plus(Complex c){
        return new Complex(re+c.re, im+c.im);
    }

    public Complex minus(Complex c){
        return new Complex(re-c.re, im-c.im);
    }

    public Complex times(Complex c){
        return new Complex(re*c.re - im*c.im, re*c.im + im*c.re);
    }

    public Complex divdedBy(Complex c){
        double tmp = c.re*c.re+c.im*c.im;
        return new Complex((re*c.re + im*c.im)/tmp,(im*c.re-re*c.im)/tmp);
    }

    @Override public boolean equals(Object o){
        if(o == this) return true;
        if(!(o instanceof Complex)) return false;
        Complex c = (Complex) o;

        return Double.compare(c.re, re) == 0 && Double.compare(c.im, im) == 0;
    }

    @Override
    public int hashCode(){
        return 31 * Double.hashCode(re) + Double.hashCode(im);
    }

    @Override
    public String toString(){
        return "("+re+" + "+im+"i)";
    }
}

```

### 2. 구성

1. 실수부와 허수부 값을 반환하는 접근자 메서드 `realPart()` `imaginaryPart()`
   - 해당 클래스에는 접근자 메서드(getter)만 있고 수정자 메서드(setter)는 존재하지 않음을 확인할 수 있다.
2. 사칙연산 메서드
   - 해당 메서드들은 인스턴스 자신을 수정하지 않으면서 새로운 Complex 인스턴스를 만들어 반환하고 있다.
   - 피연산자에 함수를 적용해 그 결과를 반환하고는 있지만 피연산자 자체는 그대로인 프로그래밍 패턴을 함수형 프로그램이라고 한다.
   - 절차적 혹은 명령형 프로그래밍의 경우엔 함수형 프로그래밍과 다르게 인스턴스 자신을 수정해 상태가 변하게 된다.
3. 메서드 이름을 동사(add)가 아닌 전치사(plus)로 작성해서 사용하자.
   - 해당 메서드가 객체의 값을 변경하지 않는 것을 강조하려는 의도이다.

---

## [불변 클래스가 적용된 예시]

### 1. 불변 클래스와 정적 팩터리 메서드

```java
package test;

import java.util.Hashtable;
import java.util.Map;

public final class Person {
    private static final Map<String, Person> personCache = new Hashtable<>();

    private final String name;
    private final int age;


    //private 생성자로 외부에서 해당 객체를 직접 생성하는 것을 막는다.
    private Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    //정적 팩토리 메서드를 사용해서 객체 생성에 관여한다.
    public static Person createPerson(String name, int age){

        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("이름은 필수입니다.");
        }
        if (age < 0) {
            throw new IllegalArgumentException("나이는 음수일 수 없습니다.");
        }

        String key = name + age;
        return personCache.computeIfAbsent(key, k -> new Person(name, age));
    }
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

- `public final class Person`: 불변 객체
- `private Person(String name, int age)`: 외부에서 해당 객체를 직접 생성하는 행위를 막아줌
- `public static Person createPerson(String name, int age)`: 정적 팩토리 메서드를 사용해 해당 이름과 나이를 가진 사람이 있는지 살펴보고(캐시) 없으면 해당 사용자를 캐싱에 넣어두고 해당 Person을 생성해서 돌려준다.

#### 가변 객체를 참조하고 있는 필드에 final

- personCache이 final 키워드로 선언이 되어있지만 HashMap은 변경이 가능한 가변 객체이기 때문에 사실상 personCache는 상수로 간주되지 않는다.
- 그럼 가변 객체를 참조한 필드에 final 키워드를 붙이는 이유가 뭘까 싶지만 이는 `참조를 변경할 수 없도록`하는 것이 final 키워드를 사용한 이유이다.
- 따라서 해당 personCache가 참조하는 HashMap의 내용을 변경은 가능하지만 참조는 불가하게 하기 위해 final 키워드를 사용한 것입니다.

#### 불변 객체를 참조하고 있는 필드나 기본 타입의 필드에 final

- BigDecimal과 같이 불변 객체를 내 필드에서 참조하고 있을 때 final 키워드를 사용해 상수로 만들었다면 이는 재할당과 수정 작업이 불가하다.
- 기본 타입 역시 위와 동일하다.

---

## [불변으로 객체를 만들 수 없다면?]

### 1. 변경할 수 있는 부분을 최소한으로 줄이자

- 객체가 가질 수 있는 상태의 수를 최소한으로 줄이면 예측하기 쉬워지고 오류가 생길 가능성이 줄어든다.
- 꼭 변경해야 할 필드를 뺀 나머지를 모두 private final로 선언하자.
- 생성자는 불변식 설정이 모두 완료된, 초기화가 완벽하게 끝난 상태의 객체를 생성해야 한다.
- 생성자, 정적 팩터리 외에는 어떤 초기화 메서드를 public으로 제공해서는 안 된다.

---

## [정리]

- 불변 객체를 사용할 수 있으면 사용하자 단점이라곤 잠재적 성능 저하가 전부이다.
- 불변 객체를 사용할 수 없다면 모든 필드를 private final 로 만들자
- 가변 객체를 참조하고 있는 필드의 경우엔 private final로 참조를 변경할 수 없게 하자
- 불변 객체를 참조하고 있는 필드나 기본 타입 필드의 경우라도 public final대신 private로 만들어 사용하자

### 1. 클래스에서의 final

```java
public final class Immutable {


}
```

- 상속을 막을 때 클래스에 final을 붙인다.
- final class의 경우엔 상속이 불가해 내부의 모든 method 역시 overrdding(재정의)이 불가하다.

### 2. 메서드에서의 final

- 오버라이딩을 막고 싶은 경우엔 메서드에 final을 붙인다.
- final method의 경우엔 해당 키워드가 붙은 메서드는 오버라이딩 할수 없게 한다.
- 예시로 부모 클래스에서 정의한 methodA()를 final 키워드를 붙인 경우라면 부모 클래스를 상속 받은 모든 클래스에서는 methodA()를 재정의 하지 않고 그대로 사용해야 한다.

**[final method - parent]**

```java
public class Parent {

    public final void immutableMethod(){
        System.out.println(" 부모 메소드 변경 금지 !! ");
    }
}
```

**[final method - child]**

```java
public class ChildA extends Parent {

}
```

**[overriding 불가]**
![](https://velog.velcdn.com/images/yeomyaloo/post/3942f3ec-f7b6-4303-8f05-6da4d54355fb/image.png)

### 3. 필드에서의 final

```java
public class Immutable {

    private final int age;

}
```

- 한번 초기화 한 필드를 더이상 변경하고 싶지 않다면 필드에 final을 붙인다.
- 불변성을 강제하거나 상수화를 위해서 필드에 final 키워드를 붙인다.

#### 불변 객체를 참조한 필드에서 private final

- 가변성 제한
- 초기화 이후라면 상태 변경이 불가하다.
- 한번 할당한 필드의 값은 변경이 불가하다.

#### 가변 객체를 참조한 필드

- 참조 변경 제한
- 가변 객체라도 private final로 선언했다면 초기화 후의 참조 변경을 막는다.
- 한 번 가변 객체를 초기화 한 후라면 새로운 객체로 참조 변경을 할수 없다.

#### 참조 변경을 막는 이유가 뭘까?

> - 불변성 유지: 참조 변경을 막는 것은 불변성(immutability)을 강제하는 일환입니다. 불변 객체는 한 번 생성된 후에 내부 상태가 변경되지 않습니다. 이는 객체를 예측 가능하게 만들고, 다중 스레드 환경에서도 안전하게 사용할 수 있도록 도와줍니다.

- 부작용 방지: 객체의 상태를 변경하면서 발생하는 부작용(side effect)을 방지하기 위해 참조 변경을 막습니다. 부작용이 발생하면 코드의 동작을 예측하기 어려워집니다. 특히 큰 시스템에서는 부작용이 코드의 복잡성을 증가시키고 유지보수를 어렵게 만들 수 있습니다.
- 다중 스레드 환경에서 안전성: 여러 스레드가 동시에 객체의 상태를 변경하면 예상치 못한 결과가 발생할 수 있습니다. 참조 변경을 막음으로써 객체가 공유될 때 다중 스레드 환경에서 더 안전하게 사용할 수 있습니다.
- 프로그램의 예측 가능성 향상: 참조 변경이 제한되면 프로그램의 동작이 더 예측 가능해집니다. 객체의 상태가 변하지 않는다면 해당 객체를 사용하는 코드에서 예상치 못한 결과가 발생할 가능성이 줄어듭니다.
- 디버깅 용이성: 참조 변경을 최소화하면 코드를 디버깅하기가 더 쉬워집니다. 상태가 변하지 않으면 특정 시점에서 객체의 상태를 파악하기가 더 용이하며, 버그를 찾고 수정하기가 더 간단해집니다.
- 설계의 단순화: 불변 객체와 참조 변경 제한은 프로그램의 설계를 단순화하고 모듈화하는 데 도움이 됩니다. 상태가 변하지 않는 객체는 일종의 "값"처럼 동작하여 프로그램의 전반적인 복잡성을 낮출 수 있습니다.<br>
  요약하면, 참조 변경을 막는 것은 예측 가능성, 안전성, 디버깅 용이성, 코드의 간결성 등을 향상시키기 위한 목적이 있습니다. 특히 불변 객체를 사용하면 객체의 상태를 변경하지 않으므로써 이러한 이점들을 얻을 수 있습니다.
