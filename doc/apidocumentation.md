Master Core API Documentation
=============================

##Introduction
Master Core is a fork of Bitcoin Core, with Master Protocol feature support added as a new layer of functionality on top.  As such interacting with the API is done in the same manner (JSON-RPC) as Bitcoin Core, simply with additional RPC calls available for utlizing Master Protocol features.

As all existing Bitcoin Core functionality is inherent to Master Core, the RPC port by default remains as 8332 as per Bitcoin Core.  If you wish to run Master Core in tandem with Bitcoin Core (eg via a seperate datadir) you may utilize the -rpcport<port> option to nominate an alternative port number.

*Caution: This document is a work in progress and is subject to change at any time.  There may be errors, omissions or inaccuracies present.*

###Simple Send
Simple send allows a Master Protocol currency to be transferred from address to address in a one-to-one transaction.  Simple send transactions are exposed via the **send_MP** RPC call.

**Required Parameters**
- **_sender address (string):_** A valid bitcoin address containing a sufficient balance to support the transaction
- **_recipient address (string):_** A valid bitcoin address - the receiving party of the transaction
- **_currency/property ID (integer):_** A valid Master Protocol currency/property ID
- **_amount (float):_** The amount to transfer (note if sending individisble tokens any decimals will be truncated)
   
**Additional Optional Parameters**
- There are currently no supported optional parameters for this call.

**Examples**
```
$src/mastercored send_MP myN6HXmFhmMRo1bzfNXBDxTALYsh3EjXxk mvKKTXj8Z1GVwjN1Ejw8yx6n7pBujdXG2Q 1 1.234
d300bb52c099c664459a75908255c8ec6a58575ac8efb07080bd81d8e6c9af40
```
```
{"jsonrpc":"1.0","id":"1","method":"send_MP","params":["myN6HXmFhmMRo1bzfNXBDxTALYsh3EjXxk","mvKKTXj8Z1GVwjN1Ejw8yx6n7pBujdXG2Q",1,1.234]}
{"result":"d300bb52c099c664459a75908255c8ec6a58575ac8efb07080bd81d8e6c9af40","error":null,"id":"1"}
```
*Please note, the private key for the requested sender address must be available in the wallet.*

###Obtaining a Master Protocol balance
The **getbalance_MP** call allows for retrieval of a Master Protocol balance for a given address and currency/property ID.

**Required Parameters**
- **_address (string):_** A valid bitcoin address 
- **_currency/property ID (integer):_** A valid Master Protocol currency/property ID

**Additional Optional Parameters**
- There are currently no supported optional parameters for this call.

**Examples**
```
$src/mastercored getbalance_MP mvKKTXj8Z1GVwjN1Ejw8yx6n7pBujdXG2Q 1
300000000.00000000
```
```
{"jsonrpc":"1.0","id":"1","method":"getbalance_MP","params":["mvKKTXj8Z1GVwjN1Ejw8yx6n7pBujdXG2Q",1]}
{"result":300000000.00000000,"error":null,"id":"1"}
```

###Retrieving a Master Protocol Transaction
The **gettransaction_MP** call allows for retrieval of a Master Protocol transaction and its associated details and validity.  

**Required Parameters**
- **_transaction ID (string):_** A valid Master Protocol transaction ID

**Additional Optional Parameters**
- There are currently no supported optional parameters for this call.

**Examples**
```
$src/mastercored gettransaction_MP d2907fe2c716fc6d510d63b52557907445c784cb2e8ae6ea9ef61e909c978cd7
{
    "txid" : "d2907fe2c716fc6d510d63b52557907445c784cb2e8ae6ea9ef61e909c978cd7",
    "sendingaddress" : "myN6HXmFhmMRo1bzfNXBDxTALYsh3EjXxk",
    "referenceaddress" : "mhgrKJ3WyX1RMYiUpgA3M3iF48zSeSRkri",
    "direction" : "out",
    "confirmations" : 884,
    "fee" : 0.00010000,
    "blocktime" : 1403298479,
    "blockindex" : 49,
    "type" : "Simple Send",
    "currency" : 1,
    "divisible" : true,
    "amount" : 50.00000000,
    "valid" : true
}
```
```
{"jsonrpc":"1.0","id":"1","method":"gettransaction_MP","params":["d2907fe2c716fc6d510d63b52557907445c784cb2e8ae6ea9ef61e909c978cd7"]}
{"result":{"txid":"d2907fe2c716fc6d510d63b52557907445c784cb2e8ae6ea9ef61e909c978cd7","sendingaddress":"myN6HXmFhmMRo1bzfNXBDxTALYsh3EjXxk","referenceaddress":"mhgrKJ3WyX1RMYiUpgA3M3iF48zSeSRkri","direction":"out","confirmations":884,"fee":0.00010000,"blocktime":1403298479,"blockindex":49,"type":"Simple Send","currency":1,"divisible":true,"amount":50.00000000,"valid":true},"error":null,"id":"1"}
```
*Please note, gettransaction_MP currently supports transactions available in the wallet only.*

