+++
title = "CB33925, 4, Model for Network Security"
date = "2023-09-11"
+++

# Model for Network Security

<center>
    <figure>
        <img alt="system types" src="../../Class/CB33925_NetworkSecurity/4_1.png" />
        <figcaption>
        그림 1. Model for Network Security
        </figcaption>
    </figure>
</center>

그림 1 은 `Sender` 내부에서 암호학적으로 메세지를 변조한 후 `Receiver` 를 향해 전송하는 과정입니다.

`Trusted Third Party`를 여기서는 키 분배의 역할로 표현했지만, 이 밖에도 여러 신뢰가 필요한 경우 활용될 수 있습니다.
물론, `TTP`가 존재하지 않는 시스템이 가장 이상적인 시스템이라 할 수 있습니다.


# Network Access Security Model

<center>
    <figure>
        <img alt="system types" src="../../Class/CB33925_NetworkSecurity/4_2.png" />
        <figcaption>
        그림 2. Network Access Security Model
        </figcaption>
    </figure>
</center>

`Gatekeeper`는 접근 허용여부를 판별합니다. **Firewall** 이라고 이해하면 됩니다.

# Unwanted Access
프로그램은 두 종류의 보안 위협을 가질 수 있습니다.

우선, `Information Access`는 인가되지 않은 사용자가 정보를 탈취하는 것을 의미합니다.
다음으로, `Service Threat`은 취약점을 이용해 의도하지 않은 동작을 일으킬 수 있음을 의미합니다.
