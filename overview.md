# Protocol for the identity-stick extension to the FIDO2 standard.
This is the protocol used for the identity-stick extension. It enables a fido token to send signed attributes about the owner of the device.

## Involved parties
For the concern of this protocol three parties are involved:
1. The signing authority: provides proof of the 
2. The relying party: 
3. The authenticator 

## Signing the attributes
The value of the attributes MUST be hashed using SHA-256. This value MUST then be signed using RSA with a private key of length 4096 bit. The corresponding public key SHOULD be made public using a trusted PKI. All information necessary for the relying party to retrieve the key MUST be provided by the authenticator together with the list of available attributes.

## Transfer of attributes
Attributes MUST be send together with their signed hash. Transfers may be send as a whole or split. 
A relying party, that wants to receive attributes via the identity-stick extension, will need to perform three steps:

1. Request list of available attributes + information about public key
2. Request certain attribute
3. Request next part of attribute

For the first two tasks the authenticator MUST check for user presence and the relying party therefore MUST set the option accordingly. The request of the next part of an attribute SHOULD NOT ask for user presence as this would dramatically increase friction and therefore reduce the usability of the system.

## Split transfers
If messages are split, the authenticator MUST send the number of splits to the relying party as a response to the request of an attribute. A messages may be split into any number of seperate parts, but each part MUST be at least the size of one character.

## List of available attribues
The authenticator MUST include all available attributes in the send list of available attributes. This list MUST be sent as JSON. The list MUST be named 'available-data'. The source to obtain the public key of the signature MUST be included with the name 'pub-key'. If any other options for the PKI has to be sent, it may be encoded with 'pub-key-options'.
The listed attributes MUST confirm to the naming conventions specified in [here](attribute_names.md). The attributes MUST be listed in an array. Attributes, which can be send send seperately, MUST be listed seperately. If attributes can be send in combination, with others, they MUST be listed in a subarray.

## Checking validity of attributes
To check the validity of received attributes, the relying party MUST obtain the public key of the siging authority. It MUST then calculate the hash of the received attributes using SHA-256, decrypt the received hash values and compare them to assert validity. It may reject attributes coming from untrusted sigin authorities.