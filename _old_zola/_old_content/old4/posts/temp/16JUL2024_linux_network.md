+++
title = "Linux network (1)"
date = "2024-07-16"
+++

# 1장. 도입

1장에서는 네트워킹 코드에서 자주 볼 수 있는 공통적인 프로그래밍 패턴과 트릭을 소개합니다.

## 기본 용어
- 8비트 묶음은 네트워킹 용어로는 옥탯(`octets`)이라 사용되지만, 이 책에서는 커널 동작을 설명하는 것에 중점을 두기 때문에 바이트(`bytes`)로 표기합니다.
- 벡터(`vector`), 배열(`array`)는 혼용합니다.
- **TCP/IP** 계층에 대해 `L2`, `L3` 등의 약어를 사용합니다.

주요 약어들을 표로 정리하면 아래와 같습니다.

|약어|의미|
|:-:|:--|
|L2| 링크 계층 (예: 이더넷)|
|L3| 네트워크 계층 (예: IP)|
|L4| 전송 계층 (예: UDP/TCP/ICMP)|
|BH| 후반부 (Bottom Half)|
|IRQ| 인터럽트|
|RX| 수신|
|TX| 송신|

## 공통 코딩 패턴
네트워크 스택의 기능들도 다른 커널 스택과 같이 커널의 일부 기능일 뿐입니다.
따라서, 네트워크 스택의 기능들도 <u>적절하고 공평하게 메모리, CPU, 그리고 다른 공유 자원들을 사용</u>해야 합니다.
또한, 네트워크 스택 기능들도 대부분의 커널 기능들과 같이 다른 커널 컴포넌트와 상호작용하게 만들어졌습니다.
따라서, 네트워킹 기능들도 최대한 비슷한 기능들을 구현하기 위해 비슷한 방식을 따르게 됩니다. (매번 바퀴를 새로 발명할 필요는 없습니다)

주요 기능의 집합을 만들기 위한 파일들의 모음을 `서브시스템`이라는 단어로 표현합니다.
이는 동일한 사람에 의해 관리되고, 동시에 변화되는 코드를 의미합니다.

## 메모리 캐시
커널은 메모리 블록을 할당하고 해제하기 위해 *kmalloc* 함수와 *kfree* 함수를 사용합니다.
이 두 함수에 사용되는 구문은 자매 함수에 해당하는 '*malloc*'이나 '*free*'와 거의 유사합니다.

커널 컴포넌트가 동일한 자료구조의 여러 인스턴스를 할당하는 것은 흔한 일입니다.
할당과 해제가 자주 일어날 것으로 예상될 때 관련 커널 컴포넌트의 **초기화 루틴**은 대개 할당을 위한 특별한 `메모리 캐시`를 할당합니다.
메모리 블록이 해제되면 초기에 할당받았던 해당 캐시 공간으로 반환되게 됩니다.

커널이 네트워크 데이터를 전용 `메모리 캐시`에 저장하는 몇 가지 예는 다음과 같습니다.

> - 소켓 버퍼 디스크립터(socket buffer descriptors)
>     - 이 캐시는 net/core/sk_buff.c 파일의 *skb_init* 에 의해 할당되고 *sk_buff* 버퍼 디스크립터를 할당하는 데 사용됩니다.
>     - *sk_buff* 구조체는 네트워크 서브시스템 중에서 할당과 반환을 가장 많이 사용하는 변수 중 하나로 볼 수 있습니다.
> 
> - 인접 프로토콜(Neighboring protocol) 매핑
>     - 각각의 인접 프로토콜은 L3-L2 간 주소 매핑 정보를 저장하는 데이터 스트럭처 할당을 위해 메모리 캐시를 사용합니다.
> 
> - 라우팅 테이블(Routing tables)
>     - 라우팅 코드는 라우트를 정의하기 위해 2개의 데이터 스트럭처를 위한 메모리 캐시를 사용합니다.

커널에서 `메모리 캐시`를 다루는 주요 함수는 다음과 같습니다.

> - ***kmem_cache_create***, ***kmem_cache_destroy***
>     - 캐시를 생성하고 소멸시킵니다.
> 
> - ***kmem_cache_alloc***, ***kmem_cache_free***
>     - 캐시를 할당하거나 해제합니다.
>     - 대개 상위 레벨에서 할당과 해제를 관리하는 래퍼(*wrapper*)에 의해 호출됩니다.
>     - 예를 들면 *sk_buffer* 인스턴스 중 하나를 *kfree_skb*로 해제할 경우 해당 인스턴스가 참조하는 버퍼들을 다 해지하고, 또 관련 있는 서브시스템에서 필요한 청소작업이 다 진행된 다음에야 *kmem_cache_free*가 호출됩니다.

설정되어 있거나 또는 현재의 캐시에서 생성할 수 있는 인스턴스의 개수는 '*kmem_cache_alloc*'을 사용하는 wrapper 에 의해 적용됩니다.
어떤 경우에는 */proc*의 매개변수로 정의하기도 합니다.

## 캐시와 해시 테이블
캐시를 사용해 성능을 향상시키는 것은 아주 일반적인 방법입니다.
네트워크 코드에서도 라우팅 테이블 캐시 등을 위해 L3-L2 매핑용 캐시를 사용합니다.

`캐시 조회용 루틴`에는 캐시가 없을 때 새로운 정보를 캐시에 추가할 것인가에 대해 매개 변수를 입력하게끔 돼 있습니다.
다른 조회용 루틴들은 존재하지 않는 정보가 있을 때 항상 추가하게만 돼 있습니다.

캐시는 보통 **해시 테이블**을 사용해 개발합니다.
커널에는 단방향 리스트나 양방향 리스트와 같은 데이터 타입이 있기 때문에 간단하게 해시 테이블을 만들 수 있습니다.

동일한 해시 값을 갖는 입력 값을 처리하는 표준 방법은 그 값을 리스트에 넣는 것입니다.
리스트를 검색하는 것은 해시 키를 사용하는 것에 비해 더 오래 걸리기 때문에 같은 해시값이 생성되는 정도를 최소화 하는 것이 중요합니다.

해시 테이블 검색 시간이 **서브시스템**의 소유자에게 큰 영향을 주는 변수가 될 경우 해시 크기를 늘려 충돌 리스트의 평균 크기는 줄이고 검색 시간은 개선할 수도 있습니다.

인접 계층과 같은 서브시스템에서 캐시 버킷에 넣는 요소들을 분산할 때 사용하는 키에 무작위 컴포넌트를 추가하는 서브시스템도 있습니다.
이렇게 하면 해시 테이블의 요소를 하나의 버킷에만 넣게 할 때보다  DoS 공격에 의한 손상을 줄여줍니다.