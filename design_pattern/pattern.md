### **개념**

- **관찰자 패턴**은 객체 간의 **1 대 N 의존 관계**를 정의하여, 하나의 객체 상태가 변경되면 이를 의존하고 있는 다른 객체들에게 자동으로 알림이 전달되도록 하는 디자인 패턴입니다.
- 주로 **이벤트 기반 시스템**이나 **상태 변경 감지**가 필요한 곳에서 사용됩니다.

### **구성 요소**

1. **주체(Subject)**
    - 상태를 관리하고 변경 사항이 있을 때 관찰자에게 알리는 역할을 합니다.
    - 관찰자를 등록하거나 제거하는 메서드를 제공합니다.
2. **관찰자(Observer)**
    - 주체를 관찰하며 상태 변경이 발생하면 알림을 받아 작업을 수행합니다.

```java
import java.util.ArrayList;
import java.util.List;

// Subject (주체)
class Subject {
    private List<Observer> observers = new ArrayList<>();
    private String state;

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void setState(String state) {
        this.state = state;
        notifyObservers();
    }

    public String getState() {
        return state;
    }

    private void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(state);
        }
    }
}

// Observer (관찰자)
interface Observer {
    void update(String state);
}

// Concrete Observer
class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    @Override
    public void update(String state) {
        System.out.println(name + " received update: " + state);
    }
}

// 사용 예
public class ObserverPatternExample {
    public static void main(String[] args) {
        Subject subject = new Subject();

        Observer observer1 = new ConcreteObserver("Observer1");
        Observer observer2 = new ConcreteObserver("Observer2");

        subject.addObserver(observer1);
        subject.addObserver(observer2);

        subject.setState("New State 1");
        subject.setState("New State 2");
    }
}
```

### **. 이터레이터 패턴 (Iterator Pattern)**

### **개념**

- **이터레이터 패턴**은 컬렉션(예: 리스트, 배열) 내부 구조를 노출하지 않고, 해당 컬렉션 요소에 순차적으로 접근할 수 있는 방법을 제공합니다.
- 컬렉션의 순회 방식을 캡슐화하여 컬렉션을 변경하더라도 순회 로직에 영향을 주지 않도록 설계합니다.

### **구성 요소**

1. **Iterable(컬렉션)**
    - 요소를 저장하고 관리하는 역할을 합니다. 이터레이터를 반환하는 메서드를 제공합니다.
2. **Iterator(이터레이터)**
    - 요소를 순차적으로 접근할 수 있도록 인터페이스를 제공합니다.
    - 주요 메서드:
        - `hasNext()`: 다음 요소가 있는지 확인
        - `next()`: 다음 요소를 반환
        - `remove()`: 현재 요소를 삭제 (선택적)

### **Java에서의 예시**

Java에서는 `java.util.Iterator`와 `java.util.Enumeration` 인터페이스로 이터레이터 패턴을 구현합니다.

```java
java
코드 복사
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class IteratorPatternExample {
    public static void main(String[] args) {
        List<String> items = new ArrayList<>();
        items.add("Item1");
        items.add("Item2");
        items.add("Item3");

        // Iterator 사용
        Iterator<String> iterator = items.iterator();

        while (iterator.hasNext()) {
            String item = iterator.next();
            System.out.println(item);
        }
    }
}

```

### **이터레이터 패턴의 커스텀 구현**

```java
java
코드 복사
// Custom Iterable Collection
class CustomCollection {
    private String[] items;
    private int size = 0;

    public CustomCollection(int capacity) {
        items = new String[capacity];
    }

    public void add(String item) {
        if (size < items.length) {
            items[size++] = item;
        }
    }

    public CustomIterator iterator() {
        return new CustomIterator();
    }

    // Custom Iterator
    class CustomIterator {
        private int index = 0;

        public boolean hasNext() {
            return index < size;
        }

        public String next() {
            return items[index++];
        }
    }
}

// 사용 예
public class CustomIteratorExample {
    public static void main(String[] args) {
        CustomCollection collection = new CustomCollection(5);
        collection.add("A");
        collection.add("B");
        collection.add("C");

        CustomCollection.CustomIterator iterator = collection.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}

```

### **주요 사용 사례**

- Java 컬렉션 프레임워크 (`List`, `Set`, `Map` 등)
- 데이터 스트림 처리
- 트리 구조 순회 (예: DFS, BFS)