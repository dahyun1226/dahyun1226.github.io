---
title: Jekyll Bloging 시작하기
categori: Bloging
---

네이버 블로그를 쓰다가 깃헙에서 제공하는 정적 사이트로 바꾸고 싶어졌다. 
내가 원하는대로 테마 - 웹사이트를 바꾸는 것은 분명히 매력적이니까.
하지만 바꾸는 데 여러 우여곡절이 있었다.
가장 큰 문제는 css의 적용이었다. 내가 원하는대로 바꾸고 싶었으나 asset의 scss 파일을 바꿔도 localhost로 띄운 내 블로그의 css는 바뀌지가 않는 것이다.
그래서 하나하나 분석을 약간씩 해보기 시작했다. 원래 계획은 블로깅하면서 천천히 분석 해보려 했는데 시작부터 막혀버려서 굉장히 답답했었다. 시간도 되게 지체되었고.
웹은 문외한 수준이라... scss liquid gem 다 처음보기도 했고 말이다.
하지만 결국 link를 찾아냈다. includes에  node의 link.liquid에서 stylesheet 하이퍼링크를 찾은 것이다. 
문제의 원인은 깃헙 버전에 link되어 있던 것이었다. 그래서 아무리 내가 바꿔도 안되는거지. 근데 와이파이 안되어있을 때도 그랬던 것 같다. 
아무튼, 그 결과 새롭게 테마를 원하는대로 적용 할 수 있었다. 아마 css는 시작일 것이다. 원하는대로 구현하는건 더 어렵겠지만, 방법을 하나하나 알아가는 재미도 분명 있을 것 같다.
가장 중요하다고 할 수 있는, 링크를 바꾼 코드는 다음과 같다.
{% comment %} theme {% endcomment %}
<link rel="stylesheet" href="/assets/css/theme.css">