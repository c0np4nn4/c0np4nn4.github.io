+++
title = "NEAR smart contract example 3"
date = "2024-02-27"
+++

<center>
<img src="https://images.pexels.com/photos/271168/pexels-photo-271168.jpeg?auto=compress&cs=tinysrgb&w=600">
</center>

# Donation on NEAR
- NEAR 상에 기부 플랫폼을 하나 만듭니다.
- 전체 코드는 [Github](https://github.com/near-examples/donation-examples/tree/main/contract-rs)에서 살펴볼 수 있습니다.

# Smart Contract code
- 이번에는 두 개의 `Rust` 파일로 컨트랙트를 구성했습니다.
    - `lib.rs`: 실제 컨트랙트가 정의된 파일입니다.
    - `donation.rs`: 기부와 관련된 파일입니다.

## lib.rs
- 우선 컨트랙트의 데이터 구조는 아래와 같습니다.
```rs
#[near_bindgen]
#[derive(BorshDeserialize, BorshSerialize)]
pub struct Contract {
    pub beneficiary: AccountId,
    pub donations: UnorderedMap<AccountId, u128>,
}

impl Default for Contract {
    fn default() -> Self {
        Self {
            beneficiary: "v1.faucet.nonofficial.testnet".parse().unwrap(),
            donations: UnorderedMap::new(b"d"),
        }
    }
}
```

- 기본적인 메서드들은 아래와 같이 정의됩니다.
```rs
#[near_bindgen]
impl Contract {
    #[init]
    #[private] // Public - but only callable by env::current_account_id()
    pub fn init(beneficiary: AccountId) -> Self {
        Self {
            beneficiary,
            donations: UnorderedMap::new(b"d"),
        }
    }

    // Public - beneficiary getter
    pub fn get_beneficiary(&self) -> AccountId {
        self.beneficiary.clone()
    }

    // Public - but only callable by env::current_account_id(). Sets the beneficiary
    #[private]
    pub fn change_beneficiary(&mut self, beneficiary: AccountId) {
        self.beneficiary = beneficiary;
    }
}
```

> `#[init]` 매크로는 말그대로 "기존 값을 지우고 초기화" 하는 메서드임을 명시합니다.
>
> `Default` 트레잇은 컨트랙트가 배포되었을 때 처음 데이터를 지정하는 역할입니다.
>
> 따라서, 값을 초기화하고 싶을 때는 `#[init]` 매크로를 이용할 수 있습니다.

## donate.rs
- 기부에 사용될 구조체는 아래와 같습니다.
```rs
#[derive(BorshDeserialize, BorshSerialize, Serialize)]
#[serde(crate = "near_sdk::serde")]
pub struct Donation {
    pub account_id: AccountId,
    pub total_amount: U128,
}
```

- 컨트랙트 구조체가 아니므로 `#[near_bindgen]`을 붙이지 않습니다.
- 기부와 관련해 컨트랙트에 추가된 메서드들 중 살펴볼만한 것은 아래의 `Promise` 입니다.

```rs
#[near_bindgen]
impl Contract {
    #[payable] // Public - People can attach money
    pub fn donate(&mut self) -> U128 {
        // ...
        // Send the money to the beneficiary
        Promise::new(self.beneficiary.clone()).transfer(to_transfer);

        // ...
    }
```

- 돈을 다뤄야 하므로 `#[payable]` 매크로를 사용합니다.
- 그냥 저런 식으로 돈을 보낼 수 있다는걸 알면 됩니다.


## 빌드 및 배포
### 빌드(Build)
- 빌드는 아래와 같이 입력해 진행합니다.

```bash
#!/bin/sh

echo ">> Building contract"

rustup target add wasm32-unknown-unknown
cargo build --all --target wasm32-unknown-unknown --release%
```

### Testnet Account
- 이번에도 `cargo-near` 로 가계정을 만들어서 진행합니다.

```bash
cargo-near near create-dev-account use-random-account-id autogenerate-new-keypair save-to-legacy-keychain network-config testnet create                                                               ─╯
```
<center>
<img alt="result" src="../../near/gen_testnet_account_3.png" />
</center>

- 이번에는 `old-boy.testnet` 으로 진행해보겠습니다.

### 배포 (Deploy)
```bash
near deploy old-boy.testnet target/wasm32-unknown-unknown/release/contract.wasm
```

## 테스트
- 구현된 기능을 이것 저것 다 해봅니다.

<center>
<img alt="result" src="../../near/donate.png" />
</center>

- `hesitate.testnet` 계정으로 `old-boy.testnet`이 수혜자인 상태에서 1NEAR 만큼 기부했습니다.

<center>
<img alt="result" src="../../near/get_donations.png" />
</center>

- 확인해보면 정말 기록이 되어 있습니다.

<center>
<img alt="result" src="../../near/donation_result_on_nearblocks.png" />
</center>