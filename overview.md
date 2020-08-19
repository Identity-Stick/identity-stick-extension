# Protocol for the identity-stick extension to the FIDO2 standard.
This is the protocol used for the identity-stick extension. It enables a fido token to send signed attributes about the owner of the device.

## Involved parties
For the concern of this protocol three parties are involved:
1. The signing authority: provides proof of the 
2. The relying party: 
3. The authenticator 

## Signing the attributes
The value of the attributes must be hashed using SHA-256. This value must then be signed using RSA with a private key of length 4096 bit. The corresponding public key should be made public using a trusted PKI. All information necessary for the relying party to retrieve the key must be provided by the authenticator together with the list of available attributes.

## Transfer of attributes
Attributes must be send together with their signed hash. Transfers may be send as a whole or split. 
A relying party, that wants to receive attributes via the identity-stick extension, will need to perform three steps:

1. Request list of available attributes + infomration about public key
2. Request certain attribute
3. Request next part of attribute

For the first two tasks the authenticator must check for user presence and the relying party therefore must set the option accordingly. The request of the next part of an attribute should not ask for user presence as this would dramatically increase friction and therefore reduce the usability of the system.

## Split transfers
If messages are split, the authenticator must send the number of splits to the relying party as a response to the request of an attribute. A messages may be split into any number of seperate parts, but each part must be at least the size of one character.

## List of available attribues
The authenticator must include all available attributes in the send list of available attributes. This list must be sent as JSON. The list must be named 'available-data'. The source to obtain the public key of the signature must be included with the name 'pub-key'. If any other options for the PKI has to be sent, it may be encoded with 'pub-key-options'.
The listed attributes must confirm to the naming conventions specified in [here](attribute_names.md). The attributes must be listed in an array. Attributes, which can be send send seperately, must be listed seperately. If attributes can be send in combination, with others, they must be listed in a subarray.

### Type of messages
- GetInfo
- makeCred
- GetAssert
	- Info message
	- Data message

## Checking validity of attributes
To check the validity of received attributes, the relying party must obtain the public key of the siging authority. It must then calculate the hash of the received attributes using SHA-256, decrypt the received hash values and compare them to assert validity. It may reject attributes coming from untrusted sigin authorities.