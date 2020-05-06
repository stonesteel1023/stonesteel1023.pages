# 워드프레스(WordPress) xmlrpc.php 취약점 공격 막는 방법
- 스크랩 : https://www.itopening.com/379/ , ![그외](https://www.thewordcracker.com/intermediate/%EC%9B%8C%EB%93%9C%ED%94%84%EB%A0%88%EC%8A%A4-xmlrpc-php%EB%A5%BC-%ED%86%B5%ED%95%9C-brute-force-%EA%B3%B5%EA%B2%A9-%EC%B0%A8%EB%8B%A8%ED%95%98%EA%B8%B0/)


개인홈페이지 및 블로그 운영을 하다보면 신경써야될 문제들이 한 두가지가 아니다. 
서버구입을 하거나 임대를 통해서 워드프레스 블로그를 운영해야되는 개인블로그 사용자들에게는 서버운영비용만큼 큰 문제가 보안이다.

플러그인을 통해서 손쉽게 설치하면 해결 할 수 있는 xmlrpc.php 취약점 공격을 서버단에서 막나보는 방법을 할 것이다. 워드프레스는 다양한 플러그인들이 있지만 너무 많은 플러그인을 설치하면 블로그 성능하락이 발생할 수 있고, 보안에 오희려 더 취약해질 수 있다.

그래서 블로그 운영자가 기본적인 리눅스 서버 운영을 할 줄 알아야 될 것이다.

xmlrpc.php 취약점 대처 방법

xmlrpc.php 공격을 당하게되면 사진과 같이 apache access 로그에서 /xmlrpc.php 파일 호출을 여러번 시도한 흔적을 확인할 수 있다. 확인결과 필자의 서버에 185.188.204.6 해외IP에서 xmlrpc.php 공격을 시도한 흔적을 확이할 수 있었다.

서버가 다운된 상태라면 리부팅을 진행하거나 프로세스를 완전히 죽이고 다시 실행하는것이 올바른 판단일 것이다.

그리고 xmlrpc 공격은 해커들이 다른 사이트를 공격하기 위한 일종의 ‘좀비 서버’를 만드는 과정이라고 한다. ddos 공격의 경유지로 사용되는 것이다.

.htaccess 파일 수정

xmlrpc.php 공격을 당하기 싫다면 .htaccess 파일을 수정해야 된다. 자신의 홈디렉토리 최상위(root)디렉토리에 .htaccess 생성후 아래와 같이 입력해준다.

```php
<Files xmlrpc.php>
Order Deny,Allow
Deny from all
allow from 123.123.123.123
</Files>
```

allow from 부분에서는 접근 허용하고 싶은 ip를 입력해주면 된다. 위와 같이 처리하면 더 이상 xmlrpc.php 공격을 당하지 않게 된다.

# 그외 방법 - 보안 플러그인에서 XML-RPC 비활성화 설정하기, XML-RPC 비활성화 플러그인 사용
