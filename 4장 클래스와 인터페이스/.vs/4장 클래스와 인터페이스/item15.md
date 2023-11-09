![](https://velog.velcdn.com/images/yeomyaloo/post/e734326e-5d56-40d1-8eb4-0e1fde2bd304/image.png)

---

# [클래스와 인터페이스]

- 추상화의 기본 단위인 클래스와 인터페이스
- 자바의 심장과도 같다
- 자바 언어에서 클래스와 인터페이스 설계에 사용한 강력 요소를 알아보자

## 아이템 15. 클래스와 멤버의 접근 권한을 최소화하라

> **[핵심 정리]**
> 프로그램 요소의 접근성은 가능한 최소한으로 하라. 꼭 필요한 것만 골라서 최소한의 public API를 설계하자. 그 외에는 클래스, 인터페이스, 멤버가 의도치 않게 API로 공개되는 일이 없도록 해야한다. public 클래스의 경우엔 상수용 public static final 필드 외에는 어떠한 public 필드도 가져서는 안 된다. public static final 필드가 참도하는 객체가 불변인지 확인하라.

## [잘 설계된 컴포넌트]

- 클래스 내부 데이터와 내부 구현 정보를 외부 컴포넌트로부터 얼마나 잘 숨겼느냐가 잘 설계된 컴포넌트의 특징이다.
- 이는 자바의 특징 중 캡슐화와 관련이 있다. 이는 `캡슐화` 또는 `정보 은닉`이라고 부른다.
- 잘 설계된 컴포넌트는 구현과 API를 깔끔하게 분리한다.

### 1. 정보 은닉의 장점

- 시스템 개발 속도를 높인다. (병렬로 개발할 수 있기 때문이다.)
- 시스템 관리 비용을 낮춘다.
- 성능 자체에는 영향이 없지만 성능 최적화에 도움을 준다.
- 소프트웨어 재사용성이 높아진다.
- 개별 컴포넌트의 동작을 검증할 수 있게 한다.

---

## [자바에서의 정보 은닉]

- 자바는 `접근 제한자`라는 접근 제어 매커니즘을 제공한다.
- `Package-private`의 경우엔 default값으로 굳이 접근 제한자를 작성하지 않는다면 package-private이다.

### 1. 정보 은닉의 기본 원칙

- 모든 클래스와 멤버의 접근성을 가능한 좁게 만들어야 한다.
- 다르게 말하면 소프트웨어가 동작하는 한 항상 가장 낮은 접근 수준을 부여해야 한다는 뜻이다.

---

## [Class와 Interface에서의 접근 제한자]

- 가장 톱레벨에 있는 클래스와 인터페이스에 접근 제한자는 `public`/`package-private`로 선언하여 사용할 수 있다.
- `public`을 붙여 사용하면 해당 클래스, 인터페이스는 open API가 된다.
- `package-private`을 붙여 사용하면 해당 클래스, 인터페이스는 해당 패키지 안에서만 사용이 가능하게 된다.

### 1. Package-Private 클래스의 기본 모습

```java
class PackagePrivateClass{

}
```

- default로 package-private 접근 제한자 키워드를 생략해도 된다.

### 2. Private 클래스의 기본 모습

```java
//탑 레벨 클래스
public class TopLevelPrivateClass {

	// 정보 은닉을 위한 private 클래스
    private class PrivateClass{

    }
}
```

- private 클래스의 경우엔 탑 레벨 클래스 내에서 중첩 클래스로 작성해줘야 한다.
- 이는 클래스의 가시성을 확보하기 위함이다.
  - 해당 클래스가 다른 클래스 또는 코드 블록에서 어떻게 접근될 수 있는지를 나타내는 것을 클래스의 가시성이라고 한다.

### 3. 패키지 외부에서 사용할 필요가 없다면 package-private로 선언하자

- 패키지 외부에서 사용할 필요가 없다면 `package-private`로 선언해서 내부 구현으로 만들자.
- 내부 구현의 경우엔 언제든 수정이 가능하다.
- 이는 클라이언트에 아무 문제 없이 다음 릴리스에서 수정, 교체, 제거가 가능하다는 말이다.

### 4. public으로 선언한 클래스와 인터페이스

- `public`으로 선언한 클래스나 인터페이스의 경우엔 공개 API가 되기 때문에 계속해서 하위 호환을 위해 영원히 관리해주어야 한다.

### 5, 클래스와 인터페이스에서의 접근 제한자 예시 코드

#### Package-private 클래스

```java
package test;

//Package-private class
class PackagePrivateClass {

    void packagePrivateMethod(){
        System.out.println("package-private Method");
    }

}
```

- 클래스, 인터페이스에 `package-private` 접근 제한자를 사용한 경우에 다른 패키지에서는 해당 클래스를 사용할 수 없다.
- 동일 패키지에서는 사용이 가능하다.

#### Private 클래스

```java
package test;

public class TopLevelPrivateClass {

    public void Test() {
        //package-private의 경우엔 동일 패키지 내에서 사용 가능
        PackagePrivateClass packagePrivateClass = new PackagePrivateClass();
        packagePrivateClass.packagePrivateMethod();

        //private의 경우엔 동일 탑 레벨 클래스, 인터페이스 내에서만 사용 가능
        PrivateClass privateClass = new PrivateClass();
        privateClass.privateMethod();

    }

    private class PrivateClass{

        private void privateMethod(){
            System.out.println("private Method");
        }
    }
}
```

- 클래스, 인터페이스에 `private` 접근 제한자를 사용한 경우에 동일 레벨 클래스, 인터페이스에서만 사용이 가능하다.
- 동일 패키지여도 다른 클래스에서는 해당 `private` 클래스에 접근할 수 없다.

---

## [멤버의 접근 제한자]

### 1. 멤버란?

- 필드, 메서드, 중첩 클래스, 중첩 인터페이스를 멤버라고 합니다.

### 2. 멤버에 사용 가능한 접근 제한자

- `private`: 멤버 선언 후 탑레벨 클래스에서만 접근
- `package-private`: 멤버가 소속된 패키지 안의 모든 클래스에서 접근할 수 있다. 이때 접근 제한자를 명시하지 않았다면 접근 제한자는 package-private이다. (단 인터페이스의 멤버의 경우엔 명시하지 않은 경우 public이 적용된다.)
- `protected`:`package-private`의 접근 범위를 포함하며 이 멤버를 선언한 클래스의 하위 클래스에서도 접근 가능하다.
- `public`: 모든 곳에서 접근할 수 있다.

#### 멤버에 사용한 접근 제한자 예시 코드

```java
public class PublicClass{

	private int privateField;
   	int packagePrivateField; //package-private field
    protected int protectedField;
    public int publicField;

}
```

### 3. 공개 API 설계 후 모든 멤버는 private로 만들자

- private가 아닌 멤버의 경우엔 공개 API가 되기 때문에 영원히 지원돼야 한다. 이런 번거로움이 싫다면 멤버는 private로 선언하자
- private, package-private 멤버의 경우엔 해당 클래스의 구현에 해당하여 보통은 공개 API에 영향을 주지 않는다.
- Serializable을 구현한 클래스의 경우엔 그 필드도 의도치 않게 공개 API가 될수도 있다.

### 4. 테스트 코드를 위해서 멤버의 접근 범위를 넓히지 말자

- 테스트만을 위해서 클래스, 인터페이스의 멤버를 공개 API로 만들어서는 안 된다.

### 5. public 클래스의 인스턴스 필드는 되도록 public이 아니어야 한다.

#### [필드에 담을 수 있는 값을 제한할 힘을 잃은 경우]

**필드가 가변 객체를 참조 - 코드 **

```java
package test;

import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.List;

public class MutableObject {

    List<String> mutableField = new ArrayList<>(List.of("hello"));


    @Test
    void mutableObjectTest(){
        MutableObject mutableObject = new MutableObject();
        mutableObject.mutableField.add("mutable Field World!");

        System.out.println("가변 객체의 변경 상태: "+mutableObject.mutableField);
    }
}
```

- ArrayList의 경우엔 대표적인 가변 객체이다.

**필드가 가변 객체를 참조 - 결과 **
![](https://velog.velcdn.com/images/yeomyaloo/post/739154cd-8740-4d07-a9c4-bb03b3e6d38f/image.png)

**final이 아닌 public으로 선언 - 코드**

```java
package test;

import org.junit.jupiter.api.Test;

public class MutableObject {

    public StringBuilder stringBuilder = new StringBuilder("hello");


    @Test
    void mutableObjectTest(){
        MutableObject mutableObject = new MutableObject();
        mutableObject.stringBuilder.append(", World!");

        System.out.println("가변 객체의 변경 상태: "+mutableObject.stringBuilder.toString());

    }
}

```

- StringBuilder는 대표적인 가변 객체이다.

**final이 아닌 public으로 선언 - 결과**
![](https://velog.velcdn.com/images/yeomyaloo/post/c6a2f167-a9b9-4351-855f-efd3e8e8688a/image.png)

#### 왜 public으로 선언하면 안될까?

- 필드와 관련된 모든 것은 불변식을 보장할 수 없게 된다는 것이다.
- 또한 필드가 수정될 때(락 획득과 같은 작업) 다른 작업을 할 수 없게 되므로 가변 필드를 갖고 있는 클래스는 일반적으로 스레드 안전하지 않다.

### 6. 추상 개념을 완성하는 데 꼭 필요한 구성요소로써의 상수는 public static final 필드로 공개해도 좋다.

- 해당 필드는 꼭 기본 타입, 불변 객체를 참조해야 한다.
- 가변 객체는 final이 아닌 필드에 적용되는 모든 불이익이 그대로 적용된다.
- 다른 객체를 참조하지 못하지만 참조된 객체 자체는 수정될 수 있어 끔찍한 결과 초래가 있을 수 있다.

---

## [정리]

### 1. 접근 제한자 사용으로 캡슐화를 해보자.

- 클래스(인터페이스)와 멤버(필드, 메서드, 중첩 클래스 등..)의 접근 제한자를 붙여 접근 권한을 최소화 하자
- 접근 제한자를 붙여 접근 권한을 최소화하는 것은 정보 은닉 즉, 캡슐화의 시작이다.

### 2. 클래스와 인터페이스의 접근 제한자

- 클래스와 인터페이스에 사용 가능한 접근 제한자는 탑 레벨 클래스(인터페이스)인지 아니면 중첩 클래스(인터페이스)인지에 따라 다르다.
- 탑 레벨 클래스(인터페이스)의 경우엔 package-private와 public만 사용 가능하다.
- 탑 레벨 클래스나 인터페이스에 public 접근 제한자를 사용하면 이는 Open API가 된다.
  - 이는 하위 호환을 위해서 계속해서 관리해줘야 한다.

#### package-private

```java
package test;

public class PublicTopLevelClass {

}
```

#### package-private

```java
package test;

class PackagePrivateTopLevelClass {

}
```

### 3. 멤버의 접근 제한자

- 멤버의 경우엔 필드, 메서드, 중첩 클래스, 중첩 인터페이스를 의미한다.
- 중첩 클래스(인터페이스)는 클래스(인터페이스)의 멤버로 private/package-private/protected/public 모두 사용 가능하다.

#### 필드에 접근 제한자 사용 예시

```java
public class PublicClass{

	private int privateField;
   	int packagePrivateField; //package-private field
    protected int protectedField;
    public int publicField;

}
```

#### 메서드에서 접근 제한자 사용 예시

```java
package test;

public class MutableObject {


    private void PrivateMethod(){

    }
    void PackagePrivateMethod(){


    }
    protected void ProtectedMethod(){

    }
    public void PublicMethod(){

    }

}
```

#### 중첩 클래스(인터페이스)에 접근 제한자 사용 예시

```java
package test;

public class TopLevelClass {


    private class PrivateClass{

    }
    class PackagePrivateClass{


    }
    protected class ProtectedClass{

    }
    public class PublicClass{

    }

}
```

### 4. public 클래스의 인스턴스 필드는 되로록 public이 아니게 하자

- 가변 객체를 필드로 가지고 있는 경우
- final이 아닌 인스턴스 필드를 public으로 선언 한 경우
- 위의 경우엔 그 필드와 관련된 모든것은 불변식을 보장할 수 없게 된다는 의미기 때문에 스레드 안전하지 않다는 단점으로 인스턴스 필드는 되도록 public아니게 해야 한다.

### 5. public static final 필드

- 상수용 public static final 필드 외에는 어떤 필드도 public이어선 안 된다.
- 그러나 public static final 필드도 참조하고 있는 객체가 불변이 아니라면 참조된 객체 자체가 수정되는 불상사가 벌어질 수 있다. 그렇기 때문에 반드시 불변 객체를 참조해야 한다.
