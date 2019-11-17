###### tags: `Mastering Ethereum`
# Chapter 9 Smart Contract Security
## Security Best Practices
- Minimalism/simplicity
- Code reuse
- Code quality
- Readability
- Test coverage

## Security Risks
### Reentrancy

![](https://i.imgur.com/G35KOGK.png)

msg.sender.call invoke malicious contract's fallback function. Repeat until it's no longer have balance in Bank.

#### Preventative Techniques
1. use ***send*** or ***transfer*** instead of ***call*** to transfer money. ***send*** and ***transfer*** functions are considered safer because they are limited to 2,300 gas. The gas limit prevents the expensive external function calls back to the target contract.
2. update balance 的logic operation 在轉錢之前執行
3. 用 mutex 鎖住
simple example
![](https://i.imgur.com/WdfIQjH.png)

OpenZeppelin has it’s own mutex implementation you can use called **ReentrancyGuard**. This library provides a modifier you can apply to any function called **nonReentrant** that guards the function with a mutex.

View the source code for the OpenZeppelin ReentrancyGuard library here: https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/utils/ReentrancyGuard.sol