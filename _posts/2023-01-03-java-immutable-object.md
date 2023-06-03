---
layout: post
title: Immutable Object 
date: 2023-01-03 19:20:23 +0900
category: java
---
원본 링크 : https://www.baeldung.com/java-immutable-object

1. Overview
    * 이번 튜토리얼에서는 우리는 객체를 불변하게 만든다는게 어떤 것인지, Java에서는 어떻게 불변 객체를 만드는지 그리고 불변객체를 만듬으로서 얻는 이득에 대해 배울 것입니다.

2. 불변 객체란 무엇인가?
    * 불변 객체란 객체가 생성된 후에 그 내부 상태 값이 유지되는 객체를 말합니다.
    * 이는 불변 객체를 사용하는 공인 api는 객체가 살아있는 동안에는 동일한 행위를 보장 받는다는 것을 의미합니다.
    * String class를 살펴보면 replace method를 사용하게 되면 마치 우리에게 내부 값을 변경하는 듯한 행위를 하는 것처럼 보여지지만, 실제 원본 String은 변경되지 않는 것을 확인할 수 있습니다.
        ```java
        String name = "baeldung";
        String newName = name.replace("dung", "----");

        assertEquals("baeldung", name); // 처음 생성한 원본은 변경되지 않음.
        assertEquals("bael----", newName);
        ```

    * 즉 API는 우리에게 read-only method들만 제공할 뿐, 객체의 내부 상태 값은 절대로 변경할 수 없어야 합니다.

3. Java final 키워드
    * Java에서의 불변성을 확인해보기 전에, 우선 final 키워드에 대해 확인해볼 것입니다.
    * Java에서 변수는 기본적으로 변경이 가능합니다. 그리고 이는 우리가 변수의 값을 변경시킬수 있다는 것을 의미합니다.
    * 변수를 선언할 때 final 키워드를 사용하면, Java 컴파일러는 우리에게 그 변수의 값을 변경할 수 없게 하며 대신 compile-time에 에러를 발생시킵니다.
    ```java
    final String name = "baeldung";
    name = "bael..."; // 에러 발생.
    ```
    * 다만 final 키워드는 변수의 참조 자체의 변경을 방지할 뿐, 변수 자체의 공인 api를 사용하여 참조되고 있는 객체의 내부 상태 값을 변경하는 것을 방지해주지는 않는다는 것을 기억해야 합니다.
    ```java
    final List<String> strings = new ArrayList<>();
    assertEquals(0, strings.size());
    strings.add("baeldung");
    assertEquals(0, strings.size());
    ```
    * list에 요소(element)가 추가되면 그 크기가 변경되기 때문에 두번째 assertEquals는 실패하게 됩니다. 그러므로 이 객체는 불변 객체가 아닙니다.

4. Java의 불변성
    * 이제 우리는 변수의 내용 변경을 피하는 방법을 알게되었으니, 이를 활용해서 불변 객체의 api를 구성할 수 있습니다.
    * 불변 객체의 api를 구성한다는 것은 우리가 어떻게 그 api를 사용해도 내부의 값을 변경할 수 없도록 보장되어야 합니다.
    * 이를 위한 올바른 방식은 멤버변수(속성)를 선언할 때, final 키워드를 사용하는 것입니다.

    ```java
    class Money {
    	private final double amount;
    	private final Currency currency;
    	// ...
    }
    ```
    * Java는 우리에게 모든 primitive 타입의 변수에게 그렇듯 amount 변수의 값을 우리가 변경하지 못하도록 합니다.
    * 하지만 예제에서의 currency에 대한 변경은 보장하지 못하므로, 변경으로 부터 보호하기 위해서는 Currency api에 의존해야 합니다.

    * 대개의 경우, 우리는 객체가 사용자 정의 값을 갖기 위해서는 멤버변수(속성)가 필요합니다. 그리고 불변 객체의 내부 상태 값을 초기화하는 위치는 생성자입니다.

    ```java
    class Money {
        // ...
        public Money(double amount, Currency currency) {
            this.amount = amount;
            this.currency = currency;
        }

        public Currency getCurrency() {
            return currency;
        }

        public double getAmount() {
            return amount;
        }
    }
    ```
    * 이전에 말했듯, 불변 api 의 요구사항을 충족시키기 위해서는 Money class는 read-only method만을 가져야 한다.
    * reflection api를 사용하면 우리는 불변성을 깨고 불변 객체를 수정할 수 있다.
    * 하지만 reflection 은 불변 객체의 공인 api에 위배되므로 일반적인 경우라면 이를 피해야 한다.

5. 이득
    * 불변 객체는 그 내부 상태 값을 유지하므로, 멀티 스레드 환경에서 안전하게 공유할 수 있다.
    * 이 객체를 자유롭게 사용할 수 있고 참조하는 객체에 대해서도 전혀 차이가 없다고 할 수 있기 때문에 불변 객체는 영향도가 없다라고 할 수 있다.

6. 결론
* 불변 객체는 그 내부 값을 변경할 수 없으며, thread-safe를 보장하며 영향도에서 자유롭다.
* 이러한 속성들 덕분에 불변 객체는 특히 멀티 스레드 환경에서 유용하다.
* 이 글에서 사용된 예제는 [github](https://github.com/eugenp/tutorials/tree/master/core-java-modules/core-java-lang-oop-patterns)에서 확인이 가능하다.

