+++
title = "블로그에 대한 고민"
date = "2024-04-15"
+++

블로그 이전을 고민했었다.
가장 큰 이유는 `zola` 를 이용해 블로그를 운영하면서 느낀 불편함들 때문이다.
- markdown 으로 작성된 내용과 웹사이트에 렌더링 되는 모습이 다르다.
  - obsidian 등에서 예쁘게 정리한 내용을 그대로 사용할 수 없다는 점
  - [bear](https://bear.app/), [marktext](https://www.marktext.cc/) 등도 마찬가지
- 이미지를 넣는 작업이 여간 불편한게 아니다.
  - 작성할 때의 폴더 구조가 아니라, `zola build` 를 통해 생성되는 `public/` 폴더 내의 구조를 생각해야 한다.

따라서, 아래의 같은 선택지들을 고려해보았다.
- `velog`
- `tistory`
- `hackmd`
- `medium`
- `naver blog`
- `github page w/o zola`

다양한 플랫폼, 다양한 블로그들을 살펴보다 문득 ***나는 왜 블로그를 운영하려고 할까?*** 하는 의문도 생겼다.
사실 obsidian 내에 글을 적는 것만으로도 `기록하는 습관`을 기르기에는 충분하다고 생각한다.
이에 관해서는 좀 더 고민한 후 글을 남겨보고 싶다.
일단 현재는 조금 더 신경을 써서 글을 남기게 된다는 생각이다.

며칠 간의 고민 끝에 내린 결론은 "<u>***github blog*** 를 그대로 유지하자</u>"이다.

첫 번째 불편함은 아래 그림처럼 `velog` 처럼 화면을 구성하여 해결하면 되었다.

<center>
<img alt="blog writing ui" src="../../misc/blog_1.png" />
</center>

두 번째 불편함은.. 익숙해지는 수 밖에는 없는 것 같다.
어쩌면 `zola docs`에 해결책이 있지 않을까 싶기도 하다.
그리고 사실 "이미지를 추가하는 것이 불편하다"는 생각은 *블로그 글 작성* 에 대한 내 생각을 점검하게 했다.
좋은 퀄리티의 글을 작성하는 것에 좀 더 집중해야겠다고 다짐했다.