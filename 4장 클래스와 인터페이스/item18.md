![](https://velog.velcdn.com/images/yeomyaloo/post/ae18a2de-a407-43df-b642-04fd197f1875/image.png)

# [4장 클래스와 인터페이스]

## 아이템 18. 상속보다는 컴포지션을 사용하라

- 여기서 말하는 상속의 개념은 클래스가 다른 클래스를 확장하는 구현 상속을 의미한다.
  - 클래스가 인터페이스를 구현하거나 인터페이스가 다른 인터페이스를 확장하는 개념은 이번 아이템에서 말하고 있는 상속에 해당하지 않는다.
- 컴포지션과 전달에 집중해서 살펴보자
- 래퍼 클래스가 하위 클래스보다 나은점이 무엇인지를 잘 살펴보자

> **[핵심 정리]**<br>
> 상속은 강력하지만 캡슐화를 해친다는 문제가 있다. 상속을 사용해야 할때는 상위 클래스와 하위 클래스의 관계가 순수하게 is - a일 때만 써야 한다. 그러나 is - a 관계라고 해도 안심하고 쓸수 만은 없는게 상위 클래스가 확장을 고려해서 설계되지 않았거나 하위 클래스의 패키지가 상위 클래스와 다를 땐 여전히 문제가 될 수 있다. <br>
> 상속의 취약점을 피하고 캡슐화를 지키기 위해선 우리는 컴포지션과 전달을 사용해야 한다. 특히 래퍼 클래스로 구현할 적당한 인터페이스가 있다면 더욱 상속 대신 컴포지션과 전달을 사용해야 한다.래퍼 클래스는 하위 클래스보다 견고하고 강력하기 때문이다.

## [상속]

### 1. 상속이란?

- 자바에서의 상속은 부모 클래스(상위 클래스)가 있을 때 자식 클래스(하위 클래스)가 이를 `extends`를 사용해 상속 받을 수 있는 개념이다.
- 상속을 받은 자식 클래스는 부모 클래스의 멤버를 모두 상속 받아 사용할 수 있게 된다.
  - 이때 부모 클래스의 모든 멤버에 접근이 가능하다고 하지만 `private` 접근 제한자로 설정해둔 멤버의 경우엔 하위 클래스도 접근이 불가합니다.
  - 다른 패키지의 부모 클래스와 자식 클래스가 있는 경우에도 접근 제한자가 허용하고 있는 접근 범위를 벗어나면 이 경우에도 하위 클래스가 부모 클래스의 멤버에 접근할 수 없습니다.

### 2. 클래스 상속

**[부모 클래스]**

```java
package test;

import org.junit.jupiter.api.Test;

public class Parent {

    private final String name;
    String phoneNumber;
    protected String email;
    public int age;


    public Parent(String name) {
        this.name = name;
    }

    void methodA(){
        System.out.println("parent method");
    }
}
```

**[자식 클래스]**

```java
package test;

public class Child extends Parent{


    public Child(String name) {
        super(name);
    }
}
```

- `extends`를 사용해 부모 클래스를 상속 받아봅시다.

**[접근 가능한 범위]**

```java
package test;

import org.junit.jupiter.api.Test;

public class Main {

    @Test
    void test(){

        Child child = new Child("ddd");

        child.methodA();

        child.age = 10;
        child.email = "dd";
        child.phoneNumber= "191919191";

    }
}

```

- 해당 경우엔 같은 패키지 내에서 상속이 이루어져 있어서 해당 위의 필드에 접근이 가능한 범위까지 멤버에 접근이 가능한 것을 확인 가능하다.

---

## [상속을 지양해야 하는 이유]

### 1. 재사용성이 좋은 상속 왜 지양해야 하나?

#### 엄청 좋아보이는데 상속?

- 위의 코드를 살펴보면 접근 제한자만 잘 선언하면 우리는 상속이란 개념을 통해서 계속해서 코드를 재사용할 수 있을 거 같은데 왜 지양해야 할까?
  - 실제로 필드의 접근제한자를 private 가 아닌 다른 키워드를 사용하는 것은 지양해야 합니다.
- 또한 코드 재사용성뿐만 아니라 상속에서 중요한 개념인 Override(재정의)를 사용하면 코드를 진짜로 계속 원형 그대로 사용이 가능해서 좋은 거 같은데 왜 지양해야 하지?

#### 그러나 상속은...

- 위의 상속의 특징을 살린 부분만 본다면 상속이 코드 재사용성에 최적의 선택인 것처럼 보인다. 그러나 **'상속은 캡슐화를 깨뜨린다'**는 큰 문제를 발생시킨다.
- 이는 상위 클래스가 어떻게 구현됐느냐에 따라서 하위 클래스의 동작에 이상이 생길 수 있음을 이야기한다.
- 또한 릴리즈(=배포)마다 상위 클래스의 내부 구현이 달라질 수 있으며 이를 상속 받아 사용하고 있는 하위 클래스는 한 줄의 코드 변경도 없이 문제가 생길 수 있게 된다.
- 또한 구체 클래스를 다른 패키지에서 상속 받아 사용하는 경우라면 매우 위험하다.
  - 구체 클래스란 추상 클래스와 인터페이스의 개념이 아닌 실제 객체를 생성할 수 있는 클래스를 의미합니다.
  - 쉽게 말해서 이미 설계가 다 된 클래스가 구체 클래스입니다. (인터페이스와 추상 클래스는 설계가 안 된 클래스라고 생각하면 됩니다.)

### 2. 재정의도 상속의 단점을 이겨내진 못한다.

#### 2-1. 예시 코드로 알아보는 상속의 단점

```java
public class InstrumentedHashSet<E> extends HashSet<E> {

    private int addCount = 0;

    public InstrumentedHashSet(int initCap, float loadFactor){
    	super(initCap, loadFactor);
    }

    @Override
    public boolean add(E e) {
    	addCount++;
        return super.add(e);
    }

    @Override
    public boolean addAll(Collection<? extends E> c) {
    	addCount += c.size();
        return super.addAll(c);
    }

    public int getAddCount() {
    	return addCount;
    }
 }

```

- 해당 데이터를 3개를 추가한 상태로 addAll을 호출한 경우 우리는 addCount가 3이 나오길 기대한다.
- 그러나 결과는 6이 나온다.
- 아래 HashSet의 addAll() 메서드의 구현 코드를 보며 이유를 알아보자.

##### [addAll()을 재정의한 코드를 호출한 경우]

```java
//  addAll()
    /**
     * {@inheritDoc}
     *
     * @implSpec
     * This implementation iterates over the specified collection, and adds
     * each object returned by the iterator to this collection, in turn.
     *
     * <p>Note that this implementation will throw an
     * {@code UnsupportedOperationException} unless {@code add} is
     * overridden (assuming the specified collection is non-empty).
     *
     * @throws UnsupportedOperationException {@inheritDoc}
     * @throws ClassCastException            {@inheritDoc}
     * @throws NullPointerException          {@inheritDoc}
     * @throws IllegalArgumentException      {@inheritDoc}
     * @throws IllegalStateException         {@inheritDoc}
     *
     * @see #add(Object)
     */
    public boolean addAll(Collection<? extends E> c) {
        boolean modified = false;
        for (E e : c)
            if (add(e))
                modified = true;
        return modified;
    }
```

- 여기서 addAll() 메서드는 HashSet 클래스에서 이미 구현되어 있습니다. 그런데 addAll() 메서드를 InstrumentedHashSet에서 오버라이딩할 때 add() 메서드를 호출하면서 addCount를 증가시키고 있습니다. 이로 인해 addCount 값이 중복으로 증가하게 됩니다.
- 이런 상황에서 기반 클래스의 메서드가 서브 클래스에서 오버라이딩되면서 추가적인 작업이 필요할 때, 조심해야 합니다. 특히, **상속 구조에서 메서드가 호출**되면 내부에서 다시 **기반 클래스의 메서드가 호출되어 예상치 못한 효과를 초래**할 수 있습니다.
- 즉, 다시 말해 재정의를 하더라도 상위 클래스에서 해당 메서드를 어떻게 구현했느냐에 따라서 내 구현 방식이 제대로 동작하지 않을 수 있다는 말이다.

##### [해결 방안?]

- 하위 클래스에서 메서드 재정의를 하지 않는다면 문제를 고칠 수도 있다. 그러나 이는 위의 코드에 예시에만 부합한 사례이기 때문에 메서드 재정의를 하지 않는다고 근본적인 상속의 문제점이 사라자지진 않는다.
- 아니면 메서드를 재정의할 때 상위 클래스가 해당 메서드를 어떻게 구현하고 있는지를 파악해서 이에 맞게 재정의를 하는 방법이 있다. 그러나 이 역시도 적절하진 않다. 왜냐하면 시간이 많이 들고 이를 파악해서 수정하는 일 자체가 어렵기 때문이다.
- 또한 하위 클래스에서 접근 불가한 private 필드를 사용하는 경우라면 이 방식은 구현 자체가 불가해지기 때문이다.

#### 2-2. 다음 릴리즈에서 상위 클래스에 새로운 메서드가 추가됐을 때 재정의

##### [예시 상황]

> 다음 릴리즈에서 상위 클래스에 새로운 메서드가 추가된다고 가정해보자, 이때 보안 때문에 컬렉션에 추가된 모든 원소가 특정 조건을 만족해야 하는 프로그램을 생각해보자

- 모든 메서드를 재정의해서 필요한 조건을 검사하게 하면 될것 같다. 그러나 이 방식이 통하는 것은 상위 클래스에 또 다른 원소 추가 메서드가 추가되기 전까지다.
- 이 말은 즉, 상위 클래스에 또 원소 추가 메서드가 추가된다면 다시 이 부분을 고려하고 전부 재정의 해야할 것이다.

#### 2-3. 재정의가 문제 해결에 도움이 되지 않는다면 새로운 메서드를 만들어 해결하는 방법은?

> 기존에 존재하던 Hashtable, Vector를 컬렉션 프레임워크에 포함시키자 이와 관련된 보안 구멍을 수정해야 하는 일이 발생했다.

- 메서드 재정의가 문제를 해결 방안이 되지 않는다면 새로운 메서드를 추가해서 이 문제를 해결해보고자 했다.
- 이 경우는 메서드 재정의보다 더 안전한 방법인 것 같지만 위험이 전혀 없는 것은 아니다.
- 하위 클래스에 추가한 메서드와 상위 클래스에 있는 메서드가 시그니처가 같고 반환 타입만 다르다면 컴파일이 되지 않는 문제가 발생한다.
  - 이는 리턴 값만 다른 경우엔 오버로딩(Overloading)이 되지 않기 때문이다.
  - 이때 리턴 타입 마저 같다면 이는 재정의에 해당한다. (메서드 명, 매개변수, 리턴 타입이 같아야 재정의라고 한다. 내부 구현 방식이 달라지는 것에 해당)
- 상위 클래스의 메서드를 상속 받아 사용할 때 발생한 문제를 해결하려 재정의를 진행했음에도 해결되지 않은 문제를 하위 클래스에서 새로운 메서드 작성으로 해결하고자 할 때의 문제점은 상위 클래스의 메서드가 요구하는 규약을 만족하지 못할 가능성이 생긴다는 것이다.

---

## [문제 해결 방안]

- 상속을 진행했을 때 하위 클래스에서의 메서드 재정의도 새로운 메서드의 작성도 상속의 단점을 상쇄시켜주지 못했다.
- 그러나 이런 문제를 상속이 아닌 다른 방식으로 해결할 수 있는데 이런 문제 해결 설계를 컴포지션(composition:구성)이라고 한다.

### 1. Composition

- 기존 클래스를 확장하는 대신(=상속), 새로운 클래스를 만들고 상속하고자 하는 클래스를 `private` 필드로 기존 클래스의 인스턴스를 참조하게 하자
- 기존 클래스가 새로운 클래스의 구성요소로 쓰인다는 뜻에서 이러한 설계를 컴포지션이라고 한다.

#### 1-1. 상속 대신 private 필드로 기존 인스턴스를 참조하게 한 래퍼 클래스

```java
import java.util.Collection;
import java.util.HashSet;
import java.util.Set;

public class InstrumentedSetWrapper<E> implements Set<E> {
    private Set<E> set;
    private int addCount = 0;

    public InstrumentedSetWrapper(Set<E> set) {
        this.set = set;
    }

    public InstrumentedSetWrapper(int initCap, float loadFactor) {
        this.set = new HashSet<>(initCap, loadFactor);
    }

    @Override
    public boolean add(E e) {
        addCount++;
        return set.add(e);
    }

    @Override
    public boolean addAll(Collection<? extends E> c) {
        addCount += c.size();
        return set.addAll(c);
    }

    @Override
    public int size() {
        return set.size();
    }

    // 나머지 Set 인터페이스의 메서드들도 구현해야 합니다.

    public int getAddCount() {
        return addCount;
    }
}
```

- <a herf="#### 2-1. 예시 코드로 알아보는 상속의 단점">예시 코드</a>에선 HashSet을 `extends`를 사용해 상속해 구현하고 있다.
- 그러나 위의 코드에선 HashSet을 상속하는 방식이 아닌 private 필드로 선언해 사용하고 있다.
- 실제 책에선 위의 코드 대신 상속 대신 컴포지션을 사용한 래퍼 클래스와 어떤 Set 구현체라도 계측하게 만들어 새로운 Set으로 만드는 전달 클래스를 예시로 들어 설명하고 있다.
  - 컴포지션과 전달의 조합은 넓은 의미로 위임이라고 부른다.
  - 포스팅에서 보여준 코드는 컴포지션의 예시만 들었다. 실제 책에서는 컴포지션을 사용한 래퍼 클래스와 Set 계측 기능(전달)을 모두 예시로 설명하고 있다.

#### 1-2. 책에서 설명하고 있는 테스트 코드 그림

![](https://velog.velcdn.com/images/yeomyaloo/post/ab4b3b26-7e25-4b96-8d08-dd0d9bdaa3be/image.png)

- 왼쪽의 기존 코드는 상속을 받아 구현한 코드
  - 실제 구체 클래스를 상속 받아 사용한 개념이다.
- 오른쪽 컴포지션과 전달을 사용하여 구현한 코드
  - 인터페이스를 확장하는 개념이라 상속이 아니다.

#### 1-3. 래퍼 클래스 만들기

- 상속 대신 private 필드로 상속 받고자 했던 기본 클래스를 선언하기
- 필드로 상속 받아 사용하고자 했던 클래스의 기능을 사용하기
  ![](https://velog.velcdn.com/images/yeomyaloo/post/659c749f-e1d8-49f1-9eb3-87536ad9b073/image.png)
- 해당 래퍼 클래스를 사용하게 되면 래퍼 클래스의 객체를 호출하게 된다.

---

## [래퍼 클래스의 단점]

### 1. 래퍼 클래스 단점

- 콜백 프레임워크와 어울리지 않는다.
- 콜백 프레임워크에서는 자기 자신의 참조를 다른 객체에 넘겨 다음 호출 때 사용하도록 한다. 내부 객체는 자신을 감싸고 있는 래퍼의 존재를 모르기 때문에 자신의 참조를 넘기고 콜백 때는 래퍼가 아닌 내부 객체를 호출하게 된다.
- 래퍼 클래스의 콜백 프레임워크 문제를 SELF 문제라고 한다.

---

## [상속을 꼭 사용해야 하는 경우]

### 1. 하위 클래스가 상위 클래스의 진짜 하위 타입인 경우에만 써라

- 하위 클래스가 상위 클래스의 진짜 하위 타입인 상황에서만 사용해야 한다.
  - is - a 관계일 때만 상위 클래스를 상속해야 한다.

### 2. 확장하려는 클래스의 API에 결함이 없는지 확인해라

- 결함이 없는 경우라면 확장해서 사용해라 그러나 결함이 있는 경우라면 결함까지 하위 클래스에게 상속하게 된다는 점을 주의하자.

### 2. 위의 경우가 아니면 상속을 쓰지말자

- 상위 클래스가 하위 클래스의 필수 구성요소가 아니라면 구현 상위 클래스를 private 인스턴스로 두고 사용해야 한다.

---

## [컴포지션 대신 상속을 사용하면 생기는 문제점 - 그러니까 컴포지션과 전달을 쓰자.]

### 1. 문제점

1. 컴포지션을 사용해야할 때 상속을 사용하면 내부 구현을 불필요하게 노출하는 꼴이다.
2. 클라이언트가 노출된 내부 구현에 직접 접근할 수 있게 된다. 이는 심각한 문제를 초래할 수 있게 된다.
3. 확장하려는 클래스가 결함이 있는 API인 경우라면 코드 재사용은 고사하고 그 결함까지 승계된다.

---

## [정리]

1. 상속의 개념과 상속의 장점, 단점
2. 상속의 단점을 해결하기 위한 메서드 재정의와 하위 클래스의 새 메서드 추가
3. 그럼에도 해결하지 못한 캡슐화를 깨버리는 문제를 해결하기 위한 컴포지션 등장
4. 컴포지션과 전달을 사용해 래퍼 클래스를 만들어 사용하는 것의 장점
5. 래퍼 클래스의 단점, 콜백 프레임워크와 사용 시 래퍼 클래스가 아닌 내부 객체의 참조를 전달하는 문제
6. 꼭 상속을 사용해야 한다면 어떤 경우에 사용해야 할지에 대한 설명
7. 결과적으로 컴포지션을 사용하길..
