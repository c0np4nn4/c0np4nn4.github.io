---
title: Orion2022
tags:
  - ZKP
  - Cryptography
aliases:
  - Orion
  - Orion paper
---
# Abstract
영지식 증명은 다양한 실제 응용이 가능한 강력한 암호학적 프리미티브입니다. 그러나 기존의 영지식 증명 스킴들은 증명 크기가 간결한 대신, 증명 생성 시간에서 큰 오버헤드를 겪고 있어, 실용적인 효율성과 확장성에 한계가 있었습니다. 이러한 문제를 해결하기 위해 본 논문에서는 ***Orion***이라는 새로운 영지식 증명 시스템을 제안했습니다.

***Orion***은 필드 연산과 해시 함수에 대해 $O(N)$의 증명 생성 시간을 달성하고, 증명 크기는 $O(\log^2 N)$으로 유지하면서도, 구체적인 효율성을 자랑합니다. 논문에서 구현된 결과에 따르면, **220개의 곱셈 게이트**로 이루어진 회로에 대해 **3.09초의 증명 생성 시간**과 **1.5MB의 증명 크기**를 보여주었으며, 이는 현존하는 모든 간결한 증명 시스템 중에서 가장 빠른 시간입니다. 또한 증명 크기는 최근 제안된 다른 스킴에 비해 **한 자릿수 작음**을 확인했습니다.

***Orion***의 이러한 효율성은 두 가지 주요 기술 덕분입니다. 첫째, 무작위 [[bipartite graph|이분 그래프]]가 손실 없는 확장 그래프인지 여부를 검사하는 **새로운 알고리즘**을 제안하여, 손실 없는 확장 그래프를 매우 높은 확률로 샘플링할 수 있게 했습니다. 이 기술은 기존 영지식 증명 스킴의 효율성과 보안성을 크게 향상시킬 수 있습니다. 둘째, **code switching**이라는 효율적인 증명 구성 스킴을 개발하여, 증명 크기를 계산 크기에 대해 **제곱근에서 polylogarithmic**으로 감소시켰습니다. 이 스킴은 증명 생성 시간에 아주 작은 오버헤드만을 발생시키는 효율성을 보장합니다.

---

# 1. Introduction
**영지식 증명** (ZKP)은 <mark style="background: #ADCCFFA6;">Prover</mark>로 하여금 명제에 관한 어떠한 추가 정보도 제공하지 않으면서 <mark style="background: #D2B3FFA6;">Verifier</mark>에게 자신의 명제가 유효함을 납득시킬 수 있도록 합니다. 
Goldwasser, Micali, Rackoff (GMR89)의 세미나 논문에서 처음 소개된 이후, **ZKP**는 순수한 이론적 흥미로부터 구체적으로 효율적인 암호 프리미티브로까지 진화해오며 실용적인 실제 응용들을 이끌어 왔습니다. 
대부분은 블록체인과 암호화폐 업계에서 프라이버시(Zcash)와 확장성의 향상(zkRollup)을 얻기 위해 사용되어 왔습니다. 
최근에는 *zero-knowledge machine learning*, *zero-knowledge program analysis*, *zero-knowledge middle box*와 같은 형태로도 응용되고 있습니다.

**ZKP**에 관해 세 가지 주요 효율성 측정 지표가 있습니다.
- **Prover Time**: <mark style="background: #ADCCFFA6;">Prover</mark>가 $\pi \; (proof)$ 를 생성하는데 드는 오버헤드
- **Proof Size**: <mark style="background: #ADCCFFA6;">Prover</mark>와 <mark style="background: #D2B3FFA6;">Verifier</mark> 간의 전체 소통량
- **Verifier Time**: <mark style="background: #D2B3FFA6;">Verifier</mark>가 $\pi$ 를 검증하는데 드는 시간

근래의 많은 발전들에도 불구하고 **ZKP**의 효율성은 다양한 응용에 활용되기에 충분히 좋지는 않습니다. 
특히, **Prover Time**은 **ZKP** 스킴이 큰 명제(*large statement*)으로 확장되는 것을 막고 있는 주요 병목입니다.

**ZKP**에서 명제(statement)는 $N$개의 게이트로 이루어진 산술회로(arithmetic circuit)로 표현됩니다.
**명제**를 **증명**하기 위해서 현존하는 '간결한(succinct)' 크기의 증명을 갖는 **ZKP** 스킴은 다음 두 가지 방식 중 하나를 사용합니다.
- <mark style="background: #FFB86CA6;">고속 푸리에 변환(FFT)</mark>
	- 리드-솔로몬 코드 인코딩(Reed-Solomon Code Encoding) 또는 다항식 보간법(polynomial interpolation)을 수행하기 위해 사용됩니다.
	- Field addition, multiplication 연산에 대해 $O(N \log N)$ 시간 복잡도를 갖습니다.
