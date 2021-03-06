
wecryptojs
=========

A small vendoring wrapper for [sjcl](http://bitwiseshiftleft.github.io/sjcl/) with support for
hashes and encodings required by the WeYouMe platform.

## Usage

If you are using Webpack or Browserify, you must ensure that Node's built-in `crypto` package
is excluded from your builds. 

Otherwise, just
```sh
$ yarn install wecryptojs
```

## API

```
> wecryptojs = require('wecryptojs');
```

### wecryptojs.sha256(data)

Hashes the content of an `ArrayBuffer` using SHA-256.

```
> shaHash = wecryptojs.sha256(new Uint8Array().buffer)
ArrayBuffer { byteLength: 32 }
> wecryptojs.hexify(shaHash)
'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855'
```

### wecryptojs.ripemd160(data) 

Hashes the content of an `ArrayBuffer` using RIPEMD-160.

```
> ripemdHash = wecryptojs.ripemd160(new Uint8Array().buffer)
ArrayBuffer { byteLength: 20 }e
> wecryptojs.hexify(ripemdHash)
'9c1185a5c5e9fc54612808977ee8f548b2258d31'
```

### wecryptojs.PrivateKey

Provides operations over WeYouMe secp256k1-based ECC private keys.
```
> secretKey = wecryptojs.PrivateKey.from('5JCDRqLdyX4W7tscyzyxav8EaqABSVAWLvfi7rdqMKJneqqwQGt')
PrivateKey { getPublicKey: [Function], sign: [Function] }
> secretKey.getPublicKey().toString()
'TME5pZ15FDVAvNKW3saTJchWmSSmYtEvA6aKiXwDtCq2JRZV9KtR9'
> secretSig = secretKey.sign(new Uint8Array(32).buffer)
ArrayBuffer { byteLength: 65 }
> wecryptojs.hexify(secretSig)
'20387d5f9ae215a64065fde2a9d4f7be83d3480b7cc89f7c01488042da348845408909e9d4f1d66466c53f0007c771a73bf2883d8d5ab4735b5b4316091361442c'
```

### wecryptojs.PublicKey

Provides operations over WeYouMe secp256k1-based ECC public keys.
```
> publicKey = wecryptojs.PublicKey.from('TME5SKxjN1YdrFLgoPcp9KteUmNVdgE8DpTPC9sF6jbjVqP9d2Utq')
... 
> publicKey.verify(new Uint8Array(32).buffer, secretSig)
true
> PublicKey.recover(someHash, someSig)
...
```

### wecryptojs.generateKeys()

Generates a new pair of keys in WeYouMe WIF format using cryptographically secure
random number generation.
```
> wecryptojs.generateKeys()
{
  private: "5JCDRqLdyX4W7tscyzyxav8EaqABSVAWLvfi7rdqMKJneqqwQGt",
  public: "TME5pZ15FDVAvNKW3saTJchWmSSmYtEvA6aKiXwDtCq2JRZV9KtR9"
}
```

### wecryptojs.keysFromPassword(accountName, accountPassword)

Given an account name and password, regenerates the derived `owner`, `posting`,
`active`, and `memo` keys.
```
> wecryptojs.keysFromPassword('username', 'password')
{ owner:
   { private: '5JCDRqLdyX4W7tscyzyxav8EaqABSVAWLvfi7rdqMKJneqqwQGt',
     public: 'TME5pZ15FDVAvNKW3saTJchWmSSmYtEvA6aKiXwDtCq2JRZV9KtR9' },
  memo:
   { private: '5JSmQQJXH5ZrSW3KJSTUPFJy7SuLeDiY3bW6vB1McamxzJQFhwD',
     public: 'TME5nwJgD9jmkAdTXuiz3jqrkw3om95gCapZo4e4Bcp3qzyiedwCn' },
  posting:
   { private: '5HsoxWiHRRyx6oSxKj32HDqDMzSGhs79zLZopDc7nMcjMbcPp5E',
     public: 'TME6gZmazY23TEMkxmPpnmvbAgWFAzwtaSDbhSUdmpTXzoJJLPFH4' },
  active:
   { private: '5JamTPvZyQsHf8c2pbN92F1gUY3sJkpW3ZJFzdmfbAJPAXT5aw3',
     public: 'TME5SKxjN1YdrFLgoPcp9KteUmNVdgE8DpTPC9sF6jbjVqP9d2Utq' } }
```

## Requirements

wecryptojs is written in Javascript as specified by 
[ECMA-262, version 5.1](https://www.ecma-international.org/ecma-262/5.1/).
Other than its vendored copy of sjcl, it has no dependencies and never will.

wecryptojs explicitly supports the following environments without polyfills:
- [Node.js](https://nodejs.com) versions 4 and up
- Microsoft Edge (all versions)
- Safari for macOS versions 7.1+
- Safari for iOS versions 8+
- Firefox, Chrome, and Opera versions 30+

wecryptojs explicitly does not support the following environments:
- Opera Mini
- Android Browser (i.e., the non-Chromium versions)
- Microsoft Internet Explorer versions <10

## Contributing

Contributions must conform to the following rules:
- They must pass formatting and linting and the existing automated test suite must pass.
- They must add test coverage for new code.
- They must not introduce any new dependencies.
- They must support the environments listed above without the use of polyfills.
