<img src="preview/Logo.png" align="left" />

# SafeExchange

[![forthebadge](https://forthebadge.com/images/badges/made-with-javascript.svg)](https://forthebadge.com)

SafeExchange is a crypto library for key-exchange, encryption/decryption and digital signature signing  
The library allows users to easily perform a [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) key exchange. It utilizes the [aes-lib-js](https://github.com/kyleruss/aes-lib-js) library for  
AES encryption/decryption and automatically signs encrypted messages and verifies their digital signatures
To see how SafeExchange can be used see the details below or checkout [ByteChat](https://github.com/kyleruss/byte-chat) for a project using SafeExchange

## Usage
#### Creating a Public Key
First you need to generate a _secret_ which you will keep private  
The secret value is then used to create the public key which you may distribute

```
var secret    = SafeExchange.generateSecretInt();
var publicKey = SafeExchange.generatePublicKey(secret);
```

#### Creating a Private Key
When you receive a public key from another user you will need to use their public key in combination with your generated secret to create a mutual private key that only the two of you know its value  
First generate your secret & public key as shown above then create the private key  
Once you've created your private key, send them your public key and they will do the same to create a private key, the two keys should match

```
//Generate your own secret & public key

var friendPublicKey = data.publicKey;
var privateKey      = SafeExchange.generatePrivateKey(friendPublicKey, secret);

//Send the friend your public key and they will create their own private key which should match yours
```

#### Creating an IV
You can also create an IV (Initialization Vector) from your private key
This IV will be required when encrypting/decrypting messages
```
var iv  = SafeExchange.generateIV(privateKey);
```

#### Encrypt Message & Digital Signature
The following function can be used to encrypt your message and generate a digital signature  
The digital signature should be attached with your encrypted message so that the user can verify the authenticity of the message

```
var msgData = SafeExchange.signMessage(message, pkey, iv);
//Returns { msg: Encrypted message, hash: Encrypted hash value of the message }
```

#### Decrypt Message & Verify 
The following function can be used to decrypt a received message and verify its digital signature  
If the hashes don't match then it could be the message is not genuine and the function will return null otherwise it will return the decrypted message

```
var message = SafeExchange.releaseMessage(encMessage, encHash, privateKey, iv);
```

#### Data Accessability
The following data can be distributed publicly:
- Public Key

The following data must not disclosed:
- Private key
- Secret 
- IV

## Getting started

### Prerequisites
- [aes-lib-js](https://github.com/kyleruss/aes-lib-js)
- [BigInt.js](http://www.onicos.com/staff/iz/amuse/javascript/expert/BigInt.txt) & [YaMD5](https://github.com/gorhill/yamd5.js/) (Both are included)

### Installation
- Clone the SafeExchange repository
```
git clone https://github.com/kyleruss/safe-exchange.git
```

- Move `SafeExchange.js` and scripts in the `dependencies` directory into your project
- Include the SafeExchange library and its dependencies in your project
**Note:** SafeExchange also requires [aes-lib-js](https://github.com/kyleruss/aes-lib-js) which needs to be loaded before SafeExchange 
```
<!-- Include the aes-lib-js scripts here -->
<script src="depdencies/yamd5.min.js"></script>
<script src="dependencies/BigInt.js"></script>
<script src="SafeExchange.js"></script>
```


## License
SafeExchange is available under the MIT License  
See [LICENSE](LICENSE) for more details
