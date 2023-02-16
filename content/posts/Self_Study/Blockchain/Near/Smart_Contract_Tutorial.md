+++
title = "⛓️ [Near] Write and Deploy a Smart Contract in Rust"
date = 2023-01-02
+++

---

> 본 포스팅은 [*[Figment.io](https://learn.figment.io/tutorials/write-and-deploy-a-smart-contract-on-near)*] 을 참고하여 작성됨 

---

# 목표

- `Rust` 로 `Smart Contract`를 작성한다.

- `Smart Contract`를 `Near` on-chain 에 배포(`Deploy`)한다.

- `near-cli`를 이용하여 배포된 contract와 Interact 해본다. 

> *[[Rust](https://www.rust-lang.org/tools/install)]*,
*[[near-cli](https://www.npmjs.com/package/near-cli)]*,
*[[near testnet account](https://wallet.testnet.near.org)]*
가 필요하다. 

---

# Smart Contract in Rust

### Setup

- `Near`는 wasm 형태로 된 smart contract를 다룬다.

- 개발자는 `Near`에서 제공하는 **SDK**를 이용해서 `Rust` 언어를 `Near` 상에서 다룰 형태의 `Wasm`으로 컴파일할 수 있다.

- 우선, Rust toolchain에 wasm target을 추가해야 한다.

  ```sh
  rustup target add wasm32-unknown-unknown
  ```

- 이후 `Smart Contract`를 작성할 `Rust` lib crate 를 생성한다.

  ```sh
  cargo init --lib my_contract
  ```

- `config.toml` 파일을 열어서 내용을 아래와 같이 수정한다.

  ```toml
  [package]
  name = "my_contract"
  version = "0.1.0"
  edition = "2021"

  [lib]
  crate-type = ["cdylib", "rlib"]

  [dependencies]
  near-sdk = "3.1.0"

  [profile.release]
  codegen-units = 1
  opt-level = "z"
  lto = true
  debug = false
  panic = "abort"
  overflow-checks = true
  ```

- [lib] 의 내용은 [[이곳](https://doc.rust-lang.org/reference/linkage.html)] 을 참고하면 된다.

- [dependencies] 에서 `near-sdk` crate 를 사용하고 있음을 확인할 수 있다.

---

### Writing the Contract

- 본 실습에서는 Contract가 갖고 있는 [*[Storage](https://docs.near.org/concepts/storage/storage-staking)*]를 이용하여 `CRUD` 기능을 구현해본다.

- `Contract`는 크게 보면 아래의 구조를 갖고 있다.

  ```rs
  // 1. Main Struct

  // 2. Default Implementation

  // 3. Core Logic

  // 4. Tests
  ```

- `1. Main Struct` 에서는 `Contract`에서 사용될 Struct를 정의한다.

- `2. Default Impl` 에서는 Struct에 대한 Default trait을 정의한다.

- `3. Core Logic` 에서는 `Contract`에서 사용될 핵심 기능들을 정의한다. 

- `4. Tests` 에서는 각종 Test Case를 작성하여 검사한다. 

- Contract 작성을 위한 기본 세팅은 아래와 같다.

  ```rs
  use near_sdk::borsh::{self, BorshDeserialize, BorshSerialize};
  use near_sdk::{env, near_bindgen};
  use near_sdk::collections::UnorderedMap;

  near_sdk::setup_alloc!();
  ```
- `setup_alloc!()` 매크로는 내부적으로 `WebAssembly`를 위해 설계된 `wee_alloc`를 이용한다.

- `Rust` 코드는 이 allocator를 이용해서 runtime 때 필요한 메모리를 획득할 수 있다.

#### 1. Main Struct
- `Rust`로 작성된 대부분의 `Near` 상의 Contract들은 하나의 Struct와 이에 대한 Impl이 구현된 패턴을 갖는다.

- 아래 코드는 본 Contract에서 사용될 struct 이다.

  ```rs
  #[near_bindgen]
  #[derive(BorshDeserialize, BorshSerialize)]
  pub struct Storage {
      pairs: UnorderedMap<String, String>,
  }
  ```

- `Storage` 는 `near_sdk`에서 제공하는 자료구조 중 `unordered_map`타입의 `pairs`라는 필드를 갖는다.

- `#[near_bindgen]` 속성을 통해, `Near`와 상호작용할 수 있는 형태의 Wasm으로 컴파일 된다.

- `#[derive(BorshDeserialize, BorshSerialize)]` 속성은 `Storage`로 하여금 내부적으로 Serialize, Deserialize가 가능하도록 해준다.

#### 2. Default Impl 
- Contract에서 사용할 struct에 대해 직접 Default 를 구현해준다.

  ```rs
  impl Default for Storage {
    fn default() -> Self {
        Self {
            pairs: UnorderedMap::new(b"default".to_vec())
        }
    }
  }
  ```

- `UnorderedMap::new(prefix: S) where S: IntoStorageKey`는 함수 인자로 `prefix`를 받아 identifier로 사용한다.

#### 3. Core Logic 
- 이제 Contract의 핵심 기능을 구현한다.

  ```rs
  #[near_bindgen]
  impl Storage {
    pub fn create_update(&mut self, k: String, v: String) {
        env::log(b"created or updated");
        self.pairs.insert(&k, &v);
    }

    pub fn read(&self, k: String) -> Option<String> {
        env::log(b"read");
        return self.pairs.get(&k);
    }

    pub fn delete(&mut self, k: String) {
        env::log(b"delete");
        self.pairs.remove(&k);
    }
  }
  ```

- `pub` 키워드를 붙여야 Contract가 배포된 후에 외부에서도 호출이 가능하다.

- 또한, 함수 인자로 사용한 변수명(`k`, `v`)은 외부에서 호출할 때도 동일하게 사용하게 된다.

- `env::log()` 함수를 이용하여 logging message 를 띄운다.

#### 4. Tests 

- 마지막으로, 구현한 Contract에 대한 Test Case를 작성할 수 있다.

- 가상환경에서 Contract를 실행시켜보기 위해 `near_sdk`는 `testing_env!()` 매크로와 `VMContext` struct 등을 제공한다.

  ```rs
  #[cfg(not(target_arch = "wasm32"))]
  #[cfg(test)]
  mod tests {
    use super::*;
    use near_sdk::MockedBlockchain;
    use near_sdk::{testing_env, VMContext};

    fn get_context(input: Vec<u8>, is_view: bool) -> VMContext {
        VMContext {
            current_account_id: "alice_near".to_string(),
            signer_account_id: "bob_near".to_string(),
            signer_account_pk: vec![0, 1, 2],
            predecessor_account_id: "carol_near".to_string(),
            input,
            block_index: 0,
            block_timestamp: 0,
            account_balance: 0,
            account_locked_balance: 0,
            storage_usage: 0,
            attached_deposit: 0,
            prepaid_gas: 10u64.pow(18),
            random_seed: vec![0, 1, 2],
            is_view,
            output_data_receivers: vec![],
            epoch_height: 0,
        }
    }

    // Test 1

    // Test 2
  }
  ```

- `creating&update and read`를 테스트하는 코드는 아래와 같다.

  ```rs
  #[test]
  fn create_read_pair() {
      let context = get_context(vec![], false);
      testing_env!(context);
      let mut contract = KeyValue::default();
      contract.create_update("first_key".to_string(), "hello".to_string());
      assert_eq!(
          "hello".to_string(),
          contract.read("first_key".to_string()).unwrap()
      );
  }
  ```

- `testing_env!()` 매크로를 이용하여 가상환경을 만든다.

- `contract` 변수를 만든 다음, 메소드를 호출하여 함수 동작을 테스트한다.

- 여기까지 모든 코드를 `lib.rs`에 작성했다면, 아래 명령으로 contract를 테스트 해볼 수 있다.

  ```sh
  cargo test -- --nocapture
  ```

---

### Compiling the Contract
- 이제 `Rust`로 작성된 Contract를 `Wasm` 으로 컴파일해야 한다.

- 아래 명령을 입력하여 진행한다.

  ```rs
  env 'RUSTFLAGS=-C link-arg=-s' 
  cargo build --target wasm32-unknown-unknown --release
  ```

- 컴파일이 끝났다면, `target/wasm32-unknown-unknown/release/` 아래에 `wasm` 파일이 생성된 것을 확인할 수 있다.

- 바로 이 `wasm` 파일이 `Near` on-chain에 배포될 Smart Contract다.

---

### Deploying the Contract
- `near-cli`를 이용하면 간편하게 Contract 를 Deploy하고 Interact할 수 있다.

- 큰 순서는 `login` -> `deploy` -> `call` 이다.

#### 1. login

- 우선 아래의 명령을 입력하여, `near-cli`로 wallet account에 로그인 해야한다.

  ```sh
  near login
  ```

- 이를 위해서는 `Near Testnet Account`가 필요한데, 이는 [*[near testnet wallet](https://wallet.testnet.near.org)*]를 통해 생성할 수 있다.

#### 2. deploy

- Smart Contract를 위한 계정을 하나 생성해야 한다.

- `Near`는 `Storage Staking` 이라는 메커니즘을 이용하는데, 이는 메모리 사용을 위해 일정량의 토큰을 예치하고 있어야 함을 의미한다.

- 따라서, 아래와 같이 명령어를 입력하여 내 계정으로부터 일정량의 토큰을 예치하게 된다.

  ```sh
  near create-account CONTRACT_NAME.ACCOUNT_ID --masterAccount ACCOUNT_ID --initialBalance 10
  ```

- 그리고 생성된 계정으로 Contract를 deploy 하면 된다.

  ```sh
  near deploy --wasmFile target/wasm32-unknown-unknown/release/my_contract.wasm --accountId CONTRACT_ID
  ```

- 이 때, `CONTRACT_ID`는 `CONTRACT_NAME.ACCOUNT_ID` 이다.

> - 만약, `CONTRACT_NAME`으로 `dodo`를 사용하고, 내 `ACCOUNT_ID`가 `1234.testnet`이라면 아래와 같이 입력하면 된다.
> 
>   ```sh
>   near create-account dodo.1234.testnet --masterAccount 1234.testnet --initialBalance 10
>   ```
> 
>   ```sh
>   near deploy --wasmFile target/wasm32-unknown-unknown/release/my_contract.wasm --accountId dodo.1234.testnet
>   ```

- 경우에 따라서는, `initialBalance`의 양이 부족하여 deploy가 실패할 수 있다.

- 그런 경우에는 아래 명령을 통해 토큰을 조금 더 예치하면 된다.

  ```sh
  near send ACCOUNT_ID CONTRACT_ID AMOUNT
  ```

> - 위의 예시에서 10 만큼을 더 보내고자 한다면 아래와 같다.
>   ```sh
>   near send 1234.testnet dodo.1234.testnet 10
>   ```

#### 3. Call

- 이제 배포된 Contract 를 호출해본다.

- 순서는 `create`, `read`, `delete` 로 진행된다.

> - 우선 {"apple: "red", "sky": "blue"} 의 Key-Value 를 입력한다.

  ```sh
  near call CONTRACT_ID create_update '{"k": "apple", "v": "red"}' --accountId ACCOUNT_ID
  near call CONTRACT_ID create_update '{"k": "sky", "v": "blue"}' --accountId ACCOUNT_ID
  ```

> - 다음으로, 입력된 값을 확인한다.

  ```sh
  near view CONTRACT_ID read '{"k": "apple"}' --accountId ACCOUNT_ID
  near view CONTRACT_ID read '{"k": "sky"}' --accountId ACCOUNT_ID
  ```

- 내부 상태를 변경하지 않는 함수를 호출할 때는 `view`를 사용할 수 있다.
- `view`를 사용하면, fee를 낼 필요도 없고 응답도 거의 즉각적으로 받을 수 있다.

> - 마지막으로, 저장된 값들을 제거하는 명령은 아래와 같다.

  ```sh
  near call CONTRACT_ID delete '{"k": "apple"}' --accountId ACCOUNT_ID
  near call CONTRACT_ID delete '{"k": "sky"}' --accountId ACCOUNT_ID
  ```

# 정리
- 본 포스팅에서는 `Rust` 언어로 `Near Smart Contract`를 작성하고, 
이를 `Rust toolchain`과 `near_sdk`를 이용하여 `Wasm`으로 컴파일한 뒤, 
`near-cli`를 이용해 on-chain에 deploy하고 interact하는 것을 실습해보았다.
