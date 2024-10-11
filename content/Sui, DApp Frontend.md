---
tags:
  - SUI
  - 개발
  - Web3
---
# 1. Example
```bash
yarn create @mysten/dapp --template react-client-dapp

# <ENTER>

cd my-first-sui-dapp

yarn

yarn dev
```

![[Pasted image 20241011024857.png]]

지갑을 연결해볼 수 있는 예제 사이트가 나옵니다.

---

# 2. 코드 분석
## 2-1. Provider
`main.tsx` 에는 세 종류의 `Provider`가 존재합니다.

```tsx
// main.tsx
ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <Theme appearance="dark">
      <QueryClientProvider client={queryClient}>
        <SuiClientProvider networks={networkConfig} defaultNetwork="testnet">
          <WalletProvider autoConnect>
            <App />
          </WalletProvider>
        </SuiClientProvider>
      </QueryClientProvider>
    </Theme>
  </React.StrictMode>,
);
```

- QueryClientProvider
- SuiClientProvider
- WalletProvider

## 2-2. Connect to Wallet
`App.tsx` 에서 `ConnectButton` 컴포넌트를 활용하여 지갑을 연결할 수 있도록 합니다.

```tsx
// App.tsx
import { ConnectButton } from '@mysten/dapp-kit';

function App() {
	return (
		<div className="App">
			<header className="App-header">
				<ConnectButton />
			</header>
		</div>
	);
}
```

## 2-3. Account 정보 활용
지갑이 연결 되었으므로, `userCurrentAccount()` Hook을 아래와 같이 활용합니다.

```tsx
// WalletStatus.tsx
import { useCurrentAccount } from "@mysten/dapp-kit";
import { Container, Flex, Heading, Text } from "@radix-ui/themes";
import { OwnedObjects } from "./OwnedObjects";

export function WalletStatus() {
  const account = useCurrentAccount();

  return (
    <Container my="2">
      <Heading mb="2">Wallet Status</Heading>

      {account ? (
        <Flex direction="column">
          <Text>Wallet connected</Text>
          <Text>Address: {account.address}</Text>
        </Flex>
      ) : (
        <Text>Wallet not connected</Text>
      )}
      <OwnedObjects />
    </Container>
  );
}
```

## 2-4. On-chain data 활용
`useSuiClientQuery` 같은 hook 을 이용하면 현재 계정이 소유하고 있는 `object`를 가져오는 것도 가능합니다.

```tsx
import { useCurrentAccount, useSuiClientQuery } from '@mysten/dapp-kit';

function ConnectedAccount() {
	const account = useCurrentAccount();

	if (!account) {
		return null;
	}

	return (
		<div>
			<div>Connected to {account.address}</div>;
			<OwnedObjects address={account.address} />
		</div>
	);
}

function OwnedObjects({ address }: { address: string }) {
	const { data } = useSuiClientQuery('getOwnedObjects', {
		owner: address,
	});
	if (!data) {
		return null;
	}

	return (
		<ul>
			{data.data.map((object) => (
				<li key={object.data?.objectId}>
					<a href={`https://example-explorer.com/object/${object.data?.objectId}`}>
						{object.data?.objectId}
					</a>
				</li>
			))}
		</ul>
	);
}
```