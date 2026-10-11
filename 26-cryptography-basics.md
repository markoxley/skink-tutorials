# Tutorial 26: Cryptography basics

This tutorial covers hashing with `std/crypto`.

## Hashing strings

`std/crypto` provides `MD5`, `SHA1`, and `SHA256` wrappers that return raw bytes, plus hex helpers for display.

```skink
module main

import "std/crypto"
import "std/str"

fn main() -> int {
    secret := "password123"
    md5 := crypto.Md5Hex(secret)
    sha256 := crypto.Sha256Hex(secret)

    print("MD5:    ", md5)
    print("SHA256: ", sha256)
    return 0
}
```

## HMAC

HMAC combines a secret key with a message digest. It is useful for authenticating messages:

```skink
key := "my-secret-key"
message := "transfer $100"
mac := crypto.HmacSha256(key, message)
macHex := crypto.HexEncode(mac, 32)
print("HMAC: ", macHex)
```

## What not to do

- `MD5` and `SHA1` are not safe for password storage. They are fine for checksums.
- `SHA256` is still not appropriate for passwords without salting.
- Use HMAC when you need to verify message integrity with a secret key.

## Exercises

1. Write a program that computes the SHA256 hash of a file and prints it.
2. Compare the HMAC of a message using two different keys.
3. Verify that changing one character in the input changes the hash.

## Next

[Tensors and math](27-tensors-and-math.md)