###Listing Historical Transactions
The **listtransactions_MP** call allows for retrieval of the last n Master Protocol transactions, if desired filtered on address.

**Required Parameters**
- There are no required parameters for this call.  Calling with no parameters will default to all addresses in the wallet and the last 10 transactions.

**Additional Optional Parameters**
- **_address (string):_** A valid bitcoin address to filter on or * for all addresses in the wallet
- **_count (integer):_** The number of recent transactions to return
- **_skip (integer):_** The number of recent transactions to skip 

**Examples**

Optional parameters can be combined as follows ```listtransactions_MP "*" 50 100``` to list the 50 most recent transactions across all addresses in the wallet, skipping the first 100. 

```
$src/mastercored listtransactions_MP mtGfANEnFsniGzWDt87kQg4zJunoQbT6f3
[
    {
        "txid" : "fda128e34edc48426ca930df6167e4560cef9cda2192e37be69c965e9c5dd9d1",
        "sendingaddress" : "mscsir9qKUYry5SqaW19T7fTriDw2BzYvD",
        "referenceaddress" : "mtGfANEnFsniGzWDt87kQg4zJunoQbT6f3",
        "direction" : "in",
        "confirmations" : 1457,
        "blocktime" : 1403126898,
        "blockindex" : 7,
        "type" : "Simple Send",
        "currency" : 1,
        "divisible" : true,
        "amount" : 123456.00000000,
        "valid" : true
    },
    {
        "txid" : "33e4ea9a43102f9ad43b086d2bcf9478c67b5a1e64ce7dfc64bfe3f94b7f9222",
        "sendingaddress" : "mscsir9qKUYry5SqaW19T7fTriDw2BzYvD",
        "referenceaddress" : "mtGfANEnFsniGzWDt87kQg4zJunoQbT6f3",
        "direction" : "in",
        "confirmations" : 1454,
        "blocktime" : 1403129492,
        "blockindex" : 4,
        "type" : "Simple Send",
        "currency" : 1,
        "divisible" : true,
        "amount" : 222.00000000,
        "valid" : true
    },
    {
        "txid" : "c93a8622b6784b4cd5e109bea423553ed729b675965b6820837f80513be04852",
        "sendingaddress" : "myN6HXmFhmMRo1bzfNXBDxTALYsh3EjXxk",
        "referenceaddress" : "mtGfANEnFsniGzWDt87kQg4zJunoQbT6f3",
        "direction" : "out",
        "confirmations" : 906,
        "blocktime" : 1403293908,
        "blockindex" : 2,
        "type" : "Simple Send",
        "currency" : 1,
        "divisible" : true,
        "amount" : 50.12340000,
        "valid" : true
    }
]
```
```
{"jsonrpc":"1.0","id":"1","method":"listtransactions_MP","params":["mtGfANEnFsniGzWDt87kQg4zJunoQbT6f3"]}
{"result":[{"txid":"fda128e34edc48426ca930df6167e4560cef9cda2192e37be69c965e9c5dd9d1","sendingaddress":"mscsir9qKUYry5SqaW19T7fTriDw2BzYvD","referenceaddress":"mtGfANEnFsniGzWDt87kQg4zJunoQbT6f3","direction":"in","confirmations":1457,"blocktime":1403126898,"blockindex":7,"type":"Simple Send","currency":1,"divisible":true,"amount":123456.00000000,"valid":true},{"txid":"33e4ea9a43102f9ad43b086d2bcf9478c67b5a1e64ce7dfc64bfe3f94b7f9222","sendingaddress":"mscsir9qKUYry5SqaW19T7fTriDw2BzYvD","referenceaddress":"mtGfANEnFsniGzWDt87kQg4zJunoQbT6f3","direction":"in","confirmations":1454,"blocktime":1403129492,"blockindex":4,"type":"Simple Send","currency":1,"divisible":true,"amount":222.00000000,"valid":true},{"txid":"c93a8622b6784b4cd5e109bea423553ed729b675965b6820837f80513be04852","sendingaddress":"myN6HXmFhmMRo1bzfNXBDxTALYsh3EjXxk","referenceaddress":"mtGfANEnFsniGzWDt87kQg4zJunoQbT6f3","direction":"out","confirmations":906,"blocktime":1403293908,"blockindex":2,"type":"Simple Send","currency":1,"divisible":true,"amount":50.12340000,"valid":true}],"error":null,"id":"1"}
```
*Please note, listtransactions_MP currently supports transactions available in the wallet only.*


