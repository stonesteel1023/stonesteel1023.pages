---
layout : post
date : 2019-08-28 10:30
title : "이중 for문 성능개선"
comment : true
tag : java
---

> 인트로
  이중 for문은 시간복잡도를 배로 늘리는 가장 큰 원인이지만 피치 못하게 사용할 때가 있다.
  바로 오늘처럼 현장에서 파일이름에 윈도우에서 사용금지되는 문자가 포함되지 않는 사양을 추가하라는 지시를 받고
  스트링 배열에 금지문자 9개를 넣어두고 파일이름을 추출한 후 비교하는 for문을 짜던 순간...
  

# 개선방안 1 : size(), length(), length 같은 크기비교 메소드 호출을 최소화하기

# 개선방안 2 : 자료구조를 사용하는 부분에서 불필요한 반복구문을 추출해버리기


> 참고 : https://12bme.tistory.com/92
> https://stackoverflow.com/questions/7457879/algorithm-to-optimize-nested-loops
