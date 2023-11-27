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
