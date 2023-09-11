+++
title = "CB33925, 3, security services"
date = "2023-09-11"
+++

# Security Services
`Security Service`란 네트워크를 비롯한 정보보안 **공격(Attack)** 에 대해 **대비책(Counter-measure)** 을 찾고,
이를 **서비스**로 제공하는 형태를 의미합니다.

`X.800`와 `RFC 4949`에 정의된 서비스들이 잘 알려져 있습니다.

`X.800`에 정의된 서비스들은 **Open System** 상에서
<u>데이터 전송</u> 또는 <u>시스템 자체</u>에 적절한 수준의 보안을 제공하도록 보장하는 서비스로,
**프로토콜 단(protocol layer)** 에서 제공됩니다.

`RFC 4949`는 **시스템 자원(system resources)** 를에 특정 형태의 보호를 제공하기 위해 제공되는
<u>일련의 작업</u> 또는 <u>통신 서비스</u>를 제공합니다.

<center>
    <figure>
        <img alt="system types" src="../../Class/CB33925_NetworkSecurity/3_1.png" />
        <figcaption>
        그림 1. Network System Types
        </figcaption>
    </figure>
</center>

---

# X.800 Service Categories
`X.800`에서 제공하는 서비스들의 범주는 아래와 같습니다.
> - Authentication
> - Access Control
> - Data Confidentiality
> - Data Integrity
> - Non-repudiation

---

## Authentication
*Proof of Identity* 와 맞닿는 개념으로, 인가된 사용자가 통신에 참여하는지를 보장하는 서비스입니다.
`X.800`에는 아래 **Data**와 **Peer** 각각에 대한 *authentication service* 가 존재합니다.

### Data Origin Authentication
단일 **message**에 대하여, <u>수신자가 예상(기대)</u>하는 송신자로부터 메세지를 받을 수 있도록 보장하는 서비스</u>입니다.

### Peer Entity Authentication
상호작용(ongoing interaction)에 관하여,
통신 중인 두 참여자의 신원을 보장하고 제 3자의 *masquerade* 와 같은 행위를 방지하는 서비스입니다.

자명한 이야기지만, ***Replay Attack*** 은 `Data Origin Authentication`으로 막을 수 없습니다.
왜냐하면, *단일 메세지* 자체에 적힌 정보는 이상이 없기 때문입니다.
따라서, `Peer Entity Authentication`으로 유효한 데이터 전송인지를 판별하여 이를 방지할 수 있습니다.

---

## Access Control
통신 링크(communication link)를 통한 호스트 시스템의 접근을 제한하고 통제하는 서비스입니다.
이를 위해, 시스템에 접근하는 주체들은 ***우선적으로 Identification (Authentication)을 통해 신원을 증명*** 해야합니다.

---

## Data Confidentiality
우선, `Passive Attack`에 대한 방지로 두 통신 주체 사이에서 **전송되는 데이터를 보호하는 형태**의 서비스가 있습니다.
또한, 통신 간에 발생하는 트래픽에 대해 공격자가 **Metadata 정보를 관측할 수 없게 하는 형태**의 서비스도 있습니다.

---

## Data Integrity
### Connection-oriented Integrity Service
`Active Attack`에 대비할 수 있도록, 메세지 전송 흐름(Stream)에 관해 *복제 되지 않았고, 조작(modification, insertion, reordering)되지 않았으며, 재전송(replay) 등이 일어나지 않았음* 을 보증하는 서비스입니다.

### Connectionless Integrity Service
각각의 메세지에 대해 조작(modification)이 일어났는지에 대해서만 보증하는 서비스입니다.
(큰 맥락(stream) 등은 고려하지 않습니다)

---

## Non-repudiation
각각의 통신 주체들이 서로에 대해 *진짜로 전송/수신 을 하였는가* 를 보증하는 서비스입니다.

---

## Availability Service
서비스를 언제든 필요할 때 이용할 수 있는지에 관한 개념입니다.
이러한 `Availability` 를 보증하는 서비스는 **DoS** 공격에 대한 방지나 복구도 포함합니다.