- <mark style="background: #FFB86CA6;">다중 스칼라 지수화(Multi-Scalar Exponentiation)</mark>
	- 이 방법은 이산 로그 가정(discrete-logarithm assumption)이나 이중 선형 사상(bilinear map)에 기반하며, 크기 $O(N)$의 벡터를 처리합니다.
	- Field Multiplication 연산에 대해 $O(N \log | \mathbb{F} |)$ 의 시간 복잡도를 가지며, 이 때 $| \mathbb{F} |$ 는 Finite Field의 크기를 의미합니다.
	- Pippenger 알고리즘 (Pip)을 사용하면 시간 복잡도를 $O(N \log | \mathbb{F} | / \log N)$으로 개선할 수는 있지만, 보안성을 보장하기 위해 $\log | \mathbb{F} | = \omega (\log N)$ 을 만족해야 합니다. 따라서, 최소한 $\log N$ 보다는 빠르게 증가한다는 의미이므로 충분히 큰 $| \mathbb{F} |$ 가 필요함을 의미합니다. 즉, 여전히 **초선형**(super-scalar) 입니다.
이 두 연산이 결국 **prover time**에서 이론적으로나 실제로나 주된 비용을 차지하게 됩니다.

문헌에서 유일한 예외는 [BCG+17], [BCG20], [BCL22], [GLS+]에서 제시된 스킴들입니다.

- Prover Time
	- **[BCG+17]**: 선형 시간으로 인코딩할 수 있는 오류 수정 코드(linear-time encodable error-correcting code)를 사용하여, 
	  필드 연산에서 $O(N)$의 시간을 달성 (**ZKP**에서 최초로 linear time을 달성).
	- **[BCG20]**: [BCG+17]과 동일한 $O(N)$ prover time 유지.
	- **[BCL22]**: [BCG20]과 동일한 $O(N)$ prover time 유지.
	- **[GLS+]**: 랜덤화된 구성 방식을 통해 선형 시간 인코딩이 가능한 코드를 활용하여 $O(N)$의 prover time을 유지.

- Proof Size
	- **[BCG+17]**: $O(\sqrt{N})$ (linear-time 인코딩을 통해 기존 대비 효율성 향상)
	- **[BCG20]**: 텐서 코드를 통해 임의의 상수 $c$에 대해 $O(N^{1/c})$로 개선
	- **[BCL22]**: PCP(Probabilistic Checkable Proof)와의 generic proof composition을 통해 $\text{polylog}(N)$까지 향상
	- **[GLS+]**: 랜덤화된 구성 방식을 통해 선형 시간 인코딩을 사용했으나, $O(\sqrt{N})$에 머무름
	
- Security
	- **[GLS+]**: 보안 보장(soundness error)이 회로 크기에 대해 역다항식(inverse polynomial) 수준으로, 이는 충분히 작지 않음

따라서, **ZKP** 스킴에 관한 아래의 질문이 여전히 남아있습니다.

> **Prover Time**이 $O(N)$, **Proof Size**가 $\text{polylog}(N)$인 구체적이고 효율적인 **ZKP** 스킴을 구축할 수 있는가?

---
## 1.1 Our Contribution

| Scheme                                                                 | Prover time | Proof size          | Verifier time | Soundness error                          | Concrete efficiency |
| ---------------------------------------------------------------------- | ----------- | ------------------- | ------------- | ---------------------------------------- | ------------------- |
| [BCG+17]                                                               | $O(N)$      | $O(\sqrt{N})$       | $O(N)$        | $\text{negl}(N)$                         | ✗                   |
| [BCG20]                                                                | $O(N)$      | $O(N^{1/c})$        | $O(N)$        | $\text{negl}(N)$                         | ✗                   |
| [BCL22]                                                                | $O(N)$      | $\text{polylog}(N)$ | $O(N)$        | $\text{negl}(N)$                         | ✗                   |
| [GLS+]                                                                 | $O(N)$      | $O(\sqrt{N})$       | $O(N)$        | $O\left(\frac{1}{\text{poly}(N)}\right)$ | ✔                   |
| ***<mark style="background: #FF5582A6;"> &nbsp; Orion &nbsp;</mark>*** | $O(N)$      | $O(\log^2 N)$       | $O(N)$        | $\text{negl}(N)$                         | ✔                   |

본 논문에서는 위 질문에 대한 답으로 새로운 **ZKP** 스킴을 제안합니다. 
구체적으로 본 논문에서 제시한 스킴은 아래의 특징을 갖습니다.

- 