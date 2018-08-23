% Extra blob functions

Introduction
============

This Sqlite3 extension module adds assorted functions for working with
BLOB types (And strings). Requires OpenSSL.

Largely influenced by MySQL functions in features and names.

Scalar Functions
================

Formatting
----------

### UNHEX()

* UNHEX(str)

Does the opposite of the built in `HEX()` function - given a string
that's Base16 encoded, returns the decoded BLOB. Returns `NULL` if
the string is not Base16 encoded or isn't an even length, or if
passed a `NULL`.

### TO_BASE64()

* TO_BASE64(blob)

Returns `blob` encoded as a Base64 string. To match MySQL, the result
is broken up into multiple lines if long enough.

### FROM_BASE64()

* FROM_BASE64(str)

Returns `str` decoded from Base64 into a BLOB. If the argument is
`NULL` or an invalid Base64 string, returns `NULL`.

Message Digests
---------------

If any of these functions are passed a `NULL` argument, return `NULL`.

### MD5() ###

* MD5(b)

Returns the MD5 digest of its blob argument as a Base16 encoded string.

### SHA1() ####

* SHA1(b)

Returns the SHA1 digest of its blob argument as a Base16 encoded string.

### SHA2() ####

* SHA2(b, i)

Returns the `i`-bit SHA2 digest of its blob argument as a Base16 encoded
string. `i` can be 224, 256, 384, 512, or 0 (Which is treated as 256).

### CREATE_DIGEST()

* CREATE_DIGEST(algo, b)

Returns the `algo` digest of `b` as a blob. `algo` can be any message
digest supported by OpenSSL, including but not limited to, 'md5', 'rmd160',
'sha1', 'sha256', 'sha512', etc.

You can get the complete list with `openssl list --digest-commands` at
a shell.

### HMAC()

* HMAC(algo, secret, b)

Returns the HMAC of `b`, using the `algo` message digest algorithm,
and secret `secret` as a Base16 encoded string.

### CRC32()

* CRC32(b)

Computes the [CRC-32] checksum of its blob argument and returns the
result as an integer.

[CRC-32]: https://en.wikipedia.org/wiki/Cyclic_redundancy_check

### CRC32C()

* CRC32C(b)

Computes the CRC-32C checksum of its blob argument and returns the
result as an integer. Currently only available on x86 processors.

Encryption
----------

### AES_ENCRYPT()

* AES_ENCRYPT(str, key)

Returns a BLOB of `str` encrypted using the **AES-128-ECB** algorithm
with key `key`. The key should be a 128 bit (16 byte) or larger blob
or string. If longer, only the first 128 bits are used, but this makes
it easier to use a digest of the actual password as the encryption
key.

### AES_DECRYPT()

* AES_DECRYPT(aes, key)

Returns the decrypted `aes`, which is a BLOB holding padded,
**AES-128-ECB** encrypted data.

UUIDs
-----

### UUID()

* UUID()

Generate a new type 4 (Random) UUID and return it as a blob.

### BIN_TO_UUID()

* BIN\_TO\_UUID(b)

Convert a UUID blob to a string representation.

### UUID_TO_BIN()

* UUID\_TO\_BIN(s)

Convert a UUID string to a blob representation.

Aggregate Functions
===================

TODO: Aggregate versions of the digest functions?