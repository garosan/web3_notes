# Lesson 14 - Integration

Start in the `scaffold-eth` project that we created in the last lesson.

Remember to run `yarn start`, `yarn chain` and `yarn deploy` in different windows.

Notice that you can change the contents of the main page by editing `packages/nextjs/app/page.tsx`.

Take a look at `packages/nextjs/scaffold.config.ts` and check all the configuration options available.

Paste this component in the same `page.tsx` file, just outside of the `Home` component, and import it as `<WalletInfo />`.

You can see the different hooks: `address, isConnecting, isDisconnected, chain` that wagmi gives us through `useAccount()` in action.

This is the code to paste:

```tsx
function WalletInfo() {
  const { address, isConnecting, isDisconnected, chain } = useAccount();
  if (address)
    return (
      <div>
        <p>Your account address is {address}</p>
        <p>Connected to the network {chain?.name}</p>
      </div>
    );
  if (isConnecting)
    return (
      <div>
        <p>Loading...</p>
      </div>
    );
  if (isDisconnected)
    return (
      <div>
        <p>Wallet disconnected. Connect wallet to continue</p>
      </div>
    );
  return (
    <div>
      <p>Connect wallet to continue</p>
    </div>
  );
}
```

Now let's add a bit more interaction with our wallet using the `useSignMessage()` hook:

Add this component in your `page.tsx` file too:

```tsx
function WalletAction() {
  const [signatureMessage, setSignatureMessage] = useState("");
  const { data, isError, isPending, isSuccess, signMessage } = useSignMessage();
  return (
    <div className="card w-96 bg-primary text-primary-content mt-4">
      <div className="card-body">
        <h2 className="card-title">Testing signatures</h2>
        <div className="form-control w-full max-w-xs my-4">
          <label className="label">
            <span className="label-text">Enter the message to be signed:</span>
          </label>
          <input
            type="text"
            placeholder="Type here"
            className="input input-bordered w-full max-w-xs"
            value={signatureMessage}
            onChange={(e) => setSignatureMessage(e.target.value)}
          />
        </div>
        <button
          className="btn btn-active btn-neutral"
          disabled={isPending}
          onClick={() =>
            signMessage({
              message: signatureMessage,
            })
          }
        >
          Sign message
        </button>
        {isSuccess && <div>Signature: {data}</div>}
        {isError && <div>Error signing message</div>}
      </div>
    </div>
  );
}
```

Import the component in your `WalletInfo` component here:

```tsx
<div>
  <p>Your account address is {address}</p>
  <p>Connected to the network {chain?.name}</p>
  <WalletAction />
</div>
```

You also need to import the `useSignMessage()` and `useState()` hooks.

You sign a message and it gives us a signature, in my case:

`0xc66d2135e62e9f2333f2bcc4081523b1e7636d20e756620d2bd5954dc7289d037419d348c66028b0ce72e122bf06cab20fa079e5e029dd059992dcdd51c4151f1c`

That's it for that demo. Now let's try the hook `useWalletBalance()`.

Let's add this new component in `page.tsx` once again:

```tsx
function WalletBalance(params: { address: `0x${string}` }) {
  const { data, isError, isLoading } = useBalance({
    address: params.address,
  });

  if (isLoading) return <div>Fetching balance…</div>;
  if (isError) return <div>Error fetching balance</div>;
  return (
    <div className="card w-96 bg-primary text-primary-content mt-4">
      <div className="card-body">
        <h2 className="card-title">Testing useBalance wagmi hook</h2>
        Balance: {data?.formatted} {data?.symbol}
      </div>
    </div>
  );
}
```

Import the wagmi hook and modify your `WalletInfo` component div again:

```tsx
<div>
  <p>Your account address is {address}</p>
  <p>Connected to the network {chain?.name}</p>
  <WalletAction></WalletAction>
  <WalletBalance address={address as `0x${string}`}></WalletBalance>
</div>
```

