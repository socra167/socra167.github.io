---
title: Java Method
author: socra167
date: 2025-03-31
category: coding-test
layout: post
---
{% raw %}

[DevDocs](https://devdocs.io/)
[JAVA 온라인 IDE](https://programiz.pro/ide/java)

---

### 라이브러리 import

스트림 관련 기능을 쓰려면 `java.util.stream.*`을 추가로 `import`해야 한다.

```java
import java.util.*;           // List, Set, Map, Queue 등 컬렉션 클래스 포함
import java.util.stream.*;    // Stream, Collectors, IntStream 등 스트림 관련 클래스 포함
```

---

### 문자열 / 숫자 변환

**Integer.parseInt(), Long.parseLong(), Double.parseDouble() : 문자열 → 숫자**

```java
int num = Integer.parseInt("123");
long longNum = Long.parseLong("1234567890");
double dbl = Double.parseDouble("3.14");
```

**String.valueOf(), Integer.toString(), Double.toString() : 숫자 → 문자열**

```java
String str = String.valueOf(123);
String str2 = Integer.toString(456);
String str3 = Double.toString(3.14);
```

---

### 배열 / 컬렉션 변환

**배열 → 리스트**

```java
String[] arr = {"a", "b", "c"};
List<String> list = Arrays.asList(arr); // 수정 불가능한 리스트
List<String> list2 = new ArrayList<>(Arrays.asList(arr)); // 수정 가능
```

**리스트 → 배열**

```java
String[] newArr = list.toArray(new String[0]);
```

**배열 → 스트림**

```java
Stream<String> stream = Arrays.stream(arr);
```

**스트림 → 리스트**

```java
List<String> newList = stream.collect(Collectors.toList());
```

---

### 정렬

| 정렬 방식                                    | 사용 대상       | 기본 정렬 기준                     | 시간 복잡도 (`O` 표기법)                    |
| ---------------------------------------- | ----------- | ---------------------------- | ----------------------------------- |
| **`Arrays.sort(arr)`**                   | 기본 타입 배열    | 오름차순 (`int[]`, `double[]` 등) | `O(n log n)` (Dual-Pivot QuickSort) |
| **`Arrays.sort(arr, Comparator)`**       | 객체 배열       | `compareTo()` 기준             | `O(n log n)` (TimSort)              |
| **`Collections.sort(list)`**             | `List<E>`   | `compareTo()` 기준             | `O(n log n)` (TimSort)              |
| **`Collections.sort(list, Comparator)`** | `List<E>`   | 없음                           | `O(n log n)` (TimSort)              |
| **`List.sort(comparator)`**              | `List<E>`   | 없음                           | `O(n log n)` (TimSort)              |
| **`Stream.sorted()`**                    | `Stream<T>` | `compareTo()` 기준             | `O(n log n)`                        |
| **`Stream.sorted(Comparator)`**          | `Stream<T>` | 없음                           | `O(n log n)`                        |

---

Java에서 객체를 정렬할 수 있는 두 가지 방법 `Comparable`과 `Comparator`.

- **`Comparable` 인터페이스**
	- **클래스에서 구현해서 기본적인 정렬 정보 제공할 때**
	- `Arrays.sort()` 또는 `Collections.sort()`에서 자동으로 사용된다.
	- 객체 스스로가 자기 자신을 어떻게 정렬할지 정의하는 방법이다. 객체 클래스 내에서 `compareTo()` 메서드를 구현하여 자기 기준으로 비교한다. 이 방법은 객체의 "자연스러운 순서"를 정의하는 데 사용된다. 예를 들어, 숫자는 자연스럽게 오름차순 정렬된다.
- **`Comparator` 인터페이스**
	- **외부에서 정렬 기준을 구현할 때**
	- 외부에서 **다양한 기준으로 비교**할 수 있도록 정의한다. 객체를 여러 기준으로 정렬하고 싶을 때 유용하다. 객체의 클래스와는 독립적으로 비교 기준을 별도로 제공할 수 있기 때문에 **다양한 기준으로 객체를 정렬할 수 있다.**

---

** `Comparator` 메서드**

- **`compare(T o1, T o2)`**: 두 객체를 비교. `o1`이 `o2`보다 작으면 음수, 같으면 0, 크면 양수를 반환
- **`reversed()`**: 기존 비교 기준의 역순으로 정렬
- **`thenComparing()`**: 첫 번째 비교가 동일하면, 두 번째 기준으로 정렬
- **`reverseOrder()`** : 순서를 반대로 바꾸는 Comparator 반환하는 static method
- **`Comparator<T> thenComparingInt(ToIntFunction<? super T> keyExtractor)`**
- **`Comparator<T> thenComparingLong(ToLongFunction<? super T> keyExtractor)`**
- **`Comparator<T> thenComparingDouble(ToDoubleFunction<? super T> keyExtractor)`**
- **`static <T> Comparator<T> comparingInt(ToIntFunction<? super T> keyExtractor)
`**
- **`static <T> Comparator<T> comparingLong(ToLongFunction<? super T> keyExtractor)`**
- **`static <T> Comparator<T> comparingDouble(ToDoubleFunction<? super T> keyExtractor)`**

---

**`Comparator.comparing()`이 있는데 `Comparator.comparingInt()`를 사용하는 이유**
- **`int`**, **`long`**, **`double`** 등 원시 타입을 다룰 때 성능 면에서 유리하다.
- 비교 시 자동 언박싱(unboxing)이 일어나지 않아 오버헤드가 적고 객체 생성이 불필요해 원시 타입 비교에 최적화되어 있다.

---

**`Comparator`를 활용한 다양한 정렬 방법**
- **기본 정렬 기준 (오름차순)**
`Collections.sort(people, Comparator.comparing(Person::getName));`

- **내림차순 정렬**
`Collections.sort(people, Comparator.comparing(Person::getName).reversed());`

- **여러 기준으로 정렬**  
    **이름 기준으로 오름차순 정렬 후, 나이 기준으로 내림차순 정렬**하고 싶은 경우:
`Collections.sort(people, Comparator.comparing(Person::getName)                                       .thenComparing(Comparator.comparingInt(Person::getAge).reversed()));`

---

**Comparator**

**람다식**을 사용해 `Comparator`를 정의할 때, **`Comparator` 인터페이스**의 `compare(T o1, T o2)` 메서드를 구현한다. (Java 8부터 람다식으로 대체 가능)

```java
Comparator<T> comparator = (T o1, T o2) -> {
    // 두 객체를 비교하는 로직
    return o1.compareTo(o2); // 예시: 오름차순 비교
};
```

**Comparator.comparing()**

**객체의 속성을 추출하는 함수**를 사용해 **주어진 객체의 속성**을 기준으로 비교한다.

```java
Comparator.comparing(Person::getAge);  // 나이 기준으로 비교
Comparator.comparing(Person::getName); // 이름 기준으로 비교

Comparator.comparing(Person::getAge).reversed(); Comparator.comparing(Person::getName).reversed();
```

```java
Comparator.comparing((Person p) -> p.getAge());   // 나이 기준 비교
Comparator.comparing((Person p) -> p.getName());  // 이름 기준 비교
```

**thenComparing() : 여러 기준으로 정렬할 때**

여러 번 메서드 체이닝해 사용 가능하다.

```java
people.sort(Comparator.comparing(Person::getName)  // 이름 기준 오름차순
                  .thenComparing(Person::getAge)); // 나이 기준 오름차순
```

```java
List<String> words = Arrays.asList("apple", null, "banana", "cherry");
words.sort(Comparator.nullsFirst(Comparator.naturalOrder()));  // null 값은 먼저 오도록 정렬
```

---

**Arrays.sort() : 배열 정렬**

> **기본 타입 배열** 정렬 시 **`Dual-Pivot Quicksort`** 알고리즘 사용 (시간 복잡도 `O(n log n)`).

```java
int[] nums = {3, 1, 4, 1, 5, 9};

// 기본 타입 배열 정렬
Arrays.sort(nums); // 오름차순 정렬

// 객체 배열 정렬
Arrays.sort(people, Comparator.comparing(Person::getName)); // Comparator를 사용한 정렬
// 또는 객체가 `Comparable`을 구현

```

**Collections.sort(), List.sort() : 리스트 정렬**

```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9);
Collections.sort(numbers); // 오름차순 정렬
numbers.sort(Comparator.reverseOrder()); // 내림차순 정렬

// Comparator
Collections.sort(people, Comparator.comparing(Person::getName)); // 이름 기준 정렬

people.sort(Comparator.comparing(Person::getAge).reversed()); // 나이 내림차순 정렬

```

- Java 8 이상에서 `List.sort()`를 사용 가능하다.
- `Collections.sort()`보다 `List.sort()`를 사용하는 것이 더 직관적이다.

**커스텀 정렬 (Comparator 사용)**

```java
List<String> words = Arrays.asList("apple", "banana", "cherry");

// 문자열 길이순 정렬
words.sort(Comparator.comparingInt(String::length));

// 문자열 역순 정렬
words.sort(Comparator.reverseOrder());
```

---

**Stream.sorted() : Stream 정렬**

```java
int[] arr = {5, 3, 8, 1, 2};

// 기본 타입 배열 오름차순 정렬
int[] sortedArr = Arrays.stream(arr)  // int 배열을 Stream으로 변환
                         .sorted()    // 오름차순 정렬
                         .toArray();  // 다시 배열로 변환

// 기본 타입 배열 내림차순 정렬 (오름차순이 아니면 boxed() 필요)
int[] sortedDesc = Arrays.stream(arr)
                          .boxed()   // int -> Integer (객체 변환)
                          .sorted(Comparator.reverseOrder()) // 내림차순 정렬
                          .mapToInt(Integer::intValue)  // 다시 int 배열로 변환
                          .toArray();
```

```java
List<Integer> numbers = Arrays.asList(5, 3, 8, 1, 2);

// List 오름차순 정렬
List<Integer> sortedList = numbers.stream()
                                  .sorted()  // 오름차순 정렬
                                  .collect(Collectors.toList());

// List 내림차순 정렬
List<Integer> sortedDescList = numbers.stream()
                                      .sorted(Comparator.reverseOrder())  // 내림차순 정렬
                                      .collect(Collectors.toList());

List<String> names = Arrays.asList("Charlie", "Alice", "Bob");

// List<String> 오름차순 정렬
List<String> sortedNames = names.stream()
                                .sorted()
                                .collect(Collectors.toList());

// List<String> 내림차순 정렬
List<String> sortedDescNames = names.stream()
                                    .sorted(Comparator.reverseOrder())
                                    .collect(Collectors.toList());
```

```java
// `age` 오름차순 → `name` 오름차순 정렬
List<Person> sortedMultiCriteria = people.stream()
                                         .sorted(Comparator.comparing(Person::getAge)
                                                           .thenComparing(Person::getName)) // 나이 → 이름 순으로 정렬
                                         .collect(Collectors.toList());

// `age` 내림차순 → `name` 오름차순 정렬
List<Person> sortedMultiDesc = people.stream()
                                     .sorted(Comparator.comparing(Person::getAge).reversed()
                                                       .thenComparing(Person::getName)) // 나이 내림차순 → 이름 오름차순
                                     .collect(Collectors.toList());

```

---

**Map 정렬**
- `Map`은 기본적으로 정렬이 보장되지 않아 `Stream`으로 **Key**/**Value**기준 정렬할 수 있다.
- **`Map<String, Integer>`을 Value 기준 오름차순 정렬**

```java
Map<String, Integer> map = new HashMap<>();
map.put("Charlie", 3);
map.put("Alice", 1);
map.put("Bob", 2);

LinkedHashMap<String, Integer> sortedByValue = map.entrySet().stream()
			  .sorted(Map.Entry.comparingByValue())
			  .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, 
				  (e1, e2) -> e1, LinkedHashMap::new));

List<Map.Entry<String, Integer>> sortedByValueList = map.entrySet().stream()
            .sorted(Map.Entry.comparingByValue())
            .collect(Collectors.toList());
```

- **`Map<String, Integer>`을 Value 기준 내림차순 정렬**

```java
LinkedHashMap<String, Integer> sortedDescByValue = map.entrySet().stream()
			.sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
			.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, 
				  (e1, e2) -> e1, LinkedHashMap::new));

List<Map.Entry<String, Integer>> sortedDescByValueList = map.entrySet()
            .stream()
            .sorted(Map.Entry.comparingByValue(Comparator.reverseOrder()))
            .collect(Collectors.toList());
```

`LinkedHashMap` : **`Map`을 계속 사용해야 할 경우** (순서 유지비용 때문에 성능은 좋지 않음)
`List<Map.Entry<>>` : **정렬된 데이터를 단순히 처리하고 결과만 필요할 때**

---

**2차원 배열 정렬 (Comparator)**
- **첫 번째 요소 기준 정렬**

```java
int[][] points = {{3, 4}, {1, 2}, {5, 6}};
Arrays.sort(points, Comparator.comparingInt(a -> a[0]));
```

- **두 번째 요소 기준 정렬**

```java
Arrays.sort(points, (a, b) -> Integer.compare(a[1], b[1]));
```

---

### Arrays

- **`Arrays.copyOf()`**
    - 배열을 복사하여 새로운 배열을 반환한다.
    - 크기를 지정하여 복사할 수도 있다.
    
    ```java
    int[] newArray = Arrays.copyOf(originalArray, newLength);
    ```
    
- **`Arrays.copyOfRange()`**
    - 배열의 일부분을 복사하여 새로운 배열을 생성한다.
    
    ```java
    int[] newArray = Arrays.copyOfRange(originalArray, fromIndex, toIndex);
    ```
    
- **`Arrays.fill()`**
    - 배열의 모든 요소를 동일한 값으로 채운다.
    
    ```java
    Arrays.fill(array, value);
    ```
    
- **`Arrays.setAll()`**
    - 배열의 각 요소를 지정된 함수를 이용하여 설정한다.
    
    ```java
    Arrays.setAll(array, i -> i * 2);  // 배열의 각 요소를 2배로 설정
    ```
    
- **`Arrays.asList()`**
    - 배열을 리스트로 변환한다.
    - 이 메서드로 생성된 리스트는 **고정 크기 리스트**이다.
    
    ```java
    List<String> list = Arrays.asList("A", "B", "C");
    ```

- **`Arrays.equals()`**
    - 두 배열의 값이 동일한지 비교한다.
    - 크기와 요소들이 동일하면 `true`를 반환한다.
    
    ```java
    boolean isEqual = Arrays.equals(array1, array2);
    ```
    
- **`Arrays.deepEquals()`**
    - 2차원 배열 또는 그 이상의 배열을 비교한다.
    - 배열의 요소들이 배열인 경우, 그 내부 요소까지 **`recursive`** 하게 비교한다.
    
    ```java
    boolean isDeepEqual = Arrays.deepEquals(array1, array2);
    ```

- **`Arrays.binarySearch()`**
    - 정렬된 배열에서 이진 검색을 통해 요소의 위치를 찾는다.
    
    ```java
    int index = Arrays.binarySearch(array, key);
    ```
    
- **`Arrays.stream()`**
    - 배열을 `Stream`으로 변환한다.
    
    ```java
    int[] array = {1, 2, 3, 4};
    IntStream stream = Arrays.stream(array);
    ```

- **`Arrays.sort()`**
    - 배열을 오름차순으로 정렬한다.
    
    ```java
    Arrays.sort(array);
    ```
    
- **`Arrays.sort()` (Comparator 사용)**
    - 객체 배열을 `Comparator`를 이용하여 정렬한다.
    
    ```java
    Arrays.sort(array, Comparator.reverseOrder());
    ```
    
- **`Arrays.parallelSort()`**
    - `Arrays.sort()`의 병렬 버전으로, 큰 배열에 대해 성능을 향상시킬 수 있다.
    
    ```java
    Arrays.parallelSort(array);
    ```

- **`Arrays.toString()`**
    - 배열을 문자열로 변환한다.
    
    ```java
    String result = Arrays.toString(array);
    ```
    
- **`Arrays.deepToString()`**
    - 2차원 이상의 배열을 문자열로 변환한다.
    
    ```java
    String result = Arrays.deepToString(array);
    ```
    
- **`Arrays.hashCode()`**
    - 배열의 해시 코드를 반환한다.
    
    ```java
    int hashCode = Arrays.hashCode(array);
    ```
    
- **`Arrays.toArray()`**
    - 스트림을 배열로 변환할 때 사용한다.
    
    ```java
    Integer[] array = stream.toArray(Integer[]::new);
    ```

---

### Stream

**Array → Stream**

```java
int[] arr = {1, 2, 3, 4, 5};
IntStream intStream = Arrays.stream(arr);
```

**IntStream, LongStream, DoubleStream : 기본형 스트림**
- 박싱(boxing) 과정이 없어 객체 스트림보다 성능이 좋다.
- 객체 스트림으로 변환하려면 `boxed()` 필요

```java
IntStream intStream = IntStream.of(1, 2, 3, 4, 5);
LongStream longStream = LongStream.of(10000000000L, 20000000000L, 30000000000L);
DoubleStream doubleStream = DoubleStream.of(1.1, 2.2, 3.3);
```

**IntStream.range(start, end), LongStream.range(start, end) : 범위 스트림**
- `start`부터 `end-1`까지 정수 스트림 생성

```java
int sum = IntStream.range(1, 10).sum(); // 45
```

**IntStream.rangeClosed(start, end), LongStream.rangeClosed(start, end) : 범위 스트림**
- `start`부터 `end`까지 정수 스트림 생성

```java
int sum = IntStream.rangeClosed(1, 10).sum(); // 55
```

**Stream -> Array**
- `mapToInt(i -> i)` → `Integer`를 `int`로 변환 (언박싱)

```java
int[] array = list.stream().mapToInt(i -> i).toArray();
```

**List → Stream**

```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> listStream = list.stream();
```

**Stream.of()**
- 여러 개의 값을 직접 전달해 스트림을 생성

```java
int[] array = Stream.of(1, 2, 3, 4, 5)
                    .mapToInt(i -> i)
                    .toArray();

System.out.println(Arrays.toString(array));  // [1, 2, 3, 4, 5]

```

**Stream -> List**

```java
List<Integer> list = IntStream.of(1, 2, 3, 4, 5)
                              .boxed() // IntStream -> Stream<Integer>
                              .collect(Collectors.toList());
```

**Stream.collect() : 스트림 수집**
- 스트림의 요소들을 원하는 형태로 변환하거나 결과를 수집한다.
- 일반적으로 `Collectors` 클래스를 사용해 수집기를 제공한다.

```java
// toList()
List<String> list = stream.collect(Collectors.toList());

// toSet()
Set<String> set = stream.collect(Collectors.toSet());

// toMap()
Map<String, Integer> map = stream.collect(Collectors.toMap(
    String::toUpperCase,  // 키로 대문자 변환
    String::length         // 값으로 문자열 길이
));
```

**Stream.toList(), Stream.toSet(), Stream.toMap() : 컬렉션으로 수집 메서드**
- 기존엔 `Stream.collect()`를 직접 사용해야 했지만, `Java16`에서 Stream 인터페이스에 메서드가 추가되었다.
- (25.04.01 기준, Programmers는 컴파일 JDK 14로 사용 불가능)

```java
List<Integer> list = Stream.of(1, 2, 3, 4, 5)
			.toList();  // Stream -> List 변환
                                   
Set<Integer> set = Stream.of(1, 2, 3, 4, 5, 5)
			 .toSet();  // Stream -> Set 변환
                                 
Map<Integer, String> map = Stream.of("apple", "banana", "cherry")
			.collect(Collectors.toMap(String::length,  s -> s));
```

**Stream.toArray()**
- `toArray()`는 매개변수를 받지 않으면 `Object[]`를 반환하기 때문에 `String[]`로 캐스팅해야 한다.

```java
// 스트림을 String 배열로 변환
String[] array = Stream.of("apple", "banana", "cherry").toArray(String[]::new); 
```

**Stream.filter()**

```java
List<Integer> filtered = List.of(1, 2, 3, 4, 5, 6)
    .stream()
    .filter(n -> n % 2 == 0) // 짝수만 필터링
    .collect(Collectors.toList()); // 결과: [2, 4, 6]
```

**Stream.joining()**
- 스트림의 모든 요소를 결합하여 하나의 문자열로 만든다.
- 구분자를 넣지 않으면 기본적으로 문자열을 그냥 이어붙인다.

```java
String result = stream.collect(Collectors.joining(", "));
```

**Stream.groupingBy()**
- **첫 번째 인자로 그룹화 기준**을 받고, 두 번째 인자로 **결과의 형태**를 정의해 요소들을 Map으로 그룹화할 수 있다.

```java
Map<Integer, List<String>> groupedByLength = stream.collect(
    Collectors.groupingBy(String::length)
);

// {5=[apple], 6=[banana, cherry], 4=[date]}
```

**Stream.partitioningBy()**
- `Predicate`를 기준으로 두 그룹으로 나눠 `Map<Boolean, List<T>>`로 반환한다.

```java
Map<Boolean, List<String>> partitioned = stream.collect(
    Collectors.partitioningBy(word -> word.length() > 5)
);

// {false=[apple, date], true=[banana, cherry]}
```

**Stream.max() : 최댓값**
- `Optional<T> max(Comparator<? super T> comparator);`
- 스트림의 요소들 중에서 **최대값**을 구한다.
- **Comparator**를 기준으로 요소들을 비교한다.

```java
Optional<Integer> max = numbers.stream()
            .max(Comparator.naturalOrder()); // 자연 순서 기준으로 비교 (오름차순)
```

**Stream.min() : 최솟값**
- `Optional<T> min(Comparator<? super T> comparator);
- 스트림의 요소들 중에서 **최소값**을 구한다.
- **Comparator**를 기준으로 요소들을 비교한다.

```java
Optional<Integer> min = numbers.stream()
            .min(Comparator.naturalOrder()); // 자연 순서 기준으로 비교 (오름차순)
```

**Stream.sum() : 합**
- `sum()` 메서드는 `IntStream`, `LongStream`, `DoubleStream`에서 사용 가능하며, 스트림의 모든 요소를 합산한다.
- 일반 `Stream<T>`에서는 사용할 수 없다.

```java
int sum = IntStream.of(1, 2, 3, 4, 5).sum();
System.out.println(sum); // 15
```

**Stream.count() : 개수**
- `count()` 메서드는 스트림의 요소 개수를 반환한다.
- `long` 타입으로 반환된다.

```java
long count = Stream.of("apple", "banana", "cherry").count();
System.out.println(count); // 3
```

**Stream.collect(Collectors.counting()) : 개수**
- 스트림의 요소 개수를 `Collectors`를 사용해 센다.
- 보통 **`groupingBy()`와 함께 사용**할 때 의미가 있다. (개수를 value로 넣을 때)

```java
long count = stream.collect(Collectors.counting());
// Arrays.asList("apple", "banana", "cherry"); 일 때 -> 3

Map<Integer, Long> lengthCount = Stream.of("apple", "banana", "cherry", "date")
    .collect(Collectors.groupingBy(String::length, Collectors.counting()));
// {5=2, 6=1, 4=1} (문자열 길이를 기준으로 개수를 셈)

// Collectors.counting() 없이 groupingBy(String::length)만 쓰면 
// {5=[apple], 6=[banana, cherry], 4=[date]} 형태로 들어간다
```

**Stream.average() : 평균**
- `average()` 메서드는 `IntStream`, `LongStream`, `DoubleStream`에서 사용 가능하며, 평균 값을 계산한다.
- 결과는 `OptionalDouble`로 반환된다.

```java
OptionalDouble average = IntStream.of(1, 2, 3, 4, 5).average();
System.out.println(average.getAsDouble()); // 3.0
```

**Stream.distinct()**
- `distinct()` 메서드는 중복된 요소를 제거한 새로운 스트림을 반환한다.

```java
List<Integer> distinctList = Stream.of(1, 2, 2, 3, 3, 3, 4, 5)
                                   .distinct()
                                   .collect(Collectors.toList());
System.out.println(distinctList); // [1, 2, 3, 4, 5]
```

**Stream.limit(n)**
- 스트림의 첫 `n`개의 요소만 포함하는 새로운 스트림을 반환한다.
- `n` = 앞에서부터 몇 개를 남길 지

```java
List<Integer> limitedList = Stream.of(1, 2, 3, 4, 5)
                                  .limit(3)
                                  .collect(Collectors.toList());
System.out.println(limitedList); // [1, 2, 3]
```

**Stream.skip(n)**
- 앞에서 `n`개의 요소를 건너뛴 후 나머지 요소로 새로운 스트림을 생성한다.
- `n` = 앞에서부터 몇 개를 건너뛸지

```java
List<Integer> skippedList = Stream.of(1, 2, 3, 4, 5)
                                  .skip(2)
                                  .collect(Collectors.toList());
System.out.println(skippedList); // [3, 4, 5]
```

**Stream.mapToInt()**
- `mapToInt(ToIntFunction<? super T> mapper)`
- 객체 스트림을 `IntStream`으로 변환한다.

```java
List<String> list = List.of("a", "bb", "ccc");
IntStream lengths = list.stream().mapToInt(String::length);
lengths.forEach(System.out::println); // 1, 2, 3
```

**Stream.mapToLong()**
- `mapToLong(ToLongFunction<? super T> mapper)`
- 객체 스트림을 `LongStream`으로 변환한다.

```java
List<Integer> numbers = List.of(1, 2, 3);
LongStream longStream = numbers.stream().mapToLong(i -> i * 10000000000L);
longStream.forEach(System.out::println); // 10000000000, 20000000000, 30000000000
```

**Stream.mapToDouble()**
- `mapToDouble(ToDoubleFunction<? super T> mapper)`
- 객체 스트림을 `DoubleStream`으로 변환한다.

```java
List<Integer> numbers = List.of(1, 2, 3);
DoubleStream doubleStream = numbers.stream().mapToDouble(i -> i * 1.5);
doubleStream.forEach(System.out::println); // 1.5, 3.0, 4.5
```

**Stream.boxed()**
- 기본형 스트림 (`IntStream`, `LongStream`, `DoubleStream`)을 래퍼 클래스(`Stream<Integer>`, `Stream<Long>`, `Stream<Double>`)로 변환한다.

```java
IntStream intStream = IntStream.of(1, 2, 3);
Stream<Integer> boxedStream = intStream.boxed();
boxedStream.forEach(System.out::println); // 1, 2, 3
```

---

### PriorityQueue (우선순위 큐)

**기본 오름차순 정렬**

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.add(3);
pq.add(1);
pq.add(4);
System.out.println(pq.poll()); // 1
```

**내림차순 정렬**

```java
PriorityQueue<Integer> maxPQ = new PriorityQueue<>(Comparator.reverseOrder());
maxPQ.add(3);
maxPQ.add(1);
maxPQ.add(4);
System.out.println(maxPQ.poll()); // 4
```

---

### HashMap (해시맵)

**put() : 데이터 추가**

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 3);
map.put("banana", 2);
```

**containsKey() : key 존재 여부 검사**

```java
if (map.containsKey("apple")) {
    System.out.println("사과가 있습니다.");
}
```

**getOrDefault() : key가 없을 경우 기본값으로 가져오기**

```java
int count = map.getOrDefault("banana", 0);
```

**entrySet()**

```java
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}
```

**keySet()**

```java
for (String key : map.keySet()) {
    System.out.println(key + " -> " + String.valueOf(map.get(key)));
}
```

**values()**

```java
List<Integer> counts = new ArrayList<>(map.values());
```

**List를 Set로 변환해 중복 제거**

```java
List<Integer> list = Arrays.asList(1, 2, 2, 3, 3, 4);
Set<Integer> set = new HashSet<>(list); // 중복 제거
List<Integer> uniqueList = new ArrayList<>(set); // 다시 리스트로 변환
```

---

### Stack (스택)

- **LIFO (Last In, First Out)** 구조 (마지막에 들어간 요소가 먼저 나옴)
- 주로 **재귀 호출, 괄호 검사, 백트래킹, DFS(깊이 우선 탐색)** 등에 사용됨

**Stack 초기화**

```java
Stack<Integer> stack = new Stack<>();
```

**push() : 삽입**

```java
stack.push(1);
stack.push(2);
stack.push(3);
```

**pop() : 삭제 (맨 위 요소 반환 후 제거)**

```java
int top = stack.pop(); // 3
```

**peek() : 최상단 요소 조회**

- `pop()` 처럼 요소를 제거하지는 않는다.

```java
int top = stack.peek(); // 2
```

**isEmpty() : 비어있는지 확인**

```java
boolean empty = stack.isEmpty(); // false
```

---

### Queue (큐)

- **FIFO (First In, First Out)** 구조 (먼저 들어간 요소가 먼저 나옴)
- 주로 **BFS(너비 우선 탐색), 캐시 구현, 작업 큐** 등에 사용됨

**Queue 초기화 (`LinkedList` 사용)**

```java
Queue<Integer> queue = new LinkedList<>();
```

**offer() : 삽입**

- `add()`는 실패 시 예외가 발생하지만, `offer()`는 `false` 반환

```java
queue.offer(1);
queue.offer(2);
queue.offer(3);
```

**poll() : 삭제 (맨 앞 요소 반환 후 제거)**

- `remove()`는 실패 시 예외가 발생하지만, `poll()`은 `null` 반환

```java
int front = queue.poll(); // 1
```

**peek() : 조회 (맨 앞 요소 확인, 제거 없음)**

```java
int front = queue.peek(); // 2
```

**isEmpty() : 비어있는지 확인**

```java
boolean empty = queue.isEmpty(); // false
```

---

### PriorityQueue (우선순위 큐, 최소 힙)

- **작은 값이 먼저 나옴 (기본 최소 힙 구조)**
- 주로 **최적화 문제, 다익스트라 알고리즘** 등에 사용됨
- **ArrayList**는 삽입 시 매번 정렬을 해야 하므로 **`O(n log n)`** 이지만
- **PriorityQueue**는 **이진 힙(binary heap)** 을 사용해 삽입 시 **`O(log n)`** 이다.

---

**유리한 경우**

1. 우선순위가 자주 변경되고, 가장 큰/작은 값을 빠르게 가져와야 하는 경우
2. 값의 순서가 중요하지만, 삽입 순서대로 처리할 필요는 없는 경우
3. 반복적으로 가장 큰 값이나 가장 작은 값을 필요로 하는 경우

---

**불리한 경우**

1. 전체 데이터를 한 번에 정렬해야 하는 경우
2. 입력 순서가 중요한 경우

---

**PriorityQueue 초기화**

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
```

**예시 : 다익스트라 알고리즘**

```java
PriorityQueue<Node> pq = new PriorityQueue<>(Comparator.comparingInt(node -> node.distance));
pq.add(new Node(0, 0));  // 시작 노드

while (!pq.isEmpty()) {
    Node current = pq.poll();  // 가장 우선순위가 낮은 (거리) 노드를 꺼냄
    // ...
}

```

**offer() : 삽입**

```java
pq.offer(3);
pq.offer(1);
pq.offer(2);
```

**poll() : 삭제 (우선순위가 가장 높은 요소 반환 후 제거)**

- 작은 값이 우선순위가 높다.

```java
int min = pq.poll(); // 1
```

**peek() : 조회 (우선순위가 가장 높은 요소 확인, 제거 없음)**

```java
int min = pq.peek(); // 2
```

---

### Deque (덱, 양쪽에서 삽입/삭제)

- **양쪽에서 삽입/삭제 가능 (스택 + 큐 기능 포함)**
- 주로 **슬라이딩 윈도우, 최적화 문제** 등에 사용됨
- `add()`, `remove()`가 사용가능하지만, `offer()`, `poll()`이 더 안정적임

**Deque 초기화 (`ArrayDeque` 사용)**

```java
Deque<Integer> deque = new ArrayDeque<>();
```

**offerFirst() : 앞쪽에 삽입**

- `add()`는 실패 시 예외가 발생하지만, `offer()`는 `false` 반환

```java
deque.offerFirst(1);
```

**offerLast() : 뒤쪽에 삽입**

```java
deque.offerLast(2);
```

**pollFirst() : 앞쪽에서 삭제**

- `remove()`는 실패 시 예외가 발생하지만, `poll()`은 `null` 반환

```java
int front = deque.pollFirst(); // 1
```

**pollLast() : 뒤쪽에서 삭제**

```java
int back = deque.pollLast(); // 2
```

**peekFirst() : 앞쪽 요소 조회**

```java
int front = deque.peekFirst(); 
```

**peekLast() : 뒤쪽 요소 조회**

```java
int back = deque.peekLast(); 
```

---

### Math 메서드

**Math.abs() : 절댓값 (Absolute)**

```java
int absValue = Math.abs(-10);  // 10
double absDouble = Math.abs(-3.14);  // 3.14
```

**Math.max(), Math.min() : 최댓값 / 최솟값**

```java
int maxVal = Math.max(10, 20);  // 20
int minVal = Math.min(10, 20);  // 10
```

**Math.pow() : 거듭제곱 (Power)**

```java
double powVal = Math.pow(2, 3);  // 2^3 = 8.0
```

**Math.sqrt() : 제곱근 (Square Root)**

```java
double sqrtVal = Math.sqrt(16);  // 4.0
```

**Math.ceil(), Math.floor(), Math.round() : 올림 / 내림 / 반올림**

```java
double ceilVal = Math.ceil(3.14);   // 4.0 (올림)
double floorVal = Math.floor(3.14); // 3.0 (내림)
long roundVal = Math.round(3.5);    // 4 (반올림, 정수 리턴)
double roundDbl = Math.round(3.1415 * 100.0) / 100.0; // 소수점 2자리 반올림 (3.14)
```

**BigInteger.gcd() : 최대 공약수 (GCD) & 최소 공배수 (LCM)**
- 두 수의 곱을 최대 공약수로 나누면 최소 공배수

```java
int gcdVal = BigInteger.valueOf(36).gcd(BigInteger.valueOf(24)).intValue();

static int lcm(int a, int b) {
    return (a * b) /  BigInteger.valueOf(a).gcd(BigInteger.valueOf(b)).intValue();
}
```

**Integer.signum() : 부호 반환 (signum)**

```java
int signVal = Integer.signum(-10); // -1
```

---

{% endraw %}
