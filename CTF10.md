# CTF10 - Weak Encryption

In this Capture The Flag (CTF), we were presented to an encryption algorithm `cipherspec.py` (Appendix A):

This script has a function `gen()`, which is responsible for generating a 16 bytes random flag. The first 13 bytes of the flag are all 0's and the last 3 are the ones that are randomized:

```py
def gen(): 
    offset = 3 # Hotfix to make Crypto blazing fast!!
    key = bytearray(b'\x00'*(KEYLEN-offset)) 
    key.extend(os.urandom(offset))
    return bytes(key)
```

This script also has another function `enc(k, m, nonce)` which receives the key `k`, the plain text message `m`, and the `nonce` parameter. It is responsible for the randomness of the cipher to transform it into a non-deterministic encryption algorithm. It creates the cipher with the AES-CTR algorithm:

```py
def enc(k, m, nonce):
	cipher = Cipher(algorithms.AES(k), modes.CTR(nonce))
	encryptor = cipher.encryptor()
	cph = b""
	cph += encryptor.update(m)
	cph += encryptor.finalize()
	return cph
```

On the other hand, the function `dec(k, c, nonce)` does the exact opposite of the function `enc(k, m, nonce)`. It takes the key (`k`), the encrypted message (`c`), and the `nonce` parameters and decrypts the cipher back to the original message:

```py
def dec(k, c, nonce):
	cipher = Cipher(algorithms.AES(k), modes.CTR(nonce))
	decryptor = cipher.decryptor()
	msg = b""
	msg += decryptor.update(c)
	msg += decryptor.finalize()
	return msg
```

This algorithm is considered efficient by using the function gen() which generates the key.

This approach will create the key to make the encryption. This key is formed by 16 bytes which would create a total of 2^128 different approaches. For combinations, a simple computer would not be able to solve this problem (something like 25 times the age of the universe). The problem is that, this algorithm (to make it faster), is considering that the first 13 bytes of the key are all 0's. For that, only the last 3 bytes are varying. With that, the number of configurations is 2^24 only, which makes it more easy to break this algorithm in a useful time. With that, a brute force approach is sufficient to break the algorithm:

```py

def bruteForce(ciphertext, nonce):

    for i in range(2**24):
        CTFKey = i.to_bytes(KEYLEN, byteorder='big')

        decifredMessage = dec(CTFKey, ciphertext, nonce)

        if b"flag{" in decifredMessage or b'flag{' in decifredMessage:
            print(f"Found the flag: {decifredMessage}")
            break
```

## The main problem of this approach:

1. One of the problems of this algorithm is (as said before) the reduced number of keys, which makes a brute force approach relatively easy to find the flag.

2. The nonce is given to the user, which also makes it easier to reach the flag.


## Appendix A - Source Code

```python
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
import os

KEYLEN = 16

def gen(): 
	offset = 3 # Hotfix to make Crypto blazing fast!!
	key = bytearray(b'\x00'*(KEYLEN-offset)) 
	key.extend(os.urandom(offset))
	return bytes(key)

def enc(k, m, nonce):
	cipher = Cipher(algorithms.AES(k), modes.CTR(nonce))
	encryptor = cipher.encryptor()
	cph = b""
	cph += encryptor.update(m)
	cph += encryptor.finalize()
	return cph

def dec(k, c, nonce):
	cipher = Cipher(algorithms.AES(k), modes.CTR(nonce))
	decryptor = cipher.decryptor()
	msg = b""
	msg += decryptor.update(c)
	msg += decryptor.finalize()
	return msg
```
