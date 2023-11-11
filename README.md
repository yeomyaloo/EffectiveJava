# 이펙티브 자바 
![image](https://github.com/yeomyaloo/EffectiveJava/assets/81970382/4ebee998-e487-4e28-be55-0782315c288f)

## 벨로그
[벨로그에 똑같이 정리해둔 내용들](https://velog.io/@yeomyaloo/series/Effective-Java)

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
  
