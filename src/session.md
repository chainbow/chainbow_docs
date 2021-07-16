# Session

Before you do any operation that dapp request to chainbow, you will need to set up the session.

Define app meta data like below

``` typescript
export interface AppMetadata {
    name: string;
    description: string;
    url: string;
    icons: string[];
}

<AppMetadata>{
    name: 'ddz',
    description: 'chainbow ddz',
    url: 'http://chainbow.io',
    icons: ['http://image.5you.com/attachment/soft/2020/1021/100148_22591334.png'],
};
```

Trigger the session connect proposal like below

``` typescript
const session = await data.client!.connect({
  metadata: metadata,
  pairing: undefined,
  permissions: {
    blockchain: {
      chains: ['bsv:livenet'],
    },
    jsonrpc: {
      methods: Object.values(DEFAULT_METHODS),
    },
  },
});
```

Trigger the session disconnect proposal like below

``` typescript
if (data.session) {
  await data.client?.disconnect({
    topic: data.session.topic,
    reason: ERROR.USER_DISCONNECTED.format(),
  });
}
data.session = undefined;
data.accounts = [];
```