You can see everything about the `useBalance()` hook [here](https://wagmi.sh/react/api/hooks/useBalance). You can see for example that the `formatted` method is deprecated and now we only have the `value` method.

### Interacting with smart contracts

- Change your configuration at `scaffold.config.ts` (line 16):

```typescript
targetNetworks: [chains.sepolia],
```

With only this, if you connect your actual Metamask and you switch your network to Sepolia, you will see your real balance, how cool!

Copy this one last component into the `page.tsx` file:

```tsx
function TokenInfo(params: { address: `0x${string}` }) {
  return (
    <div className="card w-96 bg-primary text-primary-content mt-4">
      <div className="card-body">
        <h2 className="card-title">Testing useReadContract wagmi hook</h2>
        <TokenName></TokenName>
        <TokenBalance address={params.address}></TokenBalance>
      </div>
    </div>
  );
}

function TokenName() {
  const { data, isError, isLoading } = useReadContract({
    address: "0x37dBD10E7994AAcF6132cac7d33bcA899bd2C660",
    abi: [
      {
        constant: true,
        inputs: [],
        name: "name",
        outputs: [
          {
            name: "",
            type: "string",
          },
        ],
        payable: false,
        stateMutability: "view",
        type: "function",
      },
    ],
    functionName: "name",
  });

  const name = typeof data === "string" ? data : 0;

  if (isLoading) return <div>Fetching name…</div>;
  if (isError) return <div>Error fetching name</div>;
  return <div>Token name: {name}</div>;
}

function TokenBalance(params: { address: `0x${string}` }) {
  const { data, isError, isLoading } = useReadContract({
    address: "0x37dBD10E7994AAcF6132cac7d33bcA899bd2C660",
    abi: [
      {
        constant: true,
        inputs: [
          {
            name: "_owner",
            type: "address",
          },
        ],
        name: "balanceOf",
        outputs: [
          {
            name: "balance",
            type: "uint256",
          },
        ],
        payable: false,
        stateMutability: "view",
        type: "function",
      },
    ],
    functionName: "balanceOf",
    args: [params.address],
  });

  const balance = typeof data === "number" ? data : 0;

  if (isLoading) return <div>Fetching balance…</div>;
  if (isError) return <div>Error fetching balance</div>;
  return <div>Balance: {balance}</div>;
}
```

Last time, import the wagmi hook and modify your `WalletInfo` component div:

```tsx
<div>
  <p>Your account address is {address}</p>
  <p>Connected to the network {chain?.name}</p>
  <WalletAction></WalletAction>
  <WalletBalance address={address as `0x${string}`}></WalletBalance>
  <TokenInfo address={address as `0x${string}`}></TokenInfo>
</div>
```

The important thing to note here is that the `useReadContract()` hook takes the following parameters:

- `abi`: The ABI of the contract you're interacting with.
- `address`: Contract address.
- `functionName`: Name of the read-only function (must be `view` or `pure`).
- `args` _(optional)_: An array of arguments the function expects.
- `chainId` _(optional)_: If the contract is on a chain different from the current connected one.

And `useReadContract()` returns an object like this:

```ts
{
  data, // The return value of the contract function
    error, // Any error encountered
    isLoading, // Boolean for loading state
    isSuccess, // Boolean for successful fetch
    isError, // Boolean for error state
    refetch; // Function to manually refetch the data
}
```

### API Architecture Overview

Dapps, especially big ones like OpenSea will always have a backend. This doesn't mean they're trying to rug or centralize power, its just that a very robust web application will necessarily need a traditional backend to perform certain tasks efficiently.

We will be using the NestJS backend framework for API creation.

### API Architecture References

<https://tray.io/blog/how-do-apis-work>

<https://www.ibm.com/topics/rest-apis>

<https://restfulapi.net/>

<https://aglowiditsolutions.com/blog/most-popular-backend-frameworks/>

## NestJS Framework

- Running services with node
- Web server with node
- Using Express to run a node web server
- About using frameworks
- Using NestJS framework
- Overview of a NestJS project
- Using the CLI
- Initializing a project with NestJS
- Swagger plugin

### NestJS Documentation References

<https://nodejs.org/en/docs/guides/getting-started-guide/>

<https://devdocs.io/express-getting-started/>

<https://docs.nestjs.com/>

<https://docs.nestjs.com/openapi/introduction>

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-14)
- [Video](https://www.youtube.com/watch?v=EDiiD1dZWIM)
- [Wagmi - useBalance() hook](https://wagmi.sh/react/api/hooks/useBalance)
- [Wagmi - useReadContract() hook](https://wagmi.sh/react/api/hooks/useReadContract)
