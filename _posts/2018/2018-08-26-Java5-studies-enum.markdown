---
layout: post
title: JAVA 1.5 studies #1 Enum
date: 2018-08-26 18:00
comment: true
---


* JAVA1.5(Tiger) 관련자료

  > 이 새로운 기능은 Java에 근본적 변화를 가져왔습니다. 이러한 기능을 언제 어떻게 사용하는지를 이해하면 더 나은 코드를 작성할 수 있을 것입니다.

- Enhancements in JDK 5 - the official list of the new features in JDK 5
- Generics Tutorial (PDF) - Gilad Bracha's generics tutorial
- dev2dev Developer Centers

# Enumerations

Enum은 수 년간 enum 값으로 사용되어온 public static final int 선언과 많이 유사합니다. int에서 가장 크고 확실하게 개선된 점은 type safe입니다. int와는 달리 enum 타입 중 한 가지를 다른 타입의 위치에 사용할 수 없습니다. 왜냐하면 컴파일러에게는 모든 것이 똑같아 보이기 때문입니다. 아주 드물게 예외가 있긴 하지만 이 경우에도 enum과 유사한 모든 int 구성을 enum 인스턴스로 교체해야 합니다.

> enum 은 Java 5에새 새로 생긴 키워드이므로, 옛날 자바 코드에서 enum을 형의 이름(변수/클래스/메소드 이름 등)으로 사용했다면, -source 1.5 를 사용하기 위해 그것들을 모두 수정해야 한다.

Enum은 여러 가지 추가 기능을 제공합니다. 유틸리티 클래스인 EnumMap 및 EnumSet은 특히 enum에 최적화된 표준 컬렉션 구현입니다. 컬렉션에 enum만 포함되는 것을 알고 있다면 HashMap 또는 HashSet 대신 이러한 특정 컬렉션을 사용해야 합니다.

대부분의 경우 코드에서 모든 public static final ints를 enums으로 교체할 수 있습니다. enum은 비슷(Comparable)하고, 내부 클래스(또는 내부 enum)일지라도 이에 대한 참조가 똑같아 보이므로 정적으로 가져올 수 있습니다. enum을 비교할 때 선언되는 순서가 서수 값을 나타낸다는 사실에 유의하십시오.

## "Hidden" 정적 메서드

작성한 모든 enum 선언에는 두 가지 정적 메서드가 표시됩니다. 이 두 가지 메서드는 Enum 자체가 아니라 enum 하위 클래스에 대한 정적 메서드이기 때문에 java.lang.Enum에 대한 javadoc에는 표시되지 않습니다.

첫 번째 values()는 enum에 가능한 모든 값의 배열을 리턴합니다.

두 번째 valueOf()는 제공된 문자열에 대한 enum을 리턴하는데, 원본 코드 선언과 똑같이 일치해야 합니다.

## 메서드
enums의 좋은 점 중의 하나는 메서드를 가질 수 있다는 것입니다. 과거에는 데이터베이스 타입을 JDBC URL로 번역하기 위해 public static final int에서 스위치를 수행한 코드가 필요했을 수 있습니다. 이제는 enum 자체에 직접 코드를 정리할 수 있는 메서드를 가질 수 있습니다. 다음은 DatabaseType enum에서 추상 메서드와 각 enum 인스턴스에 제공된 구현을 사용하여 이것을 어떻게 수행하는 지 보여주는 예제입니다.

public enum DatabaseType {
    ORACLE {
        public String getJdbcUrl() {...}
    },
    MYSQL {
        public String getJdbcUrl() {...}
    };
    public abstract String getJdbcUrl();
}
이제 enum에서 유틸리티 메서드를 직접 제공할 수 있습니다. 예를 들면 다음과 같습니다.

DatabaseType dbType = ...;
String jdbcURL = dbType.getJdbcUrl();
이전에는 URL을 얻기 위해 유틸리티 메서드가 있는 장소를 알려주어야 했습니다.


