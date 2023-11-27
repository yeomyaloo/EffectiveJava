![](https://velog.velcdn.com/images/yeomyaloo/post/0bdfcc56-6e47-47dd-a5a2-71a16f1d5b55/image.png)
# 4�� Ŭ������ �������̽�
## [������ 21. �������̽��� �����ϴ� ���� ������ �����϶�]
- �������̽��� ����Ʈ �޼����� �߰����� ������ �����ϰ� ������� �ʾҴ�.
- �׷��� ������ �������̽��� �����ϴ� ���� �����ϸ� �����ϵ��� �ؾ��Ѵ�.

> **[�ٽ� ����]**<br>
�������̽��� ����Ʈ �޼��� ����� ��������� ���� �������̽��� ����Ʈ �޼��� �߰��� ū ������ ���� �� �ִ�. 
���ο� �������̽��� ��쿣 ������ ���� �ݵ�� �׽�Ʈ�� ���ľ��Ѵ�.
�������̽� ������ ���Ŀ� ������ �߰ߵǸ� �����ϴ� ���� ������ ��쵵 ������ ����, �ش� ���ɼ����� ����ϰ� �̸� �����ϸ� �ȵȴ�.

## [�������̽����� ����Ʈ �޼��� ��� ����]
### 1. ����Ʈ �޼��尡 ���� ������ �������̽��� ���ο� �޼��� �߰� �۾�
- ���� ����ü�� ���� ��� ����� �������̽��� �޼��� �߰� �۾��� �Ұ��� �ߴ�.
- �̴� �������̽��� �޼��带 �߰��ϰ� �Ǹ� �� �������̽��� �����ϰ� �ִ� ����ü���� ������ ���� �����̴�.
- �̴� ������ ������ ���� ������� ������ �������̽��� �����ϰ� �ִ� ����ü(Ŭ����) �ȿ� ���� �߰��Ϸ��� �޼��尡 �̹� �����Ǿ� ���� ���ɼ��� �ſ� ���� �����̴�.

### 2. ����Ʈ �޼��� ����� �������� �������̽�
- `java 8`������ �������̽������� ����Ʈ �޼��� ����� ����������.
- �̴� �̹� �����ص� �޼��带 ����ü�� ��� �����ؾ� �Ѵٴ� �������̽� ���� ������ ����Ű�� �����ε�, ����Ʈ �޼���� �ش� �޼��带 �ۼ��صθ� �ش� �������̽��� �����ϴ��� �ش� ����Ʈ �޼��带 �������� �ʾƵ� �ȴ�.

### 3. �������� ���� ����Ʈ �޼���?
- ����Ʈ �޼��带 �ۼ��ص� �������̽��� �����ϴ� ����ü(Ŭ����)���� �ش� ����Ʈ �޼��带 �������� �ʴ´ٸ� ����Ʈ ������ ���̰� �˴ϴ�.

### 4. ��κ��� ��Ȳ�� ����Ʈ �޼���� �� �۵��Ѵ�.
- �ڹ��� ���̺귯���� ����Ʈ �޼���� �ڵ� ǰ���� ���� �������̶� ��κ��� ��Ȳ���� �� �۵��Ѵ�.
- �׷��� ������ �� �ִ� ��� ��Ȳ���� �Һ����� ��ġ�� �ʴ� ����Ʈ �޼��带 �ۼ��ϴ� ���� ����� ���̴�.
- �׷��ϱ� �� �۵��ϴ��� ��� ��Ȳ�� �Ϻ��ϰ� �����Ѵ�� �����̴� �ڵ带 �ۼ��ϱ� �����ϱ� �̷� �κ��� ����ؼ� �������̽��� ���� �� �ش� �������̽��� �����ؼ� ����� �κ��� ����ؼ� �����ϵ��� ����.

## [���÷� ���� �������̽��� ����Ʈ �޼��� �߰�]
### 1. Collection �������̽��� removeIf()
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
- �ش� �޼���� �־��� �Լ�(predicate)�� true�� ��ȯ�ϴ� ��� ���Ҹ� �����Ѵ�.
- ������ŰƮ�� true�� ��ȯ�ϸ� �ݺ����� remove()�� ȣ���� �� ���Ҹ� �����Ѵ�.
- �̰ͺ��� �� ���������� �����ϱ�� ������� 

### 2. ����Ʈ �޼��带 �ۼ��ص� ����� ����
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
- `org.apache.commons.collections4.collection.SynchronizedCollection<E>` �������� removeIf()�� ������ ���� �ʾƼ� �޼��� ȣ���� ����ȭ������ ���ϰ� �־����� ����� �������ϰ� ������ Ȯ���� �� �ִ�.

## [����Ʈ �޼��� �߰��� ������ �� �ִ� ����]
### 1. ����Ʈ �޼����� �߰��� �������̽��� ������ ���� �׷��� ����ü�� ��Ÿ�� ���� ����
- ����Ʈ �޼��带 �����ؼ� ���� ��쿡 �ش� ����Ʈ �޼��忡 ���� ������ �۾��� �����ϴ��� �ش� �������̽��� �����ϰ� �ִ� ���� ����ü ��ü�� ��Ÿ�� ������ �߻��� �� �ִ�.
- ������������ �ش� ��Ȳ�� �������� ���� �ƴ����� ���ɼ��� �ƿ� ���� ���� �ƴϱ� ������ �̸� �����ؾ� �Ѵ�.

### 2. ����Ʈ �޼���� �������̽��κ��� �޼��带 �����ϰų� ���� �޼����� �ñ״�ó�� �����ϴ� �뵵�� �ƴϴ�.
- �������̽��κ��� �޼��带 ����, ���� �޼����� �ñ״�ó ���� �뵵�� �̸� ����ϰ� �ȴٸ� ���� Ŭ���̾�Ʈ�� �����߸��� �ɰ��̴�.

