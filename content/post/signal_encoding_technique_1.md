+++
title = "Encoding and Modulation"
description = "Signal Encoding Technique (1)"
date = 2023-03-20

[taxonomies]
categories = ["Lect"]
tags = ["Data communication", "Network", "CB24149"]

[extra]
math=true
+++

---
# Signal
- **Data** 는 어떠한 의미나 정보를 의미한다.
- **Signal** 은 이러한 **Data** 의 *전자, 전자기적* 표현 이라 할 수 있다.

## Analog
- 끊임없이 변화하는 전자기파 (e.g. 사인파)
- 유선 뿐만 아니라, 광섬유, **무선** 등 <u>다양한 매체</u>를 통해 전달될 수 있음
- 신호가 전달될 때, *세기가 감쇠하는 정도가 낮음*

## Digital
- 전압의 변화 (e.g. square wave)
- <u>유선 매체</u>를 통해 전달될 수 있음
- 저렴하며, <u>노이즈의 간섭에 덜 민감함</u>
- 신호가 전달될 때, *세기 감쇠의 정도가 큼*

### "_x_" Data on "_y_" Signal
- *Digital* Data 는 *Digital* Signal 을 이용해 전송할 수 있다.

- *Analog* Data 는 *Analog* Signal 을 이용해 전송할 수 있다.

- 그렇다면, <u>*Digital* Data</u> 를 <u>*Analog* Signal</u> 을 이용해 전송할 수 있을까?
  - 이를 위해 `Modem` (Modulator / Demodulatr)을 이용함
  - *Digital* Data 를 *Analog* carrier frequency 로 전조(modulate) 하는 식으로 동작함

- 반대로, <u>*Analog* Data</u> 를 <u>*Digital* Signal</u> 을 이용해 전송할 수 있을까?
  - `Codec` (Coder / Decoder) 를 이용함

---
# Transmission
- ***Signal*** 이 전송되는 과정에서, content 를 고려하는지 여부에 따라 분류됨
- 전송 과정에서 <u>세기가 감쇠하는 현상</u> + <u>노이즈가 발생하는 현상</u>이 있음
  - 신호 세기를 *boost* 하는 과정이 필요함.
  - 이를 해주는 장치가 `Relay` Device 임.
- **Analog** Transmission
  - *Analog* Signal 만을 사용함
  - `Relay` 과정에서 단순히 ***amplifying*** 하기 때문에 *Noise* 도 함께 커짐.
    - 즉, content 에 대한 고려를 하지 않음
- **Digital** Transmission
  - *Analog*, *Digital* Signal 둘 다 가능함
  - `Relay` 과정에서 원래 Signal 을 ***regenerate*** 하기 때문에, *Noise* 문제를 해결할 수 있음
  - 이 때의 `Relay` 장치를 `Repeater` 라고 부름.
    
---
# Encoding, Modulation
- 앞에서 보았듯이, *Digital* 과 *Analog* 는 서로 *Data* 를 *Signal* 에 태우는 것이 가능함
- 따라서, 임의의 Signal을 입력값으로 할 때,
  - ***Digital*** Signal 로 다루는 것을 `Encoder`, `Decoder` 라 한다.
  - ***Analog*** Signal 로 다루는 것을 `Modulator`, `Demodulator` 라 한다.
    - `Modulator` 는 추가적으로 `Carrier Frequency` 정보를 필요로 한다.
