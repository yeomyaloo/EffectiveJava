![](https://velog.velcdn.com/images/yeomyaloo/post/0bdfcc56-6e47-47dd-a5a2-71a16f1d5b55/image.png)
# 4장 클래스와 인터페이스
## [아이템 21. 인터페이스는 구현하는 쪽을 생각해 설계하라]
- 인터페이스에 디폴트 메서드의 추가에도 위험이 완전하게 사라지진 않았다.
- 그렇기 때문에 인터페이스는 구현하는 쪽을 생각하며 설계하도록 해야한다.

> **[핵심 정리]**<br>
인터페이스에 디폴트 메서드 사용이 가능해졌어도 기존 인터페이스에 디폴트 메서드 추가는 큰 위험이 따를 수 있다. 
새로운 인터페이스의 경우엔 릴리스 전에 반드시 테스트를 거쳐야한다.
인터페이스 릴리스 이후에 결함이 발견되면 수정하는 것이 가능한 경우도 있지만 절대, 해당 가능성에만 기대하고 이를 진행하면 안된다.

## [인터페이스에서 디폴트 메서드 사용 가능]
### 1. 디폴트 메서드가 없던 시절의 인터페이스에 새로운 메서드 추가 작업
- 기존 구현체를 깨는 방법 말고는 인터페이스에 메서드 추가 작업은 불가능 했다.
- 이는 인터페이스에 메서드를 추가하게 되면 이 인터페이스를 구현하고 있는 구현체에도 영향이 가기 때문이다.
- 이는 컴파일 오류가 나는 대상으로 기존의 인터페이스를 구현하고 있는 구현체(클래스) 안에 새로 추가하려는 메서드가 이미 구현되어 있을 가능성이 매우 낮기 때문이다.

### 2. 디폴트 메서드 사용이 가능해진 인터페이스
- `java 8`에서는 인터페이스에서도 디폴트 메서드 사용이 가능해졌다.
- 이는 이미 정의해둔 메서드를 구현체가 모두 구현해야 한다는 인터페이스 내의 단점을 상쇄시키는 내용인데, 디폴트 메서드로 해당 메서드를 작성해두면 해당 인터페이스를 구현하더라도 해당 디폴트 메서드를 구현하지 않아도 된다.

### 3. 구현하지 않은 디폴트 메서드?
- 디폴트 메서드를 작성해둔 인터페이스를 구현하는 구현체(클래스)에서 해당 디폴트 메서드를 구현하지 않는다면 디폴트 구현이 쓰이게 됩니다.

### 4. 대부분의 상황에 디폴트 메서드는 잘 작동한다.
- 자바의 라이브러리의 디폴트 메서드는 코드 품질이 높고 범용적이라 대부분의 상황에서 잘 작동한다.
- 그러나 생각할 수 있는 모든 상황에서 불변식을 해치지 않는 디폴트 메서드를 작성하는 일은 어려운 일이다.
- 그러니까 잘 작동하더라도 모든 상황에 완벽하게 설계한대로 움직이는 코드를 작성하긴 어려우니까 이런 부분을 고려해서 인터페이스의 설계 시 해당 인터페이스를 구현해서 사용할 부분을 고려해서 설계하도록 하자.

## [예시로 보는 인터페이스의 디폴트 메서드 추가]
### 1. Collection 인터페이스의 removeIf()
```java
/**
     * Removes all of the elements of this collection that satisfy the given
     * predicate.  Errors or runtime exceptions thrown during iteration or by
     * the predicate are relayed to the caller.
     *
     * @implSpec
     * The default implementation traverses all elements of the collection using
     * its {@link #iterator}.  Each matching element is removed using
     * {@link Iterator#remove()}.  If the collection's iterator does not
     * support removal then an {@code UnsupportedOperationException} will be
     * thrown on the first matching element.
     *
     * @param filter a predicate which returns {@code true} for elements to be
     *        removed
     * @return {@code true} if any elements were removed
     * @throws NullPointerException if the specified filter is null
     * @throws UnsupportedOperationException if elements cannot be removed
     *         from this collection.  Implementations may throw this exception if a
     *         matching element cannot be removed or if, in general, removal is not
     *         supported.
     * @since 1.8
     */
    default boolean removeIf(Predicate<? super E> filter) {
        Objects.requireNonNull(filter);
        boolean removed = false;
        final Iterator<E> each = iterator();
        while (each.hasNext()) {
            if (filter.test(each.next())) {
                each.remove();
                removed = true;
            }
        }
        return removed;
    }

```
- 해당 메서드는 주어진 함수(predicate)가 true를 반환하는 모든 원소를 제거한다.
- 프레드키트가 true를 반환하면 반복자의 remove()를 호출해 그 원소를 제거한다.
- 이것보다 더 범용적으로 구현하기는 어렵지만 

### 2. 디폴트 메서드를 작성해도 생기는 문제
~~~java
// https://mvnrepository.com/artifact/org.apache.commons/commons-collections4/4.4
public class SynchronizedCollection<E> implements Collection<E>, Serializable {

    /** Serialization version */
    private static final long serialVersionUID = 2412805092710877986L;

    /** The collection to decorate */
    private final Collection<E> collection;
    /** The object to lock on, needed for List/SortedSet views */
    protected final Object lock;
		
		...

		@Override
    public boolean remove(final Object object) {
        synchronized (lock) {
            return decorated().remove(object);
        }
    }

    /**
     * @since 4.4
     */
    @Override
    public boolean removeIf(final Predicate<? super E> filter) {
        synchronized (lock) {
            return decorated().removeIf(filter);
        }
    }

		...
}
~~~
- `org.apache.commons.collections4.collection.SynchronizedCollection<E>` 기존에는 removeIf()를 재정의 하지 않아서 메서드 호출을 동기화해주지 못하고 있었지만 현재는 재정의하고 있음을 확인할 수 있다.

## [디폴트 메서드 추가로 벌어질 수 있는 문제]
### 1. 디폴트 메서드의 추가와 인터페이스의 컴파일 성공 그러나 구현체의 런타임 오류 문제
- 디폴트 메서드를 구현해서 넣은 경우에 해당 디폴트 메서드에 대한 컴파일 작업은 성공하더라도 해당 인터페이스를 구현하고 있는 기존 구현체 자체에 런타임 오류가 발생할 수 있다.
- 무조건적으로 해당 상황이 벌어지는 것은 아니지만 가능성이 아예 없는 일이 아니기 때문에 이를 주의해야 한다.

### 2. 디폴트 메서드는 인터페이스로부터 메서드를 제거하거나 기존 메서드의 시그니처를 수정하는 용도가 아니다.
- 인터페이스로부터 메서드를 제거, 기존 메서드의 시그니처 수정 용도로 이를 사용하게 된다면 기존 클라이언트를 망가뜨리게 될것이다.

