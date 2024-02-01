---
description: >-
  Forking lets you interact with contracts and run tests as if on a real network. Learn how to fork Hedera Testnet on the latest block and test your contracts with the latest state of the network.
---

# Fork Hedera Testnet: Interact with Deployed Contracts on Latest Block


## What you will accomplish

* [ ] Deploy your smart contract to Hedera Testnet using `forge create`
* [ ] Use `cast` command-line tool to execute a contract call
* [ ] Fork Hedera Testnet on the latest block & run your tests against your deployed contract

## Prerequisites

Before you begin, you should be familiar with the following:

* [x] [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
* [x] [Solidity](https://docs.soliditylang.org/en/latest/)
* [x] [Foundry](https://book.getfoundry.sh/)

<details>

<summary>Also, you should have the following set up on your computer ⬇ </summary>


* [x] `git` installed
    * Minimum version: 2.37
    * Recommended: [Install Git (Github)](https://github.com/git-guides/install-git)
* [x] A code editor or IDE
    * Recommended: [VS Code. Install VS Code (Visual Studio)](https://code.visualstudio.com/docs/setup/setup-overview)
* [x] NodeJs + npm installed
    * Minimum version of NodeJs: 18
    * Minimum version of npm: 9.5
    * Recommended for Linux & Mac: [nvm](https://github.com/nvm-sh/nvm)
    * Recommended for Windows: [nvm-windows](https://github.com/coreybutler/nvm-windows)
* [x] foundry `forge` and `cast` installed
    * `forge` Minimum version: 0.2.0
    * `cast` Minimum version: 0.2.0

</details>

<details>

<summary>Check your prerequisites set up ⬇ </summary>

Open your terminal, and enter the following commands.

```shell
git --version
code --version
node --version
npm --version
forge --version
cast --version
```

Each of these commands should output some text that includes a version number, for example:

```text
git --version
git version 2.39.2 (Apple Git-143)

code --version
1.81.1
6c3e3dba23e8fadc360aed75ce363ba185c49794
arm64

node --version
v20.6.1

npm --version
9.8.1

forge --version
0.2.0 (6fcbbd8 2023-12-15T00:29:51.472038000Z)

cast --version
0.2.0 (6fcbbd8 2023-12-15T00:29:51.851258000Z)

```
If the output contains text similar to `command not found`, please install that item.

</details>

***

## Get started

### Set up project

To follow along, start with the `main` branch,
which is the _default branch_ of this repository.
This gives you the initial state from which you can follow along
with the steps as described in the tutorial.

`forge` manages dependencies by using git submodules. Clone the following project and pass `--recurse-submodules` to the `git clone` command to automatically initialize and update the submodule in the repository.

```shell
git clone --recurse-submodules git@github.com:hedera-dev/fork-hedera-testnet.git
```

### Copy Your ECDSA Hex Encoded Private Key

Grab your ECDSA hex encoded private key by logging into [Hedera Portal](https://portal.hedera.com).

If you need to create a Hedera account, follow the [create a hedera portal profile faucet](https://docs.hedera.com/hedera/getting-started/introduction#create-hedera-portal-profile-faucet) tutorial.


### Deploy your contract to Hedera Testnet

Replace "RPC_URL" with a tesnet URL by choosing on of the options over at [How to Connect to Hedera Networks Over RPC](https://docs.hedera.com/hedera/tutorials/more-tutorials/json-rpc-connections)

Next, copy the value of "Hex Encoded Private Key" from your `ECDSA` account and replace `"HEX_Encoded_Private_Key"`in the command below:

{% hint style="warning" %} Your hex encoded private key must be prefixed with `0x`. {% endhint %}

```shell
forge create --rpc-url "RPC_URL" --private-key "HEX_Encoded_Private_Key" src/TodoList.sol:TodoList
```

You should see output similar to the following:

```text
[⠒] Compiling...
[⠔] Compiling 22 files with 0.8.23
[⠑] Solc 0.8.23 finished in 3.44s
Compiler run successful!
No files changed, compilation skipped
Deployer: 0xdfAb7899aFaBd146732c84eD83250889C40d6A00
Deployed to: 0xc1E551Eb1B3430A8D373C43e8804561fca5ce90D
Transaction hash: 0x8709443db7b60df7b563c83514ce8b03e54c341a5fe9844e01c72b05fc50950e
```

Open the Hashscan link to your deployed contract by
copying the **Deployed to** EVM address and replacing <Deployed_Contract_EVM_Address> in the link below:

```text
https://hashscan.io/testnet/contract/<Deployed_Contract_EVM_Address>
```

{% hint style="success" %}
Example: https://hashscan.io/testnet/contract/0xc1E551Eb1B3430A8D373C43e8804561fca5ce90D
{% endhint %}


### Execute a contract call and create a new todo

Use Foundry's `cast send` command to sign and publish a transaction to Hedera Tesnet.

Replace `"deployed-contract-EVM-address"` with your deployed contracts EVM address and `"HEX_Encoded_Private_Key"` with your "Hex Encoded Private Key".

Next, replace "RPC_URL" with a tesnet URL.

```shell
cast send "deployed-contract-EVM-address" --private-key "HEX_Encoded_Private_Key" "createTodo(string)(uint256)" "Buy camping supplies"  --rpc-url "RPC_URL" 
```

You should see output similar to the following:

```text
blockHash               0x958f246cc010aa074c81eae1988abc16c16bc12d2246397320d163b5ea748a89
blockNumber             7261495
contractAddress         0xc1E551Eb1B3430A8D373C43e8804561fca5ce90D
cumulativeGasUsed       91838
effectiveGasPrice       1050000000000
gasUsed                 91838
logs                    []
logsBloom               0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
root                    
status                  1
transactionHash         0x18286c79ae735dd08fccbe676ee2d7335147d7a1a6c7f2d0e99099c2802c397c
transactionIndex        2
type                    2
```

Use `cast call` to execute `TodoList.sol`'s `numberOfTodos()`. You should see `numberOftodos` has incremented by 1. 

```shell
cast call "deployed-contract-EVM-address" "numberOfTodos()(uint256)" --rpc-url "RPC_URL"
```

{% hint style="warning" %} 

```cast send``` signs/publishes a transaction and changes the state.

```cast call``` performs a call without changing state. Essentially a read transaction.

{% endhint %}


### Write your test

An almost-complete test has already been prepared for you. It's located at `test/TodoList.t.sol`.
You will only need to make a few modifications (outlined below)
for it to run successfully.

{% hint style="warning" %}
Look for a comment in the code to locate the specific lines of code which you will need to edit. For example, in this step, look for this:
    `// Step (1) in the accompanying tutorial`
You will need to delete the inline comment that looks like this: `/* ... */`. Replace it with the correct code.
{% endhint %}

#### Step 1: Target your deployed contract

Replace "Deployed_Contract_EVM_Address" with your deployed contract's EVM address".

```solidity
    TodoList public todoList =
        TodoList("Deployed_Contract_EVM_Address");
```

#### Step 2: Testing CreateTodo()

Copy the below test and paste it in  `TodoList.t.sol`

```solidity
function test_createTodo_returnsNumberOfTodosIncrementedByOne() public {
    // get the current number of todos
    uint256 numberOfTodos = todoList.getNumberOfTodos();

    // create a new todo and save the number of todos
    uint256 todoCountAfterCreate = todoList.createTodo("A new todo for you!");

    assertEq(todoCountAfterCreate, (numberOfTodos + 1));
}
```

### Fork test Hedera Testnet and run your test

Using the `--fork-url` flag you will run your test against a forked Hedera Testnet environment at the latest block. 

Replace "RPC_URL" with a tesnet URL by choosing on of the options over at [How to Connect to Hedera Networks Over RPC](https://docs.hedera.com/hedera/tutorials/more-tutorials/json-rpc-connections)


```shell
forge test --fork-url "RPC_URL" -vvvv
```

You should see output similar to the following:

```text
[⠢] Compiling...
[⠰] Compiling 1 files with 0.8.23
[⠔] Solc 0.8.23 finished in 1.15s
Compiler run successful!

Running 1 test for test/TodoList.t.sol:TodoListTest
[PASS] test_createTodo_returnsNumberOfTodosIncrementedByOne() (gas: 59188)
Traces:
  [59188] TodoListTest::test_createTodo_returnsNumberOfTodosIncrementedByOne()
    ├─ [2325] 0xc1E551Eb1B3430A8D373C43e8804561fca5ce90D::getNumberOfTodos() [staticcall]
    │   └─ ← 1
    ├─ [51026] 0xc1E551Eb1B3430A8D373C43e8804561fca5ce90D::createTodo("A new todo for you!")
    │   └─ ← 2
    └─ ← ()

Test result: ok. 1 passed; 0 failed; 0 skipped; finished in 2.13s
 
Ran 1 test suites: 1 tests passed, 0 failed, 0 skipped (1 total tests)
```

Your output will show you the state of `numberOfTodos` before you created a new todo and after. It also shows whether the test passed, failed or was skipped.

{% hint style="info" %}
If you'd like to test a contract deployed on mainnet use a Mainnet RPC URL. Currently fork testing at the latest block is only supported. Be aware everytime you run your test it is against the latest state of the network.
{% endhint %}


## Complete

Congratulations, on completing the tutorial on how to fork Hedera Testnet on the latest block.

You have learned how to:
* [x] Deploy your smart contract to Hedera Testnet using `forge create`
* [x] Use `cast` command-line tool to execute the `createTodo` function in `TodoList.sol`
* [x] Fork Hedera Testnet on the latest block & run your tests against your deployed contract

***

<table data-card-size="large" data-view="cards"><thead><tr><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th>
<tr><td align="center"><p>Writer: Abi Castro, DevRel Engineer</p><p><a href="https://github.com/a-ridley">GitHub</a> | <a href="https://twitter.com/ridley___">Twitter</a></p></td><td><a href="https://twitter.com/ridley___">https://twitter.com/ridley___</a></td></tr>
</tbody></table>