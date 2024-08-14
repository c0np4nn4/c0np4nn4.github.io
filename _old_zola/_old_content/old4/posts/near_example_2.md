+++
title = "NEAR smart contract example 2"
date = "2024-02-26"
+++

<center>
<img src="https://images.pexels.com/photos/606541/pexels-photo-606541.jpeg?auto=compress&cs=tinysrgb&w=800">
</center>

# Guestbook on NEAR
- 방명록을 구현합니다.
- 전체 코드는 [Github](https://github.com/near-examples/guest-book-examples/tree/main/contract-rs)에서 살펴볼 수 있습니다.

# Smart Contract code
- SC 코드를 분석해보면 아래와 같습니다.

## Contract Structure
- 우선 SC에서 사용되는 기본 데이터 구조는 아래와 같습니다.

```rs
#[near_bindgen]
#[derive(BorshDeserialize, BorshSerialize)]
struct GuestBook {
  messages: Vector<PostedMessage>,
}
```
- `messages`라는 이름의 Vector 데이터만 갖습니다.
    - **PostMessage** 로 정의된 *Rust 구조체* 를 원소로 갖습니다.

```rs
#[derive(BorshDeserialize, BorshSerialize, Serialize)]
#[serde(crate = "near_sdk::serde")]
pub struct PostedMessage {
  pub premium: bool, 
  pub sender: AccountId,
  pub text: String
}
```

- 임의의 Rust 구조체를 활용하는 방법으로 보입니다.
- `#[serde(crate = "near_sdk::serde")]`로 NEAR에 특화된 `serde`를 사용하는 것으로 보입니다.

## Methods
- 총 3개의 ***Method*** 를 구현했습니다.
    - `add_message`
    - `get_message`
    - `total_message`

### add_message
- 우선 `add_message` 코드를 살펴봅니다.

```rs
    #[payable]
    pub fn add_message(&mut self, text: String) {
        // If the user attaches more than 0.01N the message is premium
        let premium = env::attached_deposit() >= POINT_ONE;
        let sender = env::predecessor_account_id();

        let message = PostedMessage {
            premium,
            sender,
            text,
        };
        self.messages.push(&message);
    }
```
- `#[payable]`이 있기 때문에 `deposit` 할 수 있다.
- `env::attached_deposit` 은 **Method**를 호출할 때 `deposit`한 NEAR credit을 참조합니다.
- `env::predecessor_account_id` 는 **Method**를 호출한 주체를 의미합니다.
    - ***Cross-Contract call*** 에서 잘 활용할 수 있습니다.
    - 여기서는 `env::signer_account_id` 와 동일합니다.

### get_messages
- 다음으로 `get_messages` 코드를 살펴봅니다.

```rs
    pub fn get_messages(&self, from_index: Option<U64>, limit: Option<U64>) -> Vec<PostedMessage> {
        let from = u64::from(from_index.unwrap_or(U64(0)));
        let limit = u64::from(limit.unwrap_or(U64(10)));

        self.messages
            .iter()
            .skip(from as usize)
            .take(limit as usize)
            .collect()
    }
```

- `from` 부터 시작해서 `limit` 까지의 메세지를 전부 가져오는 코드입니다.
- 주목할 점은 **Method** 호출 시 `near_sdk::json_types::U64` 를 이용한다는 점입니다.
    - 아무래도 NEAR 쪽에 호출을 날릴 때는 이런 numeric type을 사용하는 것 같습니다.

### total_messages
- 메세지 갯수를 반환하는 **Method** 입니다.

```rs
    pub fn total_messages(&self) -> u64 {
        self.messages.len()
    }
```

## 계정 생성
- 아래와 같이 입력해 배포에 사용할 testnet 계정을 생성합니다.

```bash
cargo-near near create-dev-account use-random-account-id autogenerate-new-keypair save-to-legacy-keychain network-config testnet create
```

<center>
<img alt="result" src="../../near/gen_testnet_account_2.png" />
</center>


## 배포 (Deploy)
- 아래와 같이 입력해 배포를 진행합니다.

```bash
#!/bin/sh

# 1. Build
echo ">> Building contract"

rustup target add wasm32-unknown-unknown
cargo build --all --target wasm32-unknown-unknown --release

# 2. Deploy
near deploy romantic-waste.testnet target/wasm32-unknown-unknown/release/guestbook.wasm
```

## 테스트
```bash
near call romantic-waste.testnet add_message '{"text": "First Message"}' --useAccount romantic-waste.testnet --deposit 0.1

near call romantic-waste.testnet add_message '{"text": "Non-premium Message"}' --useAccount romantic-waste.testnet --deposit 0.0

near call romantic-waste.testnet add_message '{"text": "Hello NEAR"}' --useAccount innocent-organization.testnet --deposit 0.1

near view romantic-waste.testnet get_messages '{"from_index": "0", "limit": "10"}' 
```

# Frontend code
- 이번에는 프론트엔드에서의 코드를 살펴보고, `near-cli`로 하듯이 컨트랙트와 소통하는 법을 살펴봅니다.

## index.js
```js
// React
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

// NEAR
import { GuestBook } from './near-interface';
import { Wallet } from './near-wallet';

const CONTRACT_NAME = "romantic-waste.testnet"

// When creating the wallet you can choose to create an access key, so the user
// can skip signing non-payable methods when talking wth the  contract
const wallet = new Wallet({ createAccessKeyFor: CONTRACT_NAME })

// Abstract the logic of interacting with the contract to simplify your flow
const guestBook = new GuestBook({ contractId: CONTRACT_NAME, walletToUse: wallet });

// Setup on page load
window.onload = async () => {
  const isSignedIn = await wallet.startUp()

  ReactDOM.render(
    <App isSignedIn={isSignedIn} guestBook={guestBook} wallet={wallet} />,
    document.getElementById('root')
  );
}
```

- 위 코드대로, SC에 대한 interface 처럼 `guestBook` 이라는 변수를 하나 만들고 **App** 컴포넌트에 보냅니다.

## App.js
```js
// ...
    onSubmit = async (e) => {
    e.preventDefault();

    const { fieldset, message, donation } = e.target.elements;

    fieldset.disabled = true;

    // call
    await guestBook.addMessage(message.value, donation.value)
    const messages = await guestBook.getMessages()

    setMessages(messages);
    message.value = '';
    donation.value = '0';
    fieldset.disabled = false;
    message.focus();
  };

// ...
```

- `App.js` 의 코드 일부분입니다.
- guestBook.addMessage() 로 마치 컨트랙트를 직접 호출하는 것처럼 사용할 수 있습니다.

## near-interface.js
- `guestBook` 변수에 저장된 GuestBook은 `near-interface.js`에서 정의하고 있습니다.

```js
/* Talking with a contract often involves transforming data, we recommend you to encapsulate that logic into a class */

import { utils } from 'near-api-js';

export class GuestBook {

  constructor({ contractId, walletToUse }) {
    this.contractId = contractId;
    this.wallet = walletToUse
  }

  async getMessages() {
    const messages = await this.wallet.viewMethod({ contractId: this.contractId, method: "get_messages" })
    console.log(messages)
    return messages
  }

  async addMessage(message, donation) {
    const deposit = utils.format.parseNearAmount(donation);
    return await this.wallet.callMethod({ contractId: this.contractId, method: "add_message", args: { text: message }, deposit });
  }

}
```

- `addMessage` 메서드를 보면 `this.wallet.callMethod` 를 이용해 컨트랙트 내 함수를 호출하고 있음을 확인할 수 있습니다.
- 즉, `wallet`의 존재가 중요합니다.

## near-wallet.js
- `wallet`은 `near-wallet.js`에 정의되어 있습니다.

## 정리
- 아래와 같은 흐름으로 이해할 수 있습니다.
<center>
<img alt="result" src="../../near/frontend_guestbook.png" width="400px"/>
</center>