# ledgerhq/iframe-provider

[![NPM Version](https://img.shields.io/npm/v/@ledgerhq/iframe-provider.svg)](https://www.npmjs.com/package/@ledgerhq/iframe-provider)

This is an [EIP-1193](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md) compliant Ethereum provider that
communicates with a parent iframe using the [Ethereum JSON RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC).

## Purpose

Use the iframe provider to enable a dApp to communicate with an Ethereum provider in a different context.

This was initially built to serve the dApps that integrate with [Ethvault](https://myethvault.com).
Ledger forked it to update it and use it for integration of DAPPS in Ledger Live

## Compatibility

While the protocol is designed for the [Ledger Live Wallet](https://www.ledger.com/ledger-live), it is meant to be general
and work for any iframe based dApp browser.

Contributions are welcome.

## Usage

```typescript
import { IFrameEthereumProvider } from '@ledgerhq/iframe-provider';

let ethereum;

function isIframe(): boolean {
  /// Do some logic...
  return true;
}

if (isIframe()) {
  ethereum = new IFrameEthereumProvider();
} else {
  // Use some other provider, e.g. window.ethereum from MetaMask or Infura
  // ...
}

// Anything from https://github.com/ethereum/wiki/wiki/JSON-RPC should be supported
function getNetwork(): Promise<string> {
  return ethereum.send('net_version');
}
```

You can also use this with the [ethers.js](https://github.com/ethers-io/ethers.js) library
via the [Web3Provider](https://docs.ethers.io/ethers.js/html/api-providers.html#web3provider-inherits-from-jsonrpcprovider).

```typescript
import { IFrameEthereumProvider } from '@ledgerhq/iframe-provider';
import { Web3Provider } from 'ethers';

let web3Provider = new Web3Provider(new IFrameEthereumProvider());
```

There are some options for the construction of the ethereum provider:

```typescript
import { IFrameEthereumProvider } from '@ledgerhq/iframe-provider';

new IFrameEthereumProvider({
  // How long to wait for the response, default 1 minute
  timeoutMilliseconds: 60000,
  // The origins with which this provider is allowed to communicate, default '*'
  // See postMessage docs https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage
  targetOrigin: 'https://my-dapp-browser-exemple.com',
});
```

## Local Development

This project was bootstrapped with [TSDX](https://github.com/jaredpalmer/tsdx).

Below is a list of commands you will probably find useful.

### `npm start` or `yarn start`

Runs the project in development/watch mode. Your project will be rebuilt upon changes. TSDX has a special logger for you convenience. Error messages are pretty printed and formatted for compatibility VS Code's Problems tab.

<img src="https://user-images.githubusercontent.com/4060187/52168303-574d3a00-26f6-11e9-9f3b-71dbec9ebfcb.gif" width="600" />

Your library will be rebuilt if you make edits.

### `npm run build` or `yarn build`

Bundles the package to the `dist` folder.
The package is optimized and bundled with Rollup into multiple formats (CommonJS, UMD, and ES Module).

<img src="https://user-images.githubusercontent.com/4060187/52168322-a98e5b00-26f6-11e9-8cf6-222d716b75ef.gif" width="600" />

### `npm test` or `yarn test`

Runs the test watcher (Jest) in an interactive mode.
By default, runs tests related to files changed since the last commit.
