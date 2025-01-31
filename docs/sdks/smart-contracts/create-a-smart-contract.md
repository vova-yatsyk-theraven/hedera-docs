# Create a smart contract

{% hint style="info" %}
For support in the broader Hedera ecosystem, it’s recommended to mint a token on Hedera using the [Hedera Token Service](../tokens/define-a-token.md). In the near future, smart contracts will be able to interact with the Hedera Token Service.
{% endhint %}

A transaction that creates a new smart contract instance. After the contract is created you can get the new contract ID by requesting the receipt of the transaction. To create the solidity smart contract, you can use [remix](https://remix.ethereum.org/#optimize=false\&runs=200\&evmVersion=null\&version=soljson-v0.8.7+commit.e28d00a7.js) or another [solidity](https://docs.soliditylang.org/en/v0.8.9/) compiler. After you have the hex-encoded byte code of the smart contract you need to store that on a file using the [Hedera File Service](../file-storage/create-a-file.md). Then you will create the smart contract instance that will run the byte code stored in the given Hedera file, referenced either by file ID. The constructor will be executed using the given amount of gas.&#x20;

The constructor will be executed using the given amount of gas, and any unspent gas will be refunded to the paying account. Constructor inputs come from the given `constructorParameters`.&#x20;

If this constructor stores information, it is charged gas to store it. There is a fee in hbars to maintain that storage until the expiration time, and that fee is added as part of the transaction fee.

Please refer to [HIP-26](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-26.md) for [opcode](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-26.md#changes-unrelated-to-london-hard-fork) support and [gas costs](https://github.com/hashgraph/hedera-improvement-proposal/blob/master/HIP/hip-26.md#table-of-gas-cost-changes).

{% hint style="warning" %}
**Solidity Support**\
****The latest version of Solidity is supported on all networks (v0.8.9).
{% endhint %}

{% hint style="warning" %}
**Smart Contract State Size and Gas Limit**\
****Each smart contract has a maximum state size of 1MB which can store up to approximately 16,000 key-value pairs. The maximum gas limit is 300\_000.
{% endhint %}

**Transaction Signing Requirements**

* The client operator account is required to sign the transaction
* The admin key, if specified&#x20;

**Smart Contract Properties**

| **Field**                  | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Admin Key**              | Sets the state of the instance and its fields can be modified arbitrarily if this key signs a transaction to modify it. If this is null, then such modifications are not possible, and there is no administrator that can override the normal operation of this smart contract instance. Note that if it is created with no admin keys, then there is no administrator to authorize changing the admin keys, so there can never be any admin keys for that instance. |
| **Gas**                    | The gas to run the constructor.                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Initial Balance **       | The initial number of hbars to put into the cryptocurrency account associated with and owned by the smart contract.                                                                                                                                                                                                                                                                                                                                                  |
| **Byte Code File**         | The file containing the hex encoded smart contract byte code.                                                                                                                                                                                                                                                                                                                                                                                                        |
| **Proxy Account**          | The ID of the account to which this account is proxy staked. If proxyAccountID is null, or is an invalid account, or is an account that isn't a node, then this account is automatically proxy staked to a node chosen by the network, but without earning payments. If the proxyAccountID account refuses to accept proxy staking, or if it is not currently running a node, then it will behave as if proxyAccountID was null.                                     |
| **Auto Renew Period**      | The period that the instance will charge its account every this many seconds to renew.                                                                                                                                                                                                                                                                                                                                                                               |
| **Constructor Parameters** | The constructor parameters to pass.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| **Memo**                   | The memo to be associated with this contract. (max 100 bytes)                                                                                                                                                                                                                                                                                                                                                                                                        |

| Constructor                       | Description                                 |
| --------------------------------- | ------------------------------------------- |
| `new ContractCreateTransaction()` | Initializes the ContractCreateTransaction() |

```java
new ContractCreateTransaction()
```

### Methods

{% tabs %}
{% tab title="V2" %}
| Method                                              | Type                                           | Requirement |
| --------------------------------------------------- | ---------------------------------------------- | ----------- |
| `setGas(<gas>)`                                     | long                                           | Required    |
| `setBytecodeFileId(<fileId>)`                       | [FileId](../specialized-types.md#fileid)       | Required    |
| `setInitialBalance(<initialBalance>)`               | Hbar                                           | Optional    |
| `setAdminKey(<keys>)`                               | Key                                            | Optional    |
| `setProxyAccountId(<accountId>`)                    | [AccountId](../specialized-types.md#accountid) | Optional    |
| `setConstructorParameters(<constructorParameters>)` | byte \[ ]                                      | Optional    |
| `setConstructorParameters(<constructorParameters>)` | ContractFunctionParameters                     | Optional    |
| `setContractMemo(<memo>)`                           | String                                         | Optional    |
| `setAutoRenewPeriod(<autoRenewPeriod>)`             | Duration                                       | Optional    |

{% code title="Java" %}
```java
//Create the transaction
ContractCreateTransaction transaction = new ContractCreateTransaction()
    .setGas(100_000_000)
    .setBytecodeFileId(bytecodeFileId)
    .setAdminKey(adminKey);
    
//Modify the default max transaction fee (default: 1 hbar)
ContractCreateTransaction modifyTransactionFee = transaction.setMaxTransactionFee(new Hbar(16));

//Sign the transaction with the client operator key and submit to a Hedera network
TransactionResponse txResponse = modifyTransactionFee.execute(client);

//Get the receipt of the transaction
TransactionReceipt receipt = txResponse.getReceipt(client);

//Get the new contract ID
ContractId newContractId = receipt.contractId;
        
System.out.println("The new contract ID is " +newContractId);
//v2.0.0
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
//Create the transaction
const transaction = new ContractCreateTransaction()
    .setGas(100_000_000)
    .setBytecodeFileId(bytecodeFileId)
    .setAdminKey(adminKey);

//Modify the default max transaction fee (default: 1 hbar)
const modifyTransactionFee = transaction.setMaxTransactionFee(new Hbar(16));

//Sign the transaction with the client operator key and submit to a Hedera network
const txResponse = await modifyTransactionFee.execute(client);

//Get the receipt of the transaction
const receipt = await txResponse.getReceipt(client);

//Get the new contract ID
const newContractId = receipt.contractId;
        
console.log("The new contract ID is " +newContractId);

```
{% endcode %}

{% code title="Go" %}
```java
//Create the transaction
transaction := hedera.NewContractCreateTransaction().
		SetGas(100000000).
		SetBytecodeFileID(byteCodeFileID).
		SetAdminKey(adminKey)

//Sign the transaction with the client operator key and submit to a Hedera network
txResponse, err := transaction.Execute(client)
if err != nil {
		panic(err)
}

//Request the receipt of the transaction
receipt, err = txResponse.GetReceipt(client)
if err != nil {
		panic(err)
}

//Get the topic ID
newContractId := *receipt.ContractID

fmt.Printf("The new topic ID is %v\n", newContractId)
//v2.0.0
```
{% endcode %}
{% endtab %}

{% tab title="V1" %}
| Method                                              | Type                                     | Requirement |
| --------------------------------------------------- | ---------------------------------------- | ----------- |
| `setGas(<gas>)`                                     | long                                     | Required    |
| `setBytecodeFileId(<fileId>)`                       | [FileId](../specialized-types.md#fileid) | Required    |
| `setInitialBalance(<initialBalance>)`               | long/Hbar                                | Optional    |
| `setAdminKey(<publicKeys>)`                         | Ed25519PublicKey                         | Optional    |
| `setProxyAccountId(<accountId>`)                    | AccountId                                | Optional    |
| `setConstructorParameters(<constructorParameters>)` | byte \[ ]                                | Optional    |
| `setConstructorParameters(<constructorParameters>)` | ContractFunctionParameters               | Optional    |
| `setContractMemo(<memo>)`                           | String                                   | Optional    |
| `setAutoRenewPeriod(<autoRenewPeriod>)`             | Duration                                 | Optional    |

{% code title="Java" %}
```java
//Create the transaction
ContractCreateTransaction transaction = new ContractCreateTransaction()
     .setBytecodeFileId(fileId)
     .setGas(100_000_000)
     .setConstructorParams(new ContractFunctionParams().addString("hello from hedera!"));

//Sign with the client operator account private key, submit to a Hedera network        
TransactionId txId = transaction.execute(client);

//Get the receipt of the transaction
TransactionReceipt receipt = txId.getReceipt(client);

//Get the new contract ID
ContractId newContractId = receipt.getContractId();
        
System.out.println("The new contract ID is " + newContractId);

//v1.3.2
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
//Create the transaction
const transaction = new ContractCreateTransaction()
     .setBytecodeFileId(fileId)
     .setGas(100_000_000)
     .setConstructorParams(new ContractFunctionParams().addString("hello from hedera!"));

//Sign with the client operator account private key, submit to a Hedera network        
const txId = await transaction.execute(client);

//Get the receipt of the transaction
const receipt = await txId.getReceipt(client);

//Get the new contract ID
const newContractId = receipt.getContractId();
        
System.out.println("The new contract ID is " + newContractId);

//v1.4.4

```
{% endcode %}
{% endtab %}
{% endtabs %}

## Get transaction values

{% tabs %}
{% tab title="V2" %}
| Method                                              | Type       | Requirement |
| --------------------------------------------------- | ---------- | ----------- |
| `getAdminKey(<keys>)`                               | Key        | Optional    |
| `getGas(<gas>)`                                     | long       | Optional    |
| `getInitialBalance(<initialBalance>)`               | Hbar       | Optional    |
| `getBytecodeFileId(<fileId>)`                       | FileId     | Optional    |
| `getProxyAccountId(<accountId>`)                    | AccountId  | Optional    |
| `getConstructorParameters(<constructorParameters>)` | ByteString | Optional    |
| `getContractMemo(<memo>)`                           | String     | Optional    |
| `getAutoRenewPeriod(<autoRenewPeriod>)`             | Duration   | Optional    |

{% code title="Java" %}
```java
//Create the transaction
ContractCreateTransaction transaction = new ContractCreateTransaction()
    .setGas(500)
    .setBytecodeFileId(bytecodeFileId)
    .setAdminKey(adminKey);

//Get admin key
transaction.getAdminKey()

//v2.0.0
```
{% endcode %}

{% code title="JavaScript" %}
```javascript
//Create the transaction
const transaction = await new ContractCreateTransaction()
    .setGas(500)
    .setBytecodeFileId(bytecodeFileId)
    .setAdminKey(adminKey);

//Get admin key
transaction.getAdminKey()

//v2.0.0
```
{% endcode %}

{% code title="Go" %}
```java
//Create the transaction
transaction := hedera.NewContractCreateTransaction().
		SetGas(500).
		SetBytecodeFileID(byteCodeFileID).
		SetAdminKey(adminKey)

//Get admin key
transaction.GetAdminKey()

//v2.0.0
```
{% endcode %}
{% endtab %}
{% endtabs %}



##
