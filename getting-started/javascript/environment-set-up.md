# Environment Set-up

In this section you will complete the following:

* Create your project directory
* Create a .env file and store your Hedera testnet account ID and private key
* Set-up your Hedera testnet client

{% hint style="warning" %}
**Note:** The Hedera JavaScript SDK supports React Native with Expo on Android devices and Android emulators. It does not currently support React Native Bare.
{% endhint %}

## Pre-requisites:

{% page-ref page="../introduction.md" %}

## Step 1: Set up your node.js environment

### 1.1 Create a new directory for our sample & move into it

Open your terminal and create a directory called hello-hedera-js-sdk. After you create the project directory navigate into the directory.

```bash
mkdir hello-hedera-js-sdk && cd hello-hedera-js-sdk
```

### 1.2. Initialize a node.js project in this new directory

```bash
npm init
```

Note: you can just say “yes” to all of the defaults and/or plugin what makes sense. It’s an example!

```bash
{
  "name": "hello-hedera-js-sdk",
  "version": "",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

## Step 2: Install the Hedera JavaScript SDK

{% hint style="warning" %}
Note: Please follow the example in the version of the SDK you are using. Code specific to v2.0 of the SDK will be marked in the code block tab title, otherwise, it works for both v2.0 and v1.0. The examples may not be compatible if you are using a different version than what is listed.
{% endhint %}

Now that you have your node environment setup, we can get started with Hedera’s JS SDK! You can open this project in your favorite text editor like [Visual Studio Code](https://code.visualstudio.com/Download).

* Install it with your favorite package manager.

{% tabs %}
{% tab title="v2.0" %}
```text
// install Hedera's JS SDK with NPM
// This example uses Hedera JavaScript SDK v2.0.13
npm install --save @hashgraph/sdk@2.0.13

// Install with Yarn
yarn add @hashgraph/sdk@2.0.13
```
{% endtab %}

{% tab title="v1.0" %}
```text
// install Hedera's JS SDK with NPM
// This example uses Hedera JavaScript SDK v1.4.4
npm install --save @hashgraph/sdk@1.4.4

// Install with Yarn
yarn add @hashgraph/sdk@1.4.4
```
{% endtab %}
{% endtabs %}

* Install `dotenv` with your favorite package manager. This will allow our node environment to use your testnet account ID and the private key that we will store in a .env file next.

```text
// install with NPM
npm install dotenv

// Install with Yarn
yarn add dotenv
```

## Step 3: Create a .env file in your project

The .env file will store your Hedera testnet **account ID** and **private key.** Create this file in the root directory of your project and save it as **.env** file.

Now you can add your testnet account ID and private key provided from your [Hedera Portal](https://portal.hedera.com/) Account.

{% tabs %}
{% tab title=".env" %}
```text
MY_ACCOUNT_ID= TESTNET ACCOUNT ID (0.0.x)
MY_PRIVATE_KEY= TESTNET PRIVATE KEY (302e...)
```
{% endtab %}
{% endtabs %}

## Step 4: Create an index.js file in the 'root' directory

This file will contain the code we will write in the following samples. Your project structure should look something like this after:

![](../../.gitbook/assets/project_directory.png)

Grab your Hedera testnet account ID and private key from the .env file.

```javascript
const { Client } = require("@hashgraph/sdk");
require("dotenv").config();

async function main() {

    //Grab your Hedera testnet account ID and private key from your .env file
    const myAccountId = process.env.MY_ACCOUNT_ID;
    const myPrivateKey = process.env.MY_PRIVATE_KEY;

    // If we weren't able to grab it, we should throw a new error
    if (myAccountId == null ||
        myPrivateKey == null ) {
        throw new Error("Environment variables myAccountId and myPrivateKey must be present");
    }
}
main();
```

## Step 5: Create your Hedera testnet client

You have the option to create a client for the Hedera mainnet or testnet. Since we are using a Hedera testnet account ID and private key, we will create a client for the Hedera testnet.

After you create your Hedera testnet client, you will need to set the operator information. The operator is the account that will pay for the transaction and query fees. You will need to sign the transaction or query with the private key of the account to authorize the payment.

{% code title="index.js" %}
```javascript
// Create our connection to the Hedera network
// The Hedera JS SDK makes this really easy!
const client = Client.forTestnet();

client.setOperator(myAccountId, myPrivateKey);
```
{% endcode %}

{% hint style="info" %}
The client has a default **max transaction fee** of 100,000,000 tinybars \(1 hbar\) and default **max query payment** of 100,000,000 tinybars \(1 hbar\). If you need to change these values, you can use`.setMaxTransactionFee()` for a transaction and `.setMaxQueryPayment()` for queries.
{% endhint %}

Your project environment is now set up to successfully submit transactions/queries to the Hedera test network!

Next, you will learn how to create a Hedera testnet account.

## Code Check ✅

What your index.js file should look like at this point:

{% code title="index.js" %}
```javascript
const { Client } = require("@hashgraph/sdk");
require("dotenv").config();

async function main() {

    //Grab your Hedera testnet account ID and private key from your .env file
    const myAccountId = process.env.MY_ACCOUNT_ID;
    const myPrivateKey = process.env.MY_PRIVATE_KEY;

    // If we weren't able to grab it, we should throw a new error
    if (myAccountId == null ||
        myPrivateKey == null ) {
        throw new Error("Environment variables myAccountId and myPrivateKey must be present");
    }

    // Create our connection to the Hedera network
    // The Hedera JS SDK makes this really easy!
    const client = Client.forTestnet();

    client.setOperator(myAccountId, myPrivateKey);
}
main();
```
{% endcode %}

