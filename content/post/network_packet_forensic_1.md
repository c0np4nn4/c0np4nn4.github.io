+++
title = "Raw Packet Analysis"
description = "Network Packet Forensic 1"
date = 2023-03-23

[taxonomies]
categories = ["Forensic", "Network"]
tags = ["Network", "Forensic", "Security"]

[extra]
math=true
+++

- 본 포스트는 도서 [[SECURITY 네트워크 패킷 포렌식](http://www.yes24.com/Product/Goods/7380394)] 의 `3장, Raw 패킷분석` 을 참고하였음을 밝힙니다.

---

# Raw Packet
- 예를 들어 아래와 같은 Hex Stream 의 패킷을 분석한다고 해보겠습니다.

```
ff ff ff ff ff ff 00 17 31 45 8e 42 08 06 00 01
08 00 06 04 00 01 00 17 31 45 8e 42 7c 89 19 fe
00 00 00 00 00 00 7c 89 19 7a 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00
```

---

# 이더넷 헤더
- 우선 Hex Stream 의 앞부분 $14$-byte 는 `이더넷 헤더`를 의미합니다.

```
ff ff ff ff ff ff 00 17 31 45 8e 42 08 06 __ __
__ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __
__ __ __ __ __ __ __ __ __ __ __ __ __ __ __ __
__ __ __ __ __ __ __ __ __ __ __ __
```

- <mark>ff ff ff ff ff ff 00 17 31 45 8e 42 08 06</mark> 이며 아래의 세 부분으로 구분됩니다.
  - <mark>ff ff ff ff ff ff</mark>: `브로드캐스팅` 입니다.
  - <mark>00 17 31 45 8e 42</mark>: `발신자의 MAC 주소` 입니다.
  - <mark>08 06</mark>: `이더넷` 패킷 다음에 오는 프로토콜의 정보를 나타내는 값으로 대표적으로 아래의 값이 있습니다.
    - <mark>0806</mark>: `ARP` 헤더
    - <mark>0835</mark>: `RARP` 헤더
    - <mark>0800</mark>: `IP` 헤더

- 따라서, `이더넷` 뒤에 `ARP` 헤더가 옴을 알 수 있습니다.

---

# ARP 헤더

- 위 Hex Stream 에서 `ARP 헤더` 에 해당하는 부분은 아래와 같습니다.

```
__ __ __ __ __ __ __ __ __ __ __ __ __ __ 00 01
08 00 06 04 00 01 00 17 31 45 8e 42 7c 89 19 fe
00 00 00 00 00 00 7c 89 19 7a __ __ __ __ __ __
__ __ __ __ __ __ __ __ __ __ __ __
```

- `ARP` 프로토콜의 헤더는 아래와 같은 정보를 담고 있습니다.
- 괄호는 Byte 크기이며, *(?)* 는 가변 길이를 의미합니다.

```
+--------------+-------------------+--------------------------+
|          Hardware Type (2)       |      Protocol Type (2)   |
+--------------+-------------------+--------------------------+
| H/W Addr Len (1) | Protocol Addr Len (1) |    OP Code  (2)  |
+--------------+-------------------+--------------------------+
|              Sender H/W Addr        (?)                     |
+-------------------------------------------------------------+
|              Sender Protocol Addr   (?)                     |
+-------------------------------------------------------------+
|              Target H/W Addr        (?)                     |
+-------------------------------------------------------------+
|              Target Protocol Addr   (?)                     |
+-------------------------------------------------------------+
```

- 각각의 필드는 아래의 정보를 나타냅니다. ([참고: wiki](https://en.wikipedia.org/wiki/Address_Resolution_Protocol))
  - <mark>HW Type</mark>: 요청된 하드웨어의 종류(망의 형태)를 의미합니다.
  - <mark>Protocol Type</mark>: 다음에 올 프로토콜을 정의합니다.
  - <mark>HW Addr Len</mark>: 하드웨어 장치의 주소 길이를 정의합니다.
  - <mark>Protocol Addr Len</mark>: 다음에 올 프로토콜의 주소 길이를 정의합니다.
  - <mark>OP Code</mark>: `ARP` 패킷의 종류(목적)를 의미합니다.
  - <mark>Sender H/W Addr</mark>: 발신자의 하드웨어 장치 주소를 의미합니다.
  - <mark>Sender Protocol Addr</mark>: 발신자의 프로토콜 주소를 의미합니다.
  - <mark>Target H/W Addr</mark>: 수신자의 하드웨어 장치 주소를 의미합니다.
  - <mark>Target Protocol Addr</mark>: 수신자의 프로토콜 주소를 의미합니다.
<br />
<br />
- `ARP 헤더` 구조를 간략히 정리하면 아래와 같습니다.
  - <mark>H/W</mark> 의 종류와 길이를 정합니다.
  - <mark>Protocol</mark> 의 종류와 길이를 정합니다.
  - <mark>ARP Packet</mark> 의 종류를 정합니다.
  - <mark>발신자</mark>와 <mark>수신자</mark>의 주소값들을 기록합니다.
<br />
<br />
- `ARP 헤더`에 해당하는 byte stream 은 아래와 같습니다.

```
00 01 08 00 06 04 00 01 
00 17 31 45 8e 42 7c 89 
19 fe 00 00 00 00 00 00 
7c 89 19 7a
```
- 이제 각 필드의 바이트 크기만큼 하나 하나 뜯어보겠습니다.
  - <mark>00 01</mark>: [[참고, iana](https://www.iana.org/assignments/arp-parameters/arp-parameters.xhtml#arp-parameters-2)] 를 통해 `Ethernet` 네트워크임을 알 수 있습니다.
  - <mark>08 00</mark>: [[참고, iana](https://www.iana.org/assignments/arp-parameters/arp-parameters.xhtml#arp-parameters-3)] 를 통해 `IPv4` 네트워크를 의미함을 알 수 있습니다.
  - <mark>06</mark>: MAC 주소의 길이인 $6$-byte 를 의미합니다.
  - <mark>04</mark>: IPv4 주소의 길이인 $4$-byte 를 의미합니다.
  - <mark>00 01</mark>: [[참고, iana](https://www.iana.org/assignments/arp-parameters/arp-parameters.xhtml#arp-parameters-1)] 를 통해 `Request` 패킷임을 알 수 있습니다.
  <br/>
  <br/>
  - <mark>00 17 31 45 8e 42</mark> : 발신자의 `MAC 주소`입니다.
  - <mark>7c 89 19 fe</mark>: 발신자의 `IPv4 주소`입니다. (124.137.25.254)
  - <mark>00 00 00 00 00 00</mark>: 수신자의 `MAC 주소`입니다. 아직 모르므로, <mark>00</mark> 값으로 채워넣습니다. 
  - <mark>7c 89 19 7a</mark>: 수신자의 `IPv4 주소`입니다. (124.137.25.122)

