###### tags: `Mastering Ethereum`
# Chapter 4 Cryptography
Ethereum use **public key cryptography(asymmetric cryptography)** to create public-private key pair.

## Private Keys
The Ethereum private key is **just a number**
- 256 bits (32 bytes, 64 hex)
- between 1 and 2^256
- usually use Keccak-256 or SHA-256 to produce a 256-bit number

## Public Keys
An Ethereum public key is a point on an elliptic curve.
***K = k * G***
K:public key
k:private key
G:generator point(constant point)

Ethereum uses the same elliptic curve, called *secp256k1*, as Bitcoin.

y ** 2 mod p = (x ** 3 + 7) mod p

![](https://i.imgur.com/ACr3Bzn.png)

k * P = P + P + P + .... + P(k times)

public key K = (x,y)  04 + x-coordinate + y-coordinate 
- 65 bytes(32+32+1)
- with prefix 0x04

Elliptic Curve Libraries
- OpenSSL
- libsecp256k1(performance and security are better)

## Hash Function
Ethereum's hash function : Keccak-256

## Ethereum Address

private key ---> public key ---> address

Keccak256(K) = ..... keep only the last 20 bytes is Ethereum address

## EIP55 - Hex Encoding with Checksum in Capitalization
1. Hash the lowercase address without 0x prefix:
Keccak256("...")

2. Capitalize each alphabetic address character if the corresponding hex digit of the hash is greater than or equal to 0x8


