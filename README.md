EllipticCurveKeyPair
====================

Sign, verify, encrypt and decrypt using the Secure Enclave.

## Features

- create a private public keypair
- store the private key on the secure enclave
- store the public key in keychain
- each time you use the private key the user will be prompted with touch id or device id in order to use it
- export the public key as X.509 DER with proper ASN.1 header / structure
- verify the signature with openssl in command line easily


## Verifying a signature

In the demo app you'll see that each time you create a signature some useful information is logged to console.

Example

```sh
#! /bin/sh
echo 414243 | xxd -r -p > dataToSign.dat
echo 3046022100842512baa16a3ec9b977d4456923319442342e3fdae54f2456af0b7b8a09786b022100a1b8d762b6cb3d85b16f6b07d06d2815cb0663e067e0b2f9a9c9293bde8953bb | xxd -r -p > signature.dat
cat > key.pem <<EOF
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEdDONNkwaP8OhqFTmjLxVcByyPa19
ifY2IVDinFei3SvCBv8fgY8AU+Fm5oODksseV0sd4Zy/biSf6AMr0HqHcw==
-----END PUBLIC KEY-----
EOF
/usr/local/opt/openssl/bin/openssl dgst -sha256 -verify key.pem -signature signature.dat dataToSign.dat
```

In order to run this script you can
1. paste it in to a file: `verify.sh`
1. make the file executable: `chmod u+x verify.sh`
1. run it: `./verify.sh`

Then you should see
```sh
Verified OK
```

PS: This script will create 4 files in your current directory.

## Examples

There's a lot of great possibilities with Secure Enclave. Here's some examples

### Encrypting

1. Encrypt a message using the public key
1. Decrypt the message using the privat key – only accessible with touch id / device pin

A use case could be

1. Create the keypair
1. Send the public key to server
1. Encrypt a message using the public key on server
1. Decrypt that message on device using the privat key – only accessible with touch id / device pin


### Signing

1. Sign some data received by server using the privat key – only accessible with touch id / device pin
1. Verify that the signature is valid using the public key

A use case could be

1. User is requesting a new agreement / purchase
1. Server sends a push with a session token that should be signed
1. On device we sign the session token using the private key - prompting the user to confirm with touch id
1. The signed token is then sent to server
1. Server already is in posession of the public key and verifies the signature using the public key
1. Server is now confident that user signed this agreement with touch id


## Keywords
Security framework, Swift 3, Swift, SecKeyRawVerify, SecKeyGeneratePair, SecItemCopyMatching

## Feedback

We would 😍 to hear your opinion about this library. Wether you like or don't. Please file an issue if there's something you would like like improved. Reach me on twitter as [@hfossli](https://twitter.com/hfossli).

[<img src="http://static.agens.no/images/agens_logo_w_slogan_avenir_medium.png" width="340" />](http://agens.no/)
