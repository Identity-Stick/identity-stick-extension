# Protocol for the identity-stick extension to the FIDO2 standard.
This is the protocol used for the identity-stick extension. It enables a fido token to send signed attributes about the owner of the device.

## Involved parties
For the concern of this protocol three parties are involved:
1. The signing authority: provides proof of the 
2. The relying party: 
3. The authenticator 

## Signing the attributes
The value of the attributes must be hashed using SHA-256. This value must then be signed using RSA with a private key of length 4096 bit. The corresponding public key should be made public using a trusted PKI. 

## Transfer of attributes
- Check what attributes are there
- Request certain attribute
- 

### Type of messages
- GetInfo
- makeCred
- GetAssert
	- Info message
	- Data message

## Checking validity of attributes
- calculating hash
- decrypt hash value
- comparing calculated hash with the decrypted one
