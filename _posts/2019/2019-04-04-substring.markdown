---
layout : post
date : 2019-04-04 21:45
title : "NULL나 스페이스가 올때 Trim하는 메소드"
tag : java
comments : true
---

### 메소드를 사용하게 된 계기
- ip어드레스를 받아오는 메소드가 필요했었는데 PT환경에서는 12자리, IT환경에서는 15자리가 되버렸다.
- 분명 실장 전에 찾아봤던 자료에서는 ipv4인 경우에는 12자리라고 본 것 같아서 substring(0, 12)를 사용했었는데
> IPv4 주소란?
 IPv4주소는 32비트 길이의 식별자로 0.0.0.0~255.255.255.255까지의 숫자의 조합으로 이루어지며 총 네구간으로 나눠져있으며 최대 12자리의 번호로 이루어져 있습니다. IPv4를 통해 최대 약 43억개의 서로 다른 주소를 부여할 수 있습니다. 하지만 전세계 공용으로 사용되며 인터넷 사용자수가 급증하면서 IPv4주소가 고갈될 문제에 처해있습니다. 이러한 고갈 문제를 해결하기 위해 등장한 주소가 바로 IPv6입니다.
 [출처] IP주소란? (IPv4와 IPv6)|작성자 hostinggodo
 
- IT환경에서보니 뒤에가 짤려있는 것이 아닌가!! 다시 자료를 찾아보니 최대 12자리... 즉 "."가 3개 들어가니 스트링으로 볼땐 최대 15자리수까지 되는 것이었다. 그리고 정해져있는 것도 아니니 새로운 메소드가 필요할 수 밖에.

### 프레임워크에서 찾았다. 무단복사 ㅎㅎ
```java
public static String delLastDelimiter(String str, String delim) {
		if (str != null && delim != null) {
			int delimPos = str.lastIndexOf(delim);
			return delimPos == -1 ? null : str.substring(0, delimPos);
		} else {
			return null;
		}
	}
 ```
