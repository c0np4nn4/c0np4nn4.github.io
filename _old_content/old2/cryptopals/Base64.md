+++
title = "Base64"
date = 2023-02-26

[taxonomies]
categories = ["Base64"]
tags = ["Base64","Rust"]

[extra]
toc = true
+++

# BASE64

- <mark>BASE64</mark> 는 *binary-to-text encoding scheme* 의 일종으로, 이름에서와 같이 <mark>6-bit</mark> ($2^6 = 64$) 값을 다룹니다.

- [[wiki](https://en.wikipedia.org/wiki/Base64)] 에 따르면, 아래와 같은 표를 이용해 <mark>6-bit</mark> 값을 <mark>text</mark>로 인코딩한다고 합니다.

<center>

|Index|Binary|Char||Index|Binary|Char||Index|Binary|Char||Index|Binary|Char|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|0|000000|A||16|010000|Q| |32|100000|g| |48|110000|w||
|1|000001|B| |17|010001|R| |33|100001|h| |49|110001|x| |
|2|000010|C| |18|010010|S| |34|100010|i| |50|110010|y| |
|3|000011|D| |19|010011|T| |35|100011|j| |51|110011|z| |
|4|000100|E| |20|010100|U| |36|100100|k| |52|110100|0| |
|5|000101|F| |21|010101|V| |37|100101|l| |53|110101|1| |
|6|000110|G| |22|010110|W| |38|100110|m| |54|110110|2| |
|7|000111|H| |23|010111|X| |39|100111|n| |55|110111|3| |
|8|001000|I| |24|011000|Y| |40|101000|o| |56|111000|4| |
|9|001001|J| |25|011001|Z| |41|101001|p| |57|111001|5| |
|10|001010|K| |26|011010|a| |42|101010|q||58|111010|6| |
|11|001011|L| |27|011011|b| |43|101011|r||59|111011|7| |
|12|001100|M| |28|011100|c| |44|101100|s||60|111100|8| |
|13|001101|N| |29|011101|d| |45|101101|t||61|111101|8| |
|14|001110|O| |30|011110|e| |46|101110|u||62|111110|+| |
|15|001111|P| |31|011111|f| |47|101111|v||63|111111|/| |
|-|padding|=|

</center>

## Example

- 이해를 돕기 위해, <kbd>0x123456</kbd>을 base64 encoding 하는 과정을 살펴보겠습니다.

- <kbd>0x123456</kbd>은 binary로 변환하면 <kbd>0b000100100011010001010110</kbd> 입니다. 

- 가독성을 위해 <mark>6-bit</mark>씩 끊어서 나열하면 다음과 같습니다.
    - <kbd>0b000100_100011_010001_010110</kbd>
    - <kbd>000100</kbd>, <kbd>100011</kbd>, <kbd>010001</kbd>, <kbd>010110</kbd>

- 각각을 위 표와 대응되는 글자로 변환하면 아래와 같습니다.
    - <mark>E</mark>, <mark>j</mark>, <mark>R</mark>, <mark>W</mark>

- 따라서, base64_encode(<kbd>0x123456</kbd>) = <mark>EjRW</mark> 입니다.

---

## Padding

- 만약 <mark>6-bit</mark> 단위로 끊어지지 않는다면 적절한 ***padding*** 을 추가해야 합니다.

- ASCII 는 <mark>8-bit</mark> 단위이기 때문에, 이러한 ***padding*** 을 적절히 추가할 필요가 있습니다.

- 이해를 위해 <mark>ABC</mark> 라는 ASCII 문자열을 base64 encoding 하는 과정을 살펴보겠습니다.

### 1. No Padding

> - <mark>ABC</mark> 를 hex 값으로 변환하면 <kbd>0x414243</kbd> 입니다.
>
> - <kbd>0x414243</kbd>을 binary로 표현하면 <kbd>0b010000_010100_001001_000011</kbd>입니다.
>
> - binary 값이 <mark>6-bit</mark> 단위로 나눠지므로, base64 encoding 하면 <mark>QUJD</mark>입니다. 

### 2. Padding

> - <mark>AB</mark> 를 hex 값으로 변환하면 <kbd>0x4142</kbd> 입니다.
> 
> - <kbd>0x4142</kbd>를 binary로 표현하면 <kbd>0b010000_010100_0010</kbd> 입니다.
> 
> - binary 값이 <mark>6-bit</mark> 단위로 깔끔히 나눠지지 않는 것을 알 수 있습니다.
>
> - 이 때는, 맨 끝의 binary에 <mark>0</mark>을 붙여 <mark>6-bit</mark> 단위로 만들고, ***padding*** 을 의미하는 <mark>=</mark> 를 붙여 해결하면 됩니다.
> 
>     - <kbd>0b010000_010100_0010</kbd>
> 
>     - <kbd>0b010000_010100_001000_(padding)</kbd>
>
> - base64 encoding 하면, <mark>QUI=</mark> 입니다.
