---
layout : post
title : "SHA1방식 암호화 메소드"
date : 2019-04-01 10:17
tag : java
comments : true
---

# SHA1 algorithm

## 위키피디아 정의
- SHA(Secure Hash Algorithm, 안전한 해시 알고리즘) 함수들은 서로 관련된 암호학적 해시 함수들의 모음이다. 
- 이들 함수는 미국 국가안보국(NSA)이 1993년에 처음으로 설계했으며 미국 국가 표준으로 지정되었다. SHA 함수군에 속하는 최초의 함수는 공식적으로 SHA라고 불리지만, 나중에 설계된 함수들과 구별하기 위하여 SHA-0이라고도 불린다. 
- 2년 후 SHA-0의 변형인 SHA-1이 발표되었으며, 그 후에 4종류의 변형, 즉 SHA-224, SHA-256, SHA-384, SHA-512가 더 발표되었다. 이들을 통칭해서 SHA-2라고 하기도 한다.
- SHA-1은 SHA 함수들 중 가장 많이 쓰이며, TLS, SSL, PGP, SSH, IPSec 등 많은 보안 프로토콜과 프로그램에서 사용되고 있다. SHA-1은 이전에 널리 사용되던 MD5를 대신해서 쓰이기도 한다. 혹자는 좀 더 중요한 기술에는 SHA-256이나 그 이상의 알고리즘을 사용할 것을 권장한다.
- SHA-0과 SHA-1에 대한 공격은 이미 발견되었다. SHA-2에 대한 공격은 아직 발견되지 않았으나, 전문가들은 SHA-2 함수들이 SHA-1과 비슷한 방법을 사용하기 때문에 공격이 발견될 가능성이 있다고 지적한다. 미국 표준 기술 연구소(NIST)는 SHA-3로 불리는 새로운 암호화 해시 알고리즘에 대한 후보를 공모하였다.

## 크기
크기 비교
다음은 SHA 함수들의 특성을 요약한 표이다.

알고리즘 | 해시값 크기	| 내부 상태 크기	| 블록 크기	| 길이 한계	| 워드 크기	| 과정 수	| 사용되는 연산	| 충돌
-- | -- | -- | -- | -- | -- | -- | -- | -- |
SHA-0 | 160 | 160 | 512 | 64 | 32 | 80 | +,and,or,xor,rotl	|발견됨
SHA-1 | 160 | 160 | 512 | 64 | 32 | 80 | +,and,or,xor,rotl	|발견됨
SHA-256/224 | 256/224 | 256 | 512 | 64 | 32 | 64 | +,and,or,xor,shr,rotr	|-
SHA-512/384 | 512/384 | 512 | 1024 | 128 | 64 | 80 | +,and,or,xor,shr,rotr	|-

## 자바 method

```java
	private String getSHA1(String str) throws NoSuchAlgorithmException {
		// SHA1名生成
		MessageDigest md = MessageDigest.getInstance("SHA-1");
		md.update(str.getBytes());
		byte[] digest = md.digest();

		StringBuffer hexstr = new StringBuffer();
		String shaHex = EMPTY_STRING;
		for (int i = 0; i < digest.length; i++) {
			shaHex = Integer.toHexString(digest[i] & 0xFF);
			if (shaHex.length() < 2) {
				hexstr.append(0);
			}
			hexstr.append(shaHex);
		}

		return String.valueOf(hexstr);
	}
  ```
