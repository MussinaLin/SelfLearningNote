###### tags: `Mastering Ethereum`
# Chapter 5 Wallet
Wallet
- Non-deterministic wallet
- Deterministic wallet:most commonly used Hierarchical Deterministic Wallet(HD wallet)(BIP32/BIP44)

## HD Wallet

![](https://i.imgur.com/cLnTHjd.png)

* HD wallet - BIP32
* Mnemonic code words - BIP39
* Multipurpose HD wallet structure - BIP43
* Multicurrency and multiaccount wallet - BIP44

## Generate Mnemonic Words
start from an entropy --> add checksum --> maps to word list

1. Create a cryptographically random sequence s of 128 to 256 bits
2. Create a checksum of s by taking the first length of s / 32 bits of the SHA-256 hash of s
3. add the checksum to the end of the random sequence s
4. Divide the sequence-and-checksum into sections of 11 bits
5. Map each 11-bit value to a word from the predefined dictionary of 2048 words
6. create the mnemonic word, maintaining the order

![](https://i.imgur.com/Drn6R77.png)

![](https://i.imgur.com/buZ644W.png)

### From Mnemonic to Seed
The entropy is then used to drive a 512-bit seed through key-stretching function PBKDF2

Seed = HMAC-SHA512(mnemonic, salt) ---> 2048 times ---> 512-bit value is the seed

![](https://i.imgur.com/On8W0SF.png)


## HD Wallet(BIP-32) and Paths(BIP-43/44)

### Extended public and private keys
In BIP-32 terminology, keys can be "extended", these extended "parent" keys can be used to drive "child" keys.
Extended key involves taking the key itself and appending a special chain code to it. 
A chain code is a **256-bit** binary string that is mixed with each key to produce child keys.

private key ---> extended private key, prefix **xprv**
public key ---> extended public key, prefix **xpub**

Risk: Because the xpub contains the chain code, if a child private key is leaked, it can be used with the chain code to derive all the other child private keys. Worse, the child private key together with a parent chain code can be used to deduce the parent private key.

### Hardened child key derivation

**Breaks** the relationship between parent public key and child chain code

keys derived through normal(unhardened) derivation function, index number between 0 and 2^31 - 1(0x0~0x7fffffff)

hardened derivation fuction index number between 2^31~2^32 - 1(0x80000000~0xffffffff)

-----> make it easy !
First normal child key is display as 0
First hardened child key is display as 0'
so, when you see 1' means 2^31 + 1

### HD wallet tree structure
BIP-43: purpose the use of first hardened child index as a special identifier. 
(就是用第一個 child index 來決定用途)

BIP-44: 延續BIP-43, proposes a multicurrency multiaccount structure signified by setting the **purpose** number to 44'. All HD wallets following the BIP-44 structure, identified by the branch m/44'/*

BIP-44 specifies the structure:
m/purpose'/coin_type'/account'/change/address_index

![](https://i.imgur.com/r1EpZri.png)











