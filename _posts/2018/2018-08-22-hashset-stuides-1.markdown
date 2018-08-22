---
layout: "post"
title: "Hashset-stuides-1"
date: "2018-08-22 21:12"
---

# [컬렉션 프레임워크 - List, Set 인터페이스][6dad1c65]

  [6dad1c65]: http://ehpub.co.kr/java-%ED%99%9C%EC%9A%A9-3-10-hashset-%ED%81%B4%EB%9E%98%EC%8A%A4/ "언제나 휴일님의 블로그"

Collection 인터페이스를 기반으로 구현 클래스에는 List와 Set이 있음. List 클래스는 선형 자료구조를 구현한 클래스이며 Set은 비선형 자료를 구현한 클래스

* 그리고 Set 클래스를 기반으로 파생한 StoredSet, HashSet 클래스가 있음. StoredSet은 이진 탐색 트리를 구현한 클래스이며 HashSet은 해쉬 테이블을 구현한 클래스. 두 가지 모두 빠른 검색이 필요할 때 사용하는 클래스이며 같은 자료를 중복 보관할 수 없는 특징을 갖고 있음

* 선형 자료구조에서의 탐색 비용은 O(N)이고 이진 탐색 트리의 탐색 비용은 (logN), 해쉬 테이블의 탐색 비용은 O(1)

* 여기에서는 Set을 기반으로 파생한 HashSet 클래스 사용법을 알아봄. StoredSet 클래스도 Set을 기반으로 파생한 클래스여서 사용 방법은 HashSet 클래스와 비슷

* HashSet 클래스에는 자료를 보관하는 add 메서드, 삭제하는 remove 메서드, 모든 요소를 삭제하는 clear 메서드, 자신을 복제한 개체를 반환하는 clone 메서드, 특정 요소를 보관하였는지 판별하는 contains 메서드, 비었는지 판별하는 isEmpty 메서드, 반복자를 반환하는 iterator 메서드, 보관한 요소 개수를 반환하는 size 메서드가 있음

 # [Set 인터페이스 - HashSet, TreeSet, LinkedHashSet][f0d22ece]

  [f0d22ece]: https://onsil-thegreenhouse.github.io/programming/java/2018/02/21/java_tutorial_1-23/ "참조 : Onsil's Blog"

클래스	| 특징
--|--
HashSet	| 순서가 필요없는 데이터를 hash table에 저장. Set 중에 가장 성능이 좋음
TreeSet	| 저장된 데이터의 값에 따라 정렬됨. red-black tree 타입으로 값이 저장. HashSet보다 성능이 느림
LinkedHashSet	| 연결된 목록 타입으로 구현된 hash table에 데이터 저장. 저장된 순서에 따라 값이 정렬. 셋 중 가장 느림

* API - 메소드
*
<img src="https://lh3.googleusercontent.com/oJOIcgGVlgUz0ItdpZn8Wm1ObMvpGA1quZlKTE-_bruR1N6hnMMavqb00xPaQA-5HnGIWW4v_vmiumFaoYGpan-aGXEdFv-Iyi86oBEVmV8Vv97wMXx7ETlU25CgemxTk1V600dm7PmWVzVbBNKc4mZ2-UW9VcGiSTMEGh-qJN5buNI1S2Ogc0IORwtEwM_U6-Vjz4cS9VXSolRb3t3KGLzXD_oH2Ly8VoM0C4uG1S54pczFhskzpzdbp3PIaW_qYDCz3msgeUPNoBAIUBe_DTmT9JrpJjOAu2hLMO26kHHuqefQ6Vza0HK1lOAZlMmUgbZa67PTJb0JuRmzyWnR9of9nsOl9BGar-OccK9koUuYc8vvR5Cl5EvBEFhJDk4zKtOYrpVyGUKSMgjmu9ylfhBWPPh5oE7KejudTL5sk3Wu73tYu7yOiK53xWcxmcXjJC1lP-v7x9-tQh1JT0ehgnqIU0EYS38YJl4AhultBcE3TkbM0eB5THMHY9ptHSqxwKmuSLe6GWVWD_EDo_vEFc43MrIHrP_1CJ4CI2HDd2WXXBBT8I0uCB_IYiqs32GnWn1uNc8Oa8Q0blBA89h_4ACQ-CX_VquM8OIHVJU=w1335-h612-no" alt="Java_HashSet_method" />

# [HashSet (java.util.Hashset)][683d8088]

  [683d8088]: http://uoonleen.tistory.com/22 "Flatinum' tistory"

* 컬렉션 클래스 : HashSet (java.util.HashSet)

 - 저장하려는 객체에 대해 hashCode()를 호출하여

   그 리턴 값을 가지고 저장할 위치를 계산한다.

 - null 값을 저장할 수 있다.

 - 값을 중복 저장할 수 없다.

   > 이유 : '집합'이기 때문

 - 값을 넣을 때 인스턴스의 해시값을 기준으로 저장하기 때문에

   순서대로 저장되지 않는다.

   그래서 값을 꺼낼 때도 순서대로 꺼낼 수 없다.

 - 값을 꺼낼 때 숫자 인덱스로 꺼낼 수 없다.

   > 예 : HashSet

* Set 컬렉션에서 값을 꺼내는 방법

 1) 배열을 리턴 받는다. (입력 순서대로 리턴 받을 수 없다.)

- toArray()

`Object[] valueList = dateSet.toArray(); for (int i = 0; i < valueList.length; i++) {System.out.println(valueList[i]); }`

 2) 값을 꺼내주는 메서드를 이용한다.

 - iterator()
`Iterator iterator = dateSet.iterator(); while (iterator.hasNext()) { System.out.println(iterator.next()); }`

* 주의!
  HashSet 컬렉션에 값을 저장할 때,
  인스턴스의 주소가 영향을 끼치는 것이 아니라
  hashCode()의 리턴 값이 영향을 끼친다.

* HashSet과 hashCode() 메서드 예제

public static void main(String[] args) {

 HashSet set = new HashSet();

 set.add(new String("홍길동"));
 set.add(new String("임꺽정"));
 set.add(new String("유관순"));
 set.add(new String("윤봉길"));
 set.add(new String("안중근"));
 set.add(new String("김구"));
 set.add(new String("김구"));
 
   /* "김구" 문자열인 경우엔 서로 다른 인스턴스 임에도 불구하고 중복되지 않는다.
     *  왜?
     *  => Set은 객체(의 주소)를 저장할 때 그 객체에 대해 hashCode() 메소드를 호출한 후
     *     그 리턴 값을 위치를 계산한다.
     *  => String 클래스는 같은 값을 갖는 경우 같은 hash value를 리턴하도록
     *     hashCode()를 오버라이딩 하였다.
     *     즉 위의 두 개의 "김구" String 객체는 비록 인스턴스는 다르지만
     *     값이 같기 때문에 hashCode()의 리턴값도 같다.
     *     그래서 위치를 계산한 값도 같다.
     *     위치 계산 값이 같아서 같은 값으로 간주하기 때문에 중복 저장되지 않는다.
    */

 // 증명! -> 값이 다 똑같이 출력된다.
 System.out.println(new String("김구").hashCode());
 System.out.println(new String("김구").hashCode());
 System.out.println(new String("김구").hashCode());


 /* 결론!
    * HashSet 컬렉션에 값을 저장할 때,
    * 인스턴스의 주소가 영향을 끼치는 것이 아니라
    * hashCode()의 리턴 값이 영향을 끼친다.
 */

  Iterator iterator = set.iterator();
  while (iterator.hasNext()) { // 꺼낼 데이터가 있는가?
  System.out.println(iterator.next());
  
  }
}
