# Identity Stick Extension (identity-stick)
This extension enables relying parties and authenticators to exchange identity attributes.

## Extension identifier
identity-stick

## Operation applicability
[Registration](https://www.w3.org/TR/webauthn/#registration-extension) and [Authentication](https://www.w3.org/TR/webauthn/#authentication-extension)

## Client extension input
[create()](https://w3c.github.io/webappsec-credential-management/#dom-credentialscontainer-create): A boolean value to indicate, that a list of all available attributes is requested.
```
partial dictionary AuthenticationExtensionsClientInputs {
 	bool identity-stick;
};
```



[get():](https://w3c.github.io/webappsec-credential-management/#dom-credentialscontainer-get)An JavaScript object defined as follows:
```
dictionary identityMessage{
	required integer type; 		//indicating the type of response
    optional USVString data; 	//JSON format
	optional integer option;	//indicating further options
};


partial dictionary AuthenticationExtensionsClientInputs{
	integer indentity-stick;
};
```
Depending on the type of a message different operations MUST take place in the authenticator. The client can indicate the chosen type by setting the corresponding value. The different types are listed below.
| type | definition |
|------|------------|
| 1    | info       |
| 2    | request    |
| 3    | data       |

The different options are listed below.
| option | definition                      |
|--------|---------------------------------|
| 1      | the option field can be ignored |
| n > 1  | indicating the next nth message |

## Client extension processing
The Client should check the options for user presence. For messages of type 'request' user presence MUST be 'required'. For messages of type 'data' user presence may be 'discouraged'. If incorrect options for user presence are set, the client MUST decline the request by the relying party and respond with an error. Otherwise the client MUST create the authenticator extension input from the client extension input.

## Client extension output
The  [RFC7643 4.1.](https://tools.ietf.org/html/rfc7643#section-4.1.1) is used as naming conventions for exchange of the attributes. The [identity attributes encoding](identity_attributes_encoding.md) with abbreviations and numbers MUST be used.
```
dictionary identityMessage{
	required integer option; //indicating the type of response
	optional USVString data; //JSON format
	optional integer option; //indicating further options
};
partial dictionary AuthenticationExtensionsClientOutputs{
	identityMessage response;
};
```
## Authenticator extension input
The client extension input encoded as a CBOR text string.

## Authenticator extension processing
The authenticator will perform either [user verification](https://www.w3.org/TR/webauthn/#user-verification) or [test of user presence](https://www.w3.org/TR/webauthn/#test-of-user-presence) depending on the the options set by the relying party.

- **authenticatorGetInfo additional behaviors**  
The authenticator indicates to the platform that it supports the "identity-stick" extension via the "extensions" parameter in the [authenticatorGetInfo](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html#authenticatorGetInfo) response.

- **authenticatorMakeCredential additional behaviors**  
The platform sends the [authenticatorMakeCredential](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html#authenticatorMakeCredential) request with the following CBOR map entry in the "extensions" field to the authenticator:
	- "identity-stick": true
The authenticator responds with the following CBOR map entry in the "extensions" fields:
	- "identity-stick":  
		- *response* : is a JSON encoded object containing two values: 'available-data' and 'pub_key'. 
			- *available-data*: is a list of all the available identity attributes the authenticator could provide to the service provider. This list MUST reference the attributes according to [[RFC7643 4.1.](https://tools.ietf.org/html/rfc7643#section-4.1.1)]. If the authenticator has limited capacity encoding as described in [identity attributes encoding](identity_attributes_encoding.md) MAY be used. 
			- *pub_key*: is a String containing the URL, where the public key used for signing the attributes is stored. If the authenticator has limited capacity, the URL MAY be send like a attribute via a data message. If this is the case, the *pub_key* MUST have the value 'REQUEST'.

- **authenticatorGetAssertion additional behaviors**  
The platform sends the [authenticatorGetAssertion](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html#authenticatorGetAssertion) request with the following CBOR map entry in the "extensions" field to the authenticator:
	- "identity-stick":  
		- type (0x01): 1
		- data (0x02): 
			- *requested-attribute*: is a string indicating the type of attribute, the platform requests. This MUST reference attributes according to [[RFC7643 4.1.](https://tools.ietf.org/html/rfc7643#section-4.1.1)] or the encoding as described in [identity attributes encoding](identity_attributes_encoding.md). It should be an attribute provided by the authenticator as indicated by the *attribute-list*.
		- option (0x03): is omitted
The authenticator responds with the following CBOR map entry in the "extensions" fields:
	- "identity-stick": 
		- *number-message-parts*: is an integer value indicating the number of parts the requested attribute will be split into for transport. If the requested attribute is not part of the attributes provided by the authenticator, the response should be -1. 

The platform sends the [authenticatorGetAssertion](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html#authenticatorGetAssertion) request with the following CBOR map entry in the "extensions" field to the authenticator:
	- "identity-stick":  
		- type (0x01): 2
		- data (0x02): 
			- *requested-attribute*: is a string indicating the type of attribute, the platform requests. This MUST reference attributes according to [[RFC7643 4.1.](https://tools.ietf.org/html/rfc7643#section-4.1.1)] or the encoding as described in [identity attributes encoding](identity_attributes_encoding.md). It should be an attribute provided by the authenticator as indicated by the *attribute-list*.
		- option (0x03):
			- *next-requested-part*: is an integer indicating, which part of the attribute is requested as the next part. 
The authenticator responds with the following CBOR map entry in the "extensions" fields:
	- "identity-stick": 
		- *message-part*: is a String value with the next part of the attribute value. The attribute value MUST be a JSON String containing the value of the attribute and a signed hash of it.  the number of parts the requested attribute will be split into for transport. If the requested attribute is not part of the attributes provided by the authenticator, the response should be -1. 


## Authenticator extension output
Same as the client extension output, except represented in CBOR.