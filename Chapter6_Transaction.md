###### tags: `Mastering Ethereum`
# Chapter 6 Transactions
## The Structure of a Transaction
A transaction is a serialized binary message that contains the following data:

***Nonce***: A sequence number, used to prevent message replay
***Gas price***: The price of gas(wei)
***Gas Limit***: The maximum amount of gas the originator is willing to buy for this transaction
***Recipent***: The destination Ethereum address
***Value***: The amount of ether to send to destination
***Data***: The variable-length binary data payload
***v,r,s***: The three component of an ECDSA digital signature of the originating EOA

The transaction message's structure is serialized using Recursive Length Prefix(RLP) encoding scheme

### The Transaction Nonce
動態計算出來的,計算一個 EOA 被 confirm 的 transaction 有幾個

Transaction執行順序
nonce:
0--->1--->2
0--->?--->2(pending, 等待nonce 1 的tx出現)

### Transmitting a Data Payload

payload:
***A function selector***: The first 4 bytes of the Keccak-256 hash of the function prototype.
***The function arguments***: encoded according to the rules for the various elementary types.

example: 
function withdraw(uint amount) public {}

function selector is Keccak256("withdraw(uint)") ---> take first 4 bytes ---> 0x2e1a7d4d

if amount = 0.01 ether, arguments = toHex(10000000000000000) ---> 0x2386f26fc10000 


Now add function selector and argument(padded to 32 bytes 
)---> 2e1a7d4d000000........00002386f26fc10000


## The Elliptic Curve Digital Signature Algorithm
數位簽章功用：
1. 證明誰有權花這筆錢 or 執行contract
2. 證明這筆交易不可被否認
3. tx 一但被sign 之後, tx的內容沒有被更改


Digital Signatures Work:
1. Use private key + tx for signing
2. let others use public key to verify the signature

Sig = Fsig(Keccak256(m), k)
k: signing private key
m: RLP-encoded transaction
Fsig: signing algorithm
Sig: resulting signature

Sig is composed of two values, commonly refered to as r and s: Sig = (r,s)

### ECDSA Math

1. 產生 temp private key
2. 產生 temp public key
3. R = public key 的 x 座標

![](https://i.imgur.com/lI22jKx.png)

Note here the k^-1 which is the ‘modular multiplicative inverse‘ of k… it’s basically the inverse of k, but since we are dealing with integer numbers, then that’s not possible, so it’s a number such that (k^-1 * k ) mod p is equal to 1. And again, I remind you that k is the random number used to generate R, z is the hash of the message to sign, dA is the private key and R is the x coordinate of k*G (where G is the point of origin of the curve parameters).

Now that you have your signature, you want to verify it, it’s also quite simple, and you only need the public key (and curve parameters of course) to do that. You use this equation to calculate a point P, We set ‘dA‘ as the private key (random number) and ‘Qa‘ as the public key (a point), so we have : Qa = dA * G (where G is the point of reference in the curve parameters).

![](https://i.imgur.com/mYlmgYe.png)


for more detail: 
https://www.instructables.com/id/Understanding-how-ECDSA-protects-your-data/

or search ECDSA explained
