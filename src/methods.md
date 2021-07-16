# Method

## getBalance

GetBalance method is a no parameter method to get the user balance of bsv and tokens(currently only bsv).

``` typescript
export interface AccountBalances {
  [account: string]: AssetData[];
}

export interface AssetData {
  symbol: string;
  name: string;
  decimals: string;
  contractAddress?: string;
  balance?: string;
}

const result = await data.client!.request({
    topic: data.session!.topic,
    chainId: 'bsv:livenet',
    request: {
    method: 'getBalance',
    },
});
console.log('getAccountBalances', result);
const balances: AccountBalances = {};
result.forEach(({ account, assets }) => {
    balances[account] = assets;
});
```

According to the data structure wallet will return the balances in array with account and assets, you can just reset it to object for easily use. Currently is only one account return, connect multiple account might be need in the future.

## pay

The most convinent method to construct the payment, just like [handcash.io](https://handcash.io)

``` typescript
export interface Payment {
    to: string;
    currencyCode: string;
    amount: number;
}
export interface Attachment {
    format: string;
    value: any;
}
export interface PaymentParameters {
    payments: Payment[];
    description?: string;
    appAction?: string;
    attachment?: Attachment;
}
export interface PaymentResult {
    transactionId: string;
    satoshiFees: number;
    satoshiAmount: number;
    note?: string;
    type?: string;
    time?: number;
    fiatExchangeRate?: number;
    fiatCurrencyCode?: string;
    participants?: any[];
    attachments?: any;
    appAction?: string;
}

// Simple Payments
const paymentParameters = {
    description: "Hold my beer!🍺",
    appAction: "like",
    payments: [
        { destination: 'nosetwo', currencyCode: 'DUR', sendAmount: 25 },
    ]
};

// Pay to multiple people
const paymentParameters = {
    description: "Hold my beer!🍺",
    appAction: "like",
    payments: [
        { to: 'eyeone', currencyCode: 'DUR', amount: 1 },
        { to: 'eyeone@moneybutton.com', currencyCode: 'EUR', amount: 0.25 },
        { to: '131xrWSKXHbhucFPTfZqnxF8ZhjpMxJH7K', currencyCode: 'USD', amount: 0.25 },
    ]
};

// Attach Data
const paymentParameters = {
    description: "Hold my beer!🍺",
    appAction: "like",
    payments: [
        { destination: 'nosetwo', currencyCode: 'DUR', sendAmount: 25 },
    ],
    attachment: { format: 'json', value: {"param1": "value1", "param2": "value2"} }
};

const pay = async (paymentParameters: PaymentParameters) => {
  const res = await data.client!.request({
    topic: data.session!.topic,
    chainId: CHAIN_ID,
    request: {
      method: DEFAULT_METHODS.pay,
      params: paymentParameters,
    },
  });
  return res;
};

// res structure like 
{
  transactionId: '4c7b7cdc18702bb1a09c75a47bc2fa9630545761fbbd53b8c38735c73173e043',
  note: 'Hold my beer!🍺',
  type: 'send',
  time: 1604958667,
  satoshiFees: 113,
  satoshiAmount: 10000,
  fiatExchangeRate: 160.74284545024352,
  fiatCurrencyCode: 'USD',
  participants: [
    {
      type: 'user',
      alias: 'nosetwo',
      displayName: 'Nose two',
      profilePictureUrl: 'https://res.cloudinary.com/hk7jbd3jh/image/upload/v1574787300/gntqxv6ed7sacwpfwumj.jpg',
      responseNote: ''
    }
  ],
  attachments: [ { value: [Object], format: 'json' } ],
  appAction: 'like'
}
```

## signOutputs

This method is a use case for BIP270, dapp compose the outputs and send it to chainbow, chainbow will compose the inputs and sign the transaction, and return the transaction hex to dapp, dapp will broadcast the transaction in anytime dapp wanted, just like the merchant and customer.

``` typescript
const outputs = [
  {
    script: bsv.Script.buildPublicKeyHashOut(payment.toAddress).toHex(),
    satoshis: payment.amount,
  }
]
const txHex = await data.client!.request({
  topic: data.session!.topic,
  chainId: CHAIN_ID,
  request: {
    method: DEFAULT_METHODS.signOutputs,
    params: outputs,
  },
});
```

## signOutputsAndBroadcast

Just like the signOutputs do but in signOutputsAndBroadcast chainbow will not return transaction hex. Chainbow will broadcast the transaction and return the transaction hash.

``` typescript
const outputs = [
  {
    script: bsv.Script.buildPublicKeyHashOut(payment.toAddress).toHex(),
    satoshis: payment.amount,
  }
]
const txId = await data.client!.request({
  topic: data.session!.topic,
  chainId: CHAIN_ID,
  request: {
    method: DEFAULT_METHODS.signOutputsAndBroadcast,
    params: outputs,
  },
});
```
