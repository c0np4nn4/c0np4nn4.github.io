+++
title = "note, network security "
date = "2023-12-04"
+++

# Intrusion Detection System

## Types !!

## Host-based
- centralized manager 가 필요함.

## SNORT
- network-based IDS (NIDS)
- `Passive`: I<u>**D**</u>S 이기 때문에, *prevention*이 아니라 *Passive* 를 의미함.
- usage
    ```bash
    alert <protocol> <from_ip> <from_port> ...
    ```

## Honey Pots !!
- Decoy system
    - 자원이 위조 정보로 구성되어야 함
    - 실제 웹서버처럼 보이긴 해야함
    - 정상적인 사용자의 접근은 막아야 함
- Honey Nets?
- ref: [USENIX, Honey Pot + Web3](https://www.usenix.org/conference/usenixsecurity19/presentation/ferreira) 

## Intrusion Detection Exchange Format
- 각자 알아서 *침입 탐지* 작업을 수행한 후, 정보를 교환하기 위한 통합 표준을 이용함

---

# Password Management
- *Identification* 과 *Verification*은 다름
    - ID + password : verification 가능
- ID: *Not Athentication but Authroization*
- used in DAC(Discretionary Access Control)

## Attacks

> ### Workstation Hijacking
> - 엿보기 당하는 것
> - 자동으로 log-out 되는 등의 방법이 필요함
> 
> ### Exploiting user mistakes
> - 사회공학적 해킹...
> - 유저불량을 고치는게 근본적인 해결법
> - !!논문보다는 특허!!
> 
> ### Offline dictionary attack
> - password file 에 대한 접근
> 
> ### Popular Password Attack

## Unix Password Scheme !!
- ***Salt***
- `Crypt`, `MD5 hash`, `BCrypt`

## Password Selection Strategies
> - User education
> - Computer-generated passwords
>     - hard to guess
> - Reactive password checking
> - Proactive password checking
>     - filter vulnerable password when selecting

---

# The need for firewalls
- IDS가 있는데 방화벽이 필요?
    - control & monitoring 을 위한 *초소*  역할
    - 경계값에서 defense
    - *되는 것* 과 *안되는 것* 으로 Rule을 구분하여 정함
    - penetration 에 대해 immunity 를 가져야 함
- 방화벽의 성질
    - 3가지 ***Design Goal***
    - 4가지 *Technique*
- 방화벽의 한계
    - **IDS**가 아님
        - 방화벽을 우회하는 공격에 대해서는 보안을 제공할 수 없음
        - internet threat 에 대해서는 보호할 수 없음
        - 네트워크를 제대로 설정하지 않았을 때 (firewall 뒤에 AP가 있을 경우..)
