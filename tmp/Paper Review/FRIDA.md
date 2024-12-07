---
title: FRIDA2024
tags:
  - Cryptography
---
# Abstract
이더리움과 같은 블록체인이 꾸준히 성장함에 따라, 제한된 자원을 사용하는 클라이언트들은 더 이상 전체 데이터를 저장할 수 없게 되었습니다. 
Light node를 사용하면, 블록체인을 사용하면서도 전체적으로 좋은 상태인지 검증하는 작업은 제외한 형태로, 헤더만 저장할 수 있습니다.
하지만 결국 Light node도 블록 content를 필요로 할 수 있기 때문에, 이론상 '가용성'이 있음이 보장될 필요가 있습니다.

Bassam 등에 의해 소개된 [[Data Availability Sampling]]은 Light node가 실제 데이터를 다운로드 하지 않고도 '가용성'을 검사할 수 있도록 하는 프로세스 입니다.
최근 연구 결과에 따르면, Hall-Andersen, Simkin, Wagner는 이에 대한 공식적인 정의(formal definition)와 여러 구조에 대한 분석을 내놓았습니다.
이들의 작업은 *Data Availability Sampling*을 위한 형식적인 토대를 철저히 마련하지만, 분석된 구조들이 '엄청나게 비싸거나', 'trusted setup을 사용하거나', '데이터 크기의 제곱근에 해당하는 다운로드 복잡성을 light node에게 전가한다'는 문제점도 보입니다.

이 논문에서는 trusted setup이 없고, polylogarithmic 오버헤드만을 갖는 *efficient Data Availability Sampling* 스킴을 제시하여, 의미심장한 발전을 제시합니다.
결과로써 이 논문에서는 IOPPs(Interactive Oracle Proofs of Proximity)와의 기발한 연결을 발견했음을 보입니다.
특히, 임의의 IOPP가 추가적인 consistency 조건을 만족할 경우 erasure code commitment로 치환되고 Hall-andersen, Simkin, Wagner의 컴파일러를 활용해 Data Availability Sampling으로까지 치환됨을 증명합니다.
이 새로운 연결은 향후 연구될 IOPP로부터 DAS가 효용을 얻을 수 있도록 합니다.
마지막으로, 널리 사용되는 FRI IOPP가 상기한 조건을 만족함을 보이고 이를 DAS로 치환한 결과가 State-of-the-art 보다 훨씬 좋은 퍼포먼스를 냄을 보입니다.

---

# 1. Introduction
