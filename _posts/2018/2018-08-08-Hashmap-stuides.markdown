---
layout: "post"
title: "HashMap-Stuides"
date: "2018-08-08 23:27"
tag:
- HashMap
comments : true
---

> 본문 : https://d2.naver.com/helloworld/831311

# 댓글

* Java HashMap의 작동방식 뿐만아니라 자료구조로써 hashTable의 개념과 충돌 해결 방식에 대해서 이해하는데 큰 도움이 되었습니다. 다만 본문 내용 중에 이해가 힘든 부분이 있어서 질문드리는데요,

* HashMap에서는 Key의 해쉬값을 버킷으로 삼아 객체를 저장하는데, 이 해쉬값이 유일한 경우는 불가능하기 때문에 같은 해쉬값이 나오는 경우 발생하는 충돌을 해결하기 위해서 개방 주소법, 체이닝등이 고려되고 Java HashMap은 체이닝을 이용해서 이 문제를 해결한다는 것으로 이해했습니다.

* 그리고 이 체이닝 방법 또한 Java7에서는 LinkedList기반의 체이닝이고 Java8에서는 탐색 성능 이슈로 인해서 트리로 구현을 하는걸로 보았는데요. 이 트리에 노드 삽입 방식이 약간 헷갈리네요.. 본문에는 '해쉬값'의 대소를 이용해서 노드를 순회한다고 나와있는데, 같은 트리(같은 버킷)에 소속된 객체들의 해쉬값은 모두 같은 것 아닌가요? 해쉬값이 같아서 같은 버킷을 찾아와서 충돌이 발생했고, 충돌을 해결하기 위해 생성된 트리에 삽입될테니까요..

* 그리고 바로 다음에 이어지는 설명에서 '트리 순회 시 사용하는 대소 판단 기준은 해시 함수 값이다. 해시 값을 대소 판단 기준으로 사용하면 Total Ordering에 문제가 생기는데, Java 8 HashMap에서는 이를 tieBreakOrder() 메서드로 해결한다.' 라고 되어 있는데 여기서 문단 초에 문장인 '트리 순회시 사용하는 대소 판단 기준은 해시 함수 값(hashcode())이다'가 같을 수 밖에 없으니 말이 안되지 않나.. 싶습니다.

* 코드를 찾아보니 실제로는 java8에서 hashmap에 경우 tree로 체이닝을 구현할 때, 새롭게 identityHashCode()를 이용해서 해쉬값을 구해 대소를 판단한다고 하는데요. 자세히 나와있는 자료가 없어서,, 헷갈리네요.

## 대댓글

* 일단, 본문에 나와 있듯이 맵에 객체를 저장할 때는 해시 값 자체를 이용하는 것이 아닙니다. 따라서 HashMap을 비롯한 많은 해시 함수를 이용하는 associative array 구현체에서는 메모리를 절약하기 위하여 실제 해시 함수의 표현 정수 범위 hashmap3보다 작은 M개의 원소가 있는 배열만을 사용한다. 따라서 다음과 같이 객체에 대한 해시 코드의 나머지 값을 해시 버킷 인덱스 값으로 사용한다.

예제 2 해시를 사용하는 associative array 구현체에서 저장/조회할 해시 버킷을 계산하는 방법

int index = X.hashCode() % M;


* 이런식으로 M개의 해시 버킷에 저장을 하게 되는 것이고, 따라서 같은 버킷에 저장되어 있는 객체들의 해시 값이 동일한 것이 아닙니다. 다음으로, Total Ordering에 문제가 생긴다는 것은 모든 객체에 대해 유일한 해시코드를 생성할 수 없어서 발생하는 것입니다. tieBreakOrder() 메서드는 동일한 해시코드를 가진 객체에 대해 대소비교를 해야할 필요가 있을 때 쓰이는 것이구요.



# Map - HashTable, HashMap

  [d9b8d2d0]: http://swalloow.tistory.com/40 "해쉬맵"
  [출처 : JAVA의 HashMap, HashTable][d9b8d2d0]

### HashMap은

* 자바 컬렉션에서 제공하는 Map 인터페이스는 키(key)와 값(value)을 묶어서 하나의 데이터로 저장하는 구조입니다.

* Set 구조와 달리 중복을 허용하는 특징이 있습니다.

* 키(key)의 경우에는 유일해야 하지만 값(value)은 데이터의 중복을 허용합니다.

* HashMap은 내부적으로 해싱(Hashing)을 이용해서 구현한 컬렉션이기 때문에 많은 양의 데이터를 검색하는데 있어 뛰어난 성능을 보입니다.

### HashTable 또한

* Map 인터페이스를 구현한 구조입니다.

* 하지만 1.2 버전 이후부터 HashMap이 나오면서 HashTable에 비해 다양한 함수를 제공하는 HashMap으로 대체되었습니다.

* HashTable과 HashMap의 차이는 null 값을 허용하는데에 있습니다.

* HashTable은 null 값을 허용하지 않지만, HashMap은 null 값을 허용합니다.



### 추가

* HashMap API 문서에 따르면, "This implementation is not synchronized" 라고 명시되어 있습니다.

* 따라서, 멀티쓰레드를 사용하는 경우에는 Collections.synchronizedMap 으로 매핑시켜 주어야 합니

# JAVA의 HashMap 구현

* HashMap의 위와 같은 특징 때문에 관계형 데이터베이스(RDBMS)와 유사한 모습을 가지고 있습니다.

우선 주요 메서드와 사용법을 알아보겠습니다.


Method  |  Description
--|--
void clear()	  |   HashMap에 저장된 모든 객체를 제거
Set entrySet()  |   HashMap에 저장된 키와 값을 결합된 형태로 저장 후 반환
Object get(key)  |  지정된 키의 객체를 반환
Set keySet()  |   HashMap에 저장된 모든 키의 Set을 반환
Object put(key, value)  |   지정된 키와 값을 HashMap에 저장
Object remove(key)  |  HashMap에서 지정된 키로 저장된 값을 제거
int size()  |   HashMap에 저장된 요소의 개수를 반환
Collection values()  |    HashMap에 저장된 모든 값을 컬렉션의 형태로 반환



* 이제 HashMap을 이용한 코드를 통해 어떻게 적용하는지 알아보겠습니다.

아래는 대학교에 다니는 학생들을 저장하는 collection 구조입니다.


- 학생은 이름, 학년, 전공 값을 가지며 중복이 가능합니다. (동명이인, 동기...)

- 대부분 대학교는 학번으로 학생을 구분하기 때문에 키(key)는 학번이 됩니다.

public class Student {
    private String name;
    private String grade;
    private String major;

    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public String getGrade() {return grade;}
    public void setGrade(String grade) {this.grade = grade;}
    public String getMajor() {return major;}
    public void setMajor(String major) {this.major = major;}

    public Student(String name, String grade, String major) {
        this.name = name;
        this.grade = grade;
        this.major = major;
    }
}

<<<<<<< HEAD
=======

학생은 이름, 학년, 전공 값을 가지며 중복이 가능합니다. (동명이인, 동기...)

대부분 대학교는 학번으로 학생을 구분하기 때문에 키(key)는 학번이 됩니다.


>>>>>>> 5ad9c99dca84ffc288fb4960c1e8ff02db62e064
public class CHashMap {

    static HashMap<Integer, Student> student = new HashMap<Integer, Student>();

    public static void main(String[] args) {
        student.put(1111, new Student("김예시", "3학년", "영문과"));
        student.put(2222, new Student("정예시", "1학년", "영문과"));
        student.put(3333, new Student("김예시", "2학년", "컴공과"));
        student.put(4444, new Student("이예시", "4학년", "중문과"));
        student.put(5555, new Student("문예시", "4학년", "의예과"));

        printKey();
        printValue();
        printAll();
    }

    static public void printKey() {
        System.out.println("this is key----------");
        for(int key : student.keySet()) {
            System.out.println(key);
        }
    }

    static public void printValue() {
        System.out.println("this is value----------");
        for(Student student : student.values()) {
            System.out.println(student.getName()+" "+student.getGrade()+" "+student.getMajor());
        }
    }

    static public void printAll() {
        System.out.println("this is all----------");
        for(int key : student.keySet()) {
            System.out.println(key+" "+student.get(key).getName()+" "+student.get(key).getGrade()+" "+student.get(key).getMajor());
        }
    }
}


먼저 put 함수를 이용하여 키(key)와 값(value)을 저장했습니다.

그리고 키를 출력하는 함수, 값을 출력하는 함수, 키와 값을 함께 출력하는 함수를 만들었습니다.

위와 같이 keySet() 함수를 사용해도 되고 values() 함수를 사용해도 됩니다.

예시에는 안나와 있지만 entrySet() 함수를 사용해도 되기 때문에 상황에 맞게 편한대로 !

Key  |   Value(name) |  Value(grade) |   Value(major)
--|---|---|--
5555  | 문예시  | 4학년 |  의예과
3333  | 김예시  | 2학년 |  컴공과
1111  | 김예시  | 3학년 |  영문과
4444  | 이예시  | 4학년 |  중문과
2222  | 정예시  | 1학년 |  중문과

결과는 위와 같이 출력되며, 값에 대한 중복을 허용하는 것을 확인할 수 있습니다.

+ 추가

HashMap API 문서에 따르면, HashMap은 해당 map의 순서를 보장하지 않는다고 명시되어 있습니다.

따라서 출력결과가 HashMap에 넣은 순서와 관계없이 출력된 것입니다.

하지만, LinkedHashMap 클래스를 사용하면 삽입된 순서대로 반복문을 수행하도록 할 수 있다고 합니다.

# 사용

[* 구현 ](http://vaert.tistory.com/107)


import java.util.HashMap;

import java.util.Iterator;

import java.util.Map;

import java.util.Set;



pubilc class HashMapDemo

{

 public static void main(String[] args)

 {

  HashMap<String, Integer> fruitMap = new HashMap();

  fruitMap.put("사과", 1000);

  fruitMap.put("배", 2000);

  fruitMap.put("자두", 3000);

  fruitMap.put("수박", 4000);



  // get() --> Key에 해당하는 Value를 출력한다.

  System.out.println( fruitMap.get("자두") );   // 3000



  // values() --> 저장된 모든 값 출력

  System.out.println( fruitMap.values() ); // [1000, 2000, 3000, 4000]



  // HashMap에 넣은 Key와 Value를 Set에 넣고 iterator에 값으로 Set정보를 담에 준다.

  // Interator itr = fruitMap.entrySet().interator(); 와 같다.

  Set<Entry<String, Integer>> set = fruitMap.entrySet();

  Interator<Entry<String, Integer>> itr = set.interator();

  while (itr.hasNext())

  {

   Map.Entry<String, Integer> e = (Map.Entry<String, Integer>)itr.next();

   System.out.println("이름 : " + e.getKey() + ", 가격 : " + e.getValue() + "원");

  }

}



* 결과

```이름 : 사과, 가격 : 1000원```

```이름 : 배, 가격 : 2000원```

```이름 : 자두, 가격 : 3000원```

```이름 : 수박, 가격 :  4000원```
