---
title: AES Crypto Service Provider
description: 
published: true
date: 2021-02-22T08:23:53.282Z
tags: 
editor: markdown
dateCreated: 2021-02-22T08:23:53.282Z
---

AESCSP is using for aes encryption. Aes is symetric encryption, is using only one key for encrypt/decrypt data. For more security it can be used together with asymetric encryption.

### Intializing provider

```php
use MayMeow\Cryptography\AES\AESCryptoServiceProvider;

//initialize CSP, generate key and IV
$csp = new AESCryptoServiceProvider();
$csp->generateIV();
$key = $csp->generateKey();
```

### Text encryption

```php
//encrypt data
$plainText = "This is going to be encrypted!";
$encryptedText = $this->csp->encrypt($plainText);
```

### Text decryption

Sure you can use same instance to decrypt but in most cases you only encrypting and then storing data to database to decrypt it later.

```php
// inistialize another CSP
$csp2 = new AESCryptoServiceProvider();
$csp2->setKey($key); // et key you generated before

//decrypt text
$originalText = $csp2->decrypt($encryptedText);
```