# ChainBow Documentation

::: warning

ChainBow is still in alpha version

:::

ChainBow is a wallet for the Bitcoin SV blockchain. It is using the [walletconnect v2.0](https://docs.walletconnect.org/v/2.0/) to solve the connect and payment or other operations that DApp need to connect with the wallet.

However, ChainBow is far more than that. There is a currency conversion, dex, smart contracts, support for multiple outputs, and so on. We are aiming to do the best BitcoinSV wallet. We are synchronizing the UTXO from the data provider, not using the server to save user UTXO. And using [cup protocol](https://cup.network) to solve the token issue in the BitcoinSV blockchain.

# Integrate ChainBow in DApp

## What need to do for using chainbow in your dapp

Refer to [walletconnect v2.0](https://docs.walletconnect.org/v/2.0/) read the dapp section that you know you are going to need install the dependcies. And we are developing the game platform [https://game.mydapp.io](https://game.mydapp.io) that can show the use case for chainbow.

### Before started

Before started to developing we need to know a few constant.

``` typescript
export const DEFAULT_RELAY_PROVIDER = 'wss://wc.cercle.sg';
export const CHAIN_ID = 'bsv:livenet';

export const DEFAULT_METHODS = {
  getBalance: 'getBalance',
  pay: 'pay',
  signOutputsAndBroadcast: 'signOutputsAndBroadcast',
};
```

#### DEFAULT_RELAY_PROVIDER

This is the relay server that we have been deploy on our server, according to the walletconnect v2.0 documentation, relay server you just can deploy on your own server or use ours, or any others.

#### CHAIN_ID

This is a predefined just like Ethereum or other block chains do.

#### DEFAULT_METHODS

This is the methods has been defined in dapp and chainbow to do the json rpc.
