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


## Transaction Signing in Practice

sign a transaction flow:
1. Create transaction data structure, containing nine fields: nonce, gasPrice, gasLimit, to, value, data, chainID,0,0
2. Produce an RLP-encoded serialized message of the transaction data structure
3. Compute the Keccak-256 hash of this serialized message
4. Compute the ECDSA signature, signing the hash with EOA's private key
5. Append signature's computed v,r,s value to the transaction

v indicates two things: the chainID and the recovery identifier to help ECDSArecover function check the signature.


## EIP-155
EIP-155 **Simple Replay Attack Protection** specifies a replay-attack-protected transaction encoding, which includes a chain identifier inside the transaction data, prior to signing. Ensure that transaction created for one blockchain(e.g. Ethereum main network) are invalid on another blockchain(e.g. Ethereum Classic)

EIP-155 **adds three field to the main six fields of the transaction data structure, namely chainID, 0, 0**

![](https://i.imgur.com/Wcbjbec.png)

## The Signature Prefix Value(v) and Public Key Recovery
First, you find the two points 𝑅, 𝑅′ which have the value 𝑟 as the x-coordinate 𝑟.

You also compute 𝑟−1, which is the multiplicative inverse of the value 𝑟 from the signature (modulo the order of the generator of the curve).

Then, you compute 𝑧 which is the lowest 𝑛 bits of the hash of the message (where 𝑛 is the bit size of the curve).

Then, the two public keys are 𝑟−1(𝑠𝑅−𝑧𝐺) and 𝑟−1(𝑠𝑅′−𝑧𝐺)

To make things more efficient, transaction signature includes a prefix value v, which tell us which of the two possible R value value is the temp public key. If v is even, then R is correct value. If v is odd, then it is R'. That way, we only need to calculate one value for R.