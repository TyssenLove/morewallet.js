# morewallet.js

![https://www.npmjs.com/package/morewallet.js](https://img.shields.io/npm/v/morewallet.js.svg) ![](https://img.shields.io/badge/language-javascript-red.svg)

> more wallet dapp js sdk

[中文版](https://github.com/EOSMore/morewallet.js/blob/master/README-zh.md)

## Example

[Complete Example](https://github.com/EOSMore/morewalletjsdemo)

### ES6
```javascript
import mw from 'morewallet.js';

const dappName = "dappdemo";
const client = mw.getClient(dappName);

//Get APP Version Number
client.getAppVersion().then(version => {
  console.log(version);
}).catch(error => {
  console.error(error);
});

//Current Account Information
client.getAccount();

//If Open in Wallet App
client.openInApp();

//Check If Can Execute A Cretain Action
client.checkAction("eosio.token", "transfer");

//Submit Action
const buyramData = {
  payer: "demouser1111",
  receiver: "demouser1111",
  quantity: "10.0000 EOS"
};
const authorization = [{
  actor: "demouser1111",
  permission: "active"
}];
client.pushAction("eosio", "buyram", authorization, buyramData);

//Submit Action In Batch
const actions = [{
  account: "eosio",
  name: "buyram",
  authorization: [{
    actor: "demouser1111",
    permission: "active"
  }],
  data: {
    payer: "demouser1111",
    receiver: "demouser1111",
    quant: "10.0000 EOS"
  }
}, {
  account: "eosio",
  name: "delegatebw",
  authorization: [{
    actor: "demouser1111",
    permission: "active"
  }],
  data: {
    from: "demouser1111",
    receiver: "demouser1111",
    stake_net_quantity: "10.0000 EOS",
    stake_cpu_quantity: "5.0000 EOS",
    transfer: false
  }
}];

client.pushActions(actions);

//Transfer
client.transfer("eosio.token", "demouser1111", "100.0000 EOS", "hi");

//Get Balance
client.getCurrencyBalance("eosio.token", "EOS");

//Data Table
client.getTableRows({
  code: "eosio",
  scope: "eosio",
  table: "global"
});

//Use Private Key signed Text
client.signText("text");

```

### UMD

- Introduce script to page：

```html
<script src="https://cdn.more.top/morewallet/1.0.5/morewallet.js.min.js"></script>
```

- Use

```html
<script>
  var dappName = "dappdemo"; //dapp name, shoud be the name field filled in wallet APP
  var client = MOREWALLET.getClient(dappName);
  client.getCurrencyBalance("eosio.token", "EOS");
</script>
```

## Access

### client.getAccount(account = "")

**Parameter**

- account - the one want to check, no transfer means check current account

> Check Account Information

**Return Value**

- account_name - String account name 
- core_liquid_balance - String eos balance
- ram_quota - Integer ram
- net_weight - Integer bandwidth
- cpu_weight - Integer CPU
- permissions - Array authority information
- total_resources - Object resources information
- voter_info - Object voting information

**Example**

```javascript
// async/await
const accountInfo = await client.getAccount();

// Promise
client.getAccount().then(accountInfo => {
    console.log(accountInfo);
}).catch(e => {
    console.error(e);
});
```

### client.openInApp()

> If Open in Wallet APP

**Return Value**

- res - Boolean

**Example**

```javascript
// async/await
const inApp = await client.openInApp()

// Promise
client.openInApp().then(inApp => {
    if (!inApp) {
        console.log("open in morewallet")
    }
})
```

### client.getAppVersion()

> Get APP Version Number

**Return Value**

- version - String

### client.checkAction(contract, action)

> Check If Have Action Authority

**Parameter**

- contract - contract account
- action - action name

**Return Value**

- res - Boolean

### client.getCurrencyBalance(contract, symbol)

> Get Balance of Appointed Token

**Parameter**

- contract - contract account
- symbol - symbol name

### client.pushAction(contract, action, authorization, data)

> Submit Action

**Parameter**

- contract - contract account
- action - action name
- authorization - authority array
- data - executive data

### client.pushActions(actions)

> Submit Action in Batch

**Parameter**

- actions - action array

### client.transfer(contract, to, quantity, memo)

> Transfer

**Parameter**

- contract - token contract
- to - receiver
- quantity - transfer amount
- memo - transfer memo

### client.getTableRows(params)

> Get Data Table

**Parameter**

- params - check information，refer to`/chain/get_table_rows`

### client.signText(text)

> Use Private Key Signed Text

**Parameter**

- text - signed text

**Return Value**

- signedText - String