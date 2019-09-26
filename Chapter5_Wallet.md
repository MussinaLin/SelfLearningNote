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
