+++
title = "Lua book reading"
description = "Study Lua"
date = 2023-05-04
toc = true

[taxonomies]
categories = ["Lua"]
tags = ["Lua", "Language"]

[extra]
math=true
+++

---

*본 포스트는 "한빛미디어"에서 출간한 "루아 프로그래밍 가이드"를 참고하여 작성되었습니다.*

---

# 3. 언어
3장에서는 `Lua` 에 대해 *프로그래밍 언어론* 적 관점에서 설명하고 있습니다.
이번 학기에`프로그래밍 언어론` 강의를 수강하고 있는데, 수업 때 배운 **BNF**나 **토큰**과 같은 용어들이 눈에 띄었습니다.

## 3.1 어휘 규칙(Lexical Conventions)
`Lua` 는 ***자유 형식 언어([wiki](https://en.wikipedia.org/wiki/Free-form_language))*** 이기 때문에, 프로그램을 기술한 텍스트에서 어휘 요소(token)들이 어느 위치에 있는지가 크게 중요하지 않습니다. 즉, 줄바꿈, 공백 문자, 주석 등을 무시합니다.

`Lua`는 ***변수***, ***테이블 필드***, ***레이블*** 을 가리키기 위해 `식별자`를 사용합니다. `식별자`는 ***문자***, ***숫자***, ***언더스코어(_)*** 로 이루어진 문자열이며, 숫자로 시작할 수 없습니다.

아래의 ***키워드*** 들은 예약어로 사용되며, `식별자`로 사용할 수 없습니다.
```
and     false     local     then      break     for
nil     true      do        function  not       until
else    goto      or        while     elseif    if
repeat  end       in        return
```

`Lua`는 *case-sensitive language* 입니다. 따라서, 예약어인 and 대신 And 나 AND 는 `식별자`로 사용할 수 있습니다.

관용적으로, `_VERSION`과 같이 ***언더스코어(_)*** 와 ***대문자*** 로 시작하는 이름들은 `Lua`에서 사용하는 변수의 이름으로 예약됩니다.

아래의 문자열들 또한 `Lua`에서의 토큰입니다.
```
+       -         *         /         %         ^       # 
==      ~=        <=        >=        <         >       = 
(       )         {         }         [         ]       ::
;       :         ,         .         ..        ...       
```

***리터럴 문자열*** 은 `''(single quote)`와 `""(double quote)`로 구분되며, `C` 에서 사용하는 *escape sequence*를 포함할 수 있습니다.

***리터럴 문자열*** 안의 바이트를 숫자로 표현하는 것도 가능합니다.
- 16진수: '\xXX'
- 10진수: '\ddd'

그리고 종결 문자인 '\0' 을 포함한 모든 8 비트 값을 문자열에 포함할 수 있습니다.
