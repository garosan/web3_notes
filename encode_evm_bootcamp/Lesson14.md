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

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-14)
- [Video](https://www.youtube.com/watch?v=EDiiD1dZWIM)
