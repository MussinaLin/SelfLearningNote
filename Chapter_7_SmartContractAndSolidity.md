###### tags: `Mastering Ethereum`
# Chapter 7 Smart Contract and Solidity
每一個Contract 也有一個 Ethereum address, contract address 是用 originating account 跟 nonce 計算出來的.

Contract 可以 call another Contract.

可以把 EVM 視為 Single thread 的 machine.

To delete Contract, we can execute an EVM opcode call **SELFDESTRUCT**

## The Ethereum Contract ABI
An *application binary interface* is an interface between two module; often between operating system and user program. An ABI defines how data structure and functions are accessed in machine code. not to be confused with API, which defines this access in high-level.

In Ethereum, ABI 定義了 Contract 裡面可以被呼叫的function, 還有描述 function 的傳入參數以及回傳值.

Contract ABI 是 JSON 格式, 包含 function description and events.

Function description 是一個 json object 包含: type, name, inputs, outputs, constant, payable.

Event description 是一個 json object 包含: type, name, inputs, anonymous


