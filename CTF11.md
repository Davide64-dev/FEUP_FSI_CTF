# CTF11 - RSA

The central focus of this CTF revolves around delving into the cipher mechanism of RSA and its susceptibility to vulnerabilities, particularly when dealing with prime numbers, especially when knowledge is available regarding the factorization of the values involved.


## Challenge

In this challenge, we were granted access to a server that transmitted an RSA-encoded flag (flag{xxxxxxxx}), along with details about the RSA encryption, including the module (n) and a public exponent (e), as we can see in the image below.

(**print 1**)

Furthermore, we were informed that the module was generated using a pair of prime numbers. Specifically, we were given the following information:
- **p** is a prime number close to 2⁵¹²
- **q** is a prime number close to 2⁵¹³

Additionally, we were provided with the ``` challenge.py ``` file, which proved instrumental in comprehending the conversion process from the flag to its numeric representation, as well as the decoding algorithm that reversed this process.

``` python
# Python Module ciphersuite
import os
import sys
from rsa_utils import getParams
from binascii import hexlify, unhexlify

FLAG_FILE = '/flags/flag.txt'

def enc(x, e, n):
    int_x = int.from_bytes(x, "little")
    y = pow(int_x,e,n)
    return hexlify(y.to_bytes(256, 'little'))

def dec(y, d, n):
    int_y = int.from_bytes(unhexlify(y), "little")
    x = pow(int_y,d,n)
    return x.to_bytes(256, 'little')

with open(FLAG_FILE, 'r') as fd:
	un_flag = fd.read()

(p, q, n, phi, e, d) = getParams()

print("Public parameters -- \ne: ", e, "\nn: ", n)
print("ciphertext:", hexlify(enc(un_flag.encode(), e, n)).decode())
sys.stdout.flush()
```


Initially, our task involved implementing a function to test whether a number is prime, a crucial step in capturing the flag. To accomplish this, we opted to implement the Miller-Rabin algorithm, as was sugested, which is presented below (*primes.py*).

``` python
import random

def miller_rabin(n, k=5):
    """
    Miller-Rabin primality test.

    :param n: Number to test for primality.
    :param k: Number of iterations for the test. Higher k means higher accuracy.
    :return: True if n is probably prime, False if n is composite.
    """
    if n <= 1:
        return False
    if n == 2 or n == 3:
        return True
    if n % 2 == 0:
        return False

    # Write n as 2^r * d + 1
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2

    # Witness loop
    for _ in range(k):
        a = random.randint(2, n - 2)
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False  # n is definitely composite

    return True  # n is probably prime
```

Subsequently, armed with the implemented prime-checking code, we proceeded to determine the values of **p** and **q**. All that remained was to ascertain the value of **d**.

To uncover this, we leveraged the fact that *ed % (p-1)(q-1) = 1*. Consequently, we calculated the value of **phi**, ensuring it equaled *(p-1) * (q-1)*, representing the count of positive integers coprime to *n = p * q*. This led us to the conclusion that *e * d ≡ 1 (mod phi)*. To guarantee the existence of a modular inverse for **e**, the greatest common divisor (gcd) of **e** and **phi** had to be 1.

``` python
def extended_gcd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, x, y = extended_gcd(b % a, a)
        return (g, y - (b // a) * x, x)


phi = (p - 1)*(q - 1)
d = extended_gcd(e, phi)[1]
```

