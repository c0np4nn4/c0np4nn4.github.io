+++
title = "Lua2"
description = "PL"
date = 2023-05-18
toc = true

[taxonomies]
categories = ["Lect", "PL"]
tags = ["PL"]

[extra]
math=true
+++

---
> 프로그래밍 언어를 빨리 배우는 방법
> > - 보통 20 챕터로 구성됨
> > - 한 챕터당 보통 10개의 샘플 프로그램이 존재
> > - 약 200개의 프로그램이 존재
> > - 따라서, 그 200개의 프로그램을 다 쓸 수 있다면 언어를 *코드 중심*으로는 이해할 수 있음
> > ---
> > - 일단 그대로 타이핑
> > - 조금 더 변형

<txtred>수행 예상</txtred> -> <txtylw>Typing</txtylw> -> <txtylw>실행</txtylw> -> <txtylw>변경</txtylw> -> <txtylw>실행</txtylw>

```
수행 예상 -> Typing -> 실행 -> 변경 -> 실행
|                      |     |
+----------------------+     |
      [비교]------------------+
         |
         +-------> 책을 읽자
```

---

<details>
<summary>코드 예시</summary>
<p>

```lua
-- msg = "Hello"

-- print(msg)
-- print(msg:sub(1, 2))

-- for i = 1, 10 do
--   print(i)
-- end

------------------------
-- Date: 18 MAY 2023
-- [+] Table in Lua
-- ns = {10, 20, 30}

-- [-] Table for a Record
-- pnt = { x = 3, y = 4 }
-- print(pnt["x"] + pnt["y"])

-- [-] Mixed Table
-- fav = { name = "Lua", year = 1993, "script", "dynamic", "fast" }
-- print(fav["name"])
-- print(fav["year"])

-- print(fav[0])
-- print(fav[1])
-- print(fav[2])
-- print(fav[3])

-- [+] Scanning Table
-- `Generic for`
-- Scanning Keys and Values
-- fav = { name = "Lua", year = 1993, "script", "dynamic", "fast" }
-- for k in pairs(fav) do
--   print(k)
-- end

-- Scanning Index and Values
-- fav = { name = "Lua", year = 1993, "script", "dynamic", "fast" }
-- for k, v in pairs(fav) do
--   print(k, v)
-- end

-- fav = { name = "Lua", year = 1993, "script", "dynamic", "fast" }
-- for i, v in ipairs(fav) do
--   print(i, v)
-- end

-- String Length
-- fav = { name = "Lua", year = 1993, "script", "dynamic", "fast" }
-- print(#fav["name"])

-- Length of list in Table
-- print(#fav)

-- fav = { name = "Lua", year = 1993, "script", "dynamic", "fast", "small", "like C" }
-- print(#fav)

--------------------------
-- [+] Math, Modular Operation
-- [-] Using math library
a = math.fmod(17, 5)
print(a)

-- [-] Using a formula
-- "a = b * q + r"
a = 17 - math.floor(17 / 5) * 5
print(a)

-- [-] Using math library

```

</p>
</details>

# Table

- <txtred>연관 배열</txtred> (Associative Array)
- `Table for a Record`

- 한 테이블 안에 record, list 를 모두 저장할 수 있음

---

# Modular Operation
- math library
  - math.fmod()
- formula
  - $a = b \times p + r$
> - `mod` 연산에 대해 언어마다 서로 다른 개념을 갖고 구현 될 수도 있음(?)

---

# Comment

```lua
-- 한 줄
-- [[ 여러줄
여러줄]]
```

---

# No Empty Statements but..

```lua
if m == nil then --[[empty statement]] else
  print(m)
end
```

---

# Function
- Definition

