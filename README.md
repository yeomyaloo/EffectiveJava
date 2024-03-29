# 이펙티브 자바 
![image](https://github.com/yeomyaloo/EffectiveJava/assets/81970382/4ebee998-e487-4e28-be55-0782315c288f)

## 4장 클래스와 인터페이스
- [아이템 15. 클래스와 멤버의 접근 권한을 최소화하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/item15.md)
  - 접근 제한자를 클래스,인터페이스에 사용할 때와 필드에 사용할 때 어떻게 캡슐화를 할수 있는지에 대해서 알아보자.
- [아이템 16. Public 클래스에서는 Public 필드가 아닌 접근자 메서드를 사용하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/item16.md)
  - public 필드 대신 getter(= 접근자 메서드)를 사용해 직접 필드에 접근하지 못하게 막아 내부 데이터 변경을 못하게 하는 방법을 알아보자. 
- [아이템 17. 변경 가능성을 최소화하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/item17.md)
  - final 키워드를 사용해서 클래스, 메서드, 필드를 불변으로 만드는 방법을 알아보자
  - 불변 객체를 만들어 사용하는 방법을 알아보고 이에 이점과 단점을 알아보자
  - 불변 객체를 만들어 사용할 수 없는 상황이라면 최대한 필드를 private final로 선언해서 데이터 변경의 가능성을 최소화 시키는 방법을 알아보자
- [아이템 18. 상속보다는 컴포지션을 사용하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/item18.md)
  - 상속의 가장 큰 문제인 캡슐화를 깬다는 문제를 예시와 코드를 통해 알아보자
  - 컴포지션과 전달 클래스를 만드는 방법을 알아보고 상속 대신 래퍼클래스를 사용하도록 하자
- [아이템 19. 상속을 고려해 설계하고 문서화하라. 그러지 않았다면 상속을 금지하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/item19.md)
  - 상속이 필요한 경우가 아니라면 final을 사용해 클래스를 상속받지 못하게 막아두고 상속이 꼭 필요하다면 설계할 때 상속을 꼭 고려하고 또 문서화를 꼭 해야 한다!
  - 상속을 고려한 설계법과 문서화 방법을 설명하고 있다. 
- [아이템 20. 추상 클래스보다는 인터페이스를 우선하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/Item_20_%EC%B6%94%EC%83%81%20%ED%81%B4%EB%9E%98%EC%8A%A4%EB%B3%B4%EB%8B%A4%EB%8A%94%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%A5%BC%20%EC%9A%B0%EC%84%A0%ED%95%98%EB%9D%BC.md)
- 상속과 인터페이스 중 고려해야 하는 상황이라면 인터페이스를 확장해서 사용하자, 그 이유는 자바는 다중 상속을 막고 있기 때문에 이 제약에서 자유로운 인터페이스 확장을 이용해 조금더 자유롭게 사용해보자.
- 또한 골격 구현 클래스와 인터페이스를 같이 사용해서 인터페이스의 장점과 추상 클래스의 장점을 모두 사용할 수 있게 하자 
- [아이템 21. 인터페이스는 구현하는 쪽을 생각해 설계하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/item21.md)
  - 인터페이스에 디폴트 메서드를 사용할 수 있다고 해도 설게 시 고려하지 않은 디폴트 메서드의 무분별한 추가는 지양해야 한다.
  - 인터페이스 설계 시 해당 부분을 충분히 고려하여야 한다. 
- [아이템 22. 인터페이스는 타입을 정의하는 용도로만 사용하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/item22%20.md)
  - 클래스명 규칙을 지키지 않기 위해서 인터페이스를 상수용 인터페이스로 정의하지 않아야 한다.
  - 이는 해당 인터페이스가 필요하지 않게 됐을 시점의 릴리스에도 바이너리 코드로 계속 제공되어야 하기 때문이다.
  - 대신해서 상수를 필요로 하는 클래스에 종속시켜 사용할 수 있게 하자 
- [아이템 23. 태크 달린 클래스 대신 클래스 계층구조를 이용하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/23.md)
  - 태그 달린 클래스의 경우엔 무수한 단점이 있다. 이를 사용하기보단 클래스 계층구조를 사용해서 더 유연한 코드를 설계하자
- [아이템 24. 멤버 클래스는 되도록 static으로 만들라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/24.md)
  - 중첩 클래스의 종류 4가지를 확인하고 이때 특별하게 static 중첩 클래스를 사용하지 않아야 하는 이유가 아니라면 이를 사용하자
- [아이템 25. 톱레벨 클래스는 한 파일에 하나만 담으라](https://github.com/yeomyaloo/EffectiveJava/blob/main/4%EC%9E%A5%20%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4/25.md)
  - 톱레벨 클래스를 굳이 한 소스 파일 안에서 사용해야한다면 정적 멤버 클래스로 사용하자
  - 그게 아니라면 톱레벨 클래스별로 소스파일을 분리해서 작업하자
 
## 5장 제네릭
- [아이템 26. 로 타입은 사용하지 말라](https://github.com/yeomyaloo/EffectiveJava/blob/main/5%EC%9E%A5%20%EC%A0%9C%EB%84%A4%EB%A6%AD/26.md)
  - 제네릭의 도입과 적응까지 로타입을 두어서 이를 지원했다.
  - 그러나 여러 문제를 일으키는 로타입을 우리는 굳이 사용할 필요가 없다.
  - 그러니까 로타입 대신 제네릭을 사용하자 이때, 로타입을 꼭 사용해야 하는 클래스 리터럴 혹은 instanceof에서만 로타입을 사용하자
- [아이템 27. 비검사 경고를 제거하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/5%EC%9E%A5%20%EC%A0%9C%EB%84%A4%EB%A6%AD/27.md)
  - 컴파일러가 내뱉는 비검사 경고를 무시하지말고 최대한 제거를하자
  - 제거할 수 없다면 @SuppressWarnings 애너테이션을 붙여 이 경고를 숨기고 이 애너테이션은 꼭 타입 안전성을 확보 했을때만 좁은 범위에 붙여 사용하자
- [아이템 28. 배열보다는 리스트를 사용하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/5%EC%9E%A5%20%EC%A0%9C%EB%84%A4%EB%A6%AD/28.md)
  - 배열이 가지는 치명적 단점은 런타임시에 일어난다. 그러니까 이를 애초에 해결할 리스트(제네릭)을 사용해서 이런 문제를 발생하지 않도록 하자
- [아이템 29. 이왕이면 제네릭 타입으로 만들라](https://github.com/yeomyaloo/EffectiveJava/blob/main/5%EC%9E%A5%20%EC%A0%9C%EB%84%A4%EB%A6%AD/29.md)
  - 클라이언트에서 직접 형변환해야 하는 배열보단 제네릭 타입이 더 안전하고 쓰기 편하다.
  - 그러니 가능하면 새로 타입을 선언한다면 형변환 없이도 사용 가능하게 하자. (배열 대신 리스트를 쓰자는 말이랑 일맥상통 ~) 
- [아이템 30. 이왕이면 제네릭 메서드로 만들라](https://github.com/yeomyaloo/EffectiveJava/blob/main/5%EC%9E%A5%20%EC%A0%9C%EB%84%A4%EB%A6%AD/30.md)
  - 메서드 역시 제네릭하게 사용하면 문제가 줄어든다. 
- [아이템 31. 한정적 와일드카드를 사용해 API 유연성을 높이라](https://github.com/yeomyaloo/EffectiveJava/blob/main/5%EC%9E%A5%20%EC%A0%9C%EB%84%A4%EB%A6%AD/31.md)
  - 타입 매개변수의 불공변 성질은 유연한 설계를 방해할 때가 있다. 이를 해결하고 싶다면 한정적 와일드카드를 사용해서 불공보다 더 나은 유연한 방식을 도입하자.
  - 또한 타입 매개변수를 사용한 메서드를 사용할 때 고려할 점을 더 살펴보자
- [아이템 32. 제네릭과 가변인수를 함께 쓸 때는 신중하라](https://github.com/yeomyaloo/EffectiveJava/blob/main/5%EC%9E%A5%20%EC%A0%9C%EB%84%A4%EB%A6%AD/32.md)
  - 가변인수가 무엇인지 가변인수가 배열을 어떻게 생성하는지 알아보고, 이 가변인수에 제네릭이 들어가면 어떤 문제가 생길지 살펴보자
  - 또한 왜 우리는 가변인수에 제네릭 사용을 하면 안 되는것처럼 말하면서도 계속 사용하는지에 대해서 살펴보자 
