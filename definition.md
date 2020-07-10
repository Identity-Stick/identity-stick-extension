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
Depending on the type of a message different operations should be take place in th authenticator. The different types are listed below.
| type | definition |
|------|------------|
| 0    | request    |
| 1    | challenge  |
| 2    | data       |


The different options are listed below
| option | definition                      |
|--------|---------------------------------|
| 1      | the option field can be ignored |
| n > 1  | indicating the next nth message |

## Client extension processing
None, except creating the authenticator extension input from the client extension input.

## Client extension output
The RFC7643 is used for exchange the attributes
```
dictionary identityMessage{
	required integer option; //indicating the type of response
	optional USVString data; //JSON format 
};
partial dictionary AuthenticationExtensionsClientOutputs{
	identityResponse response;
};
```
## Authenticator extension input
The client extension input encoded as a CBOR text string.

## Authenticator extension processing
- **authenticatorGetInfo additional behaviors**  
The authenticator indicates to the platform that it supports the "identity-stick" extension via the "extensions" parameter in the [authenticatorGetInfo](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html#authenticatorGetInfo) response.

- **authenticatorMakeCredential additional behaviors**  
The authenticator sends the [authenticatorMakeCredential](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html#authenticatorMakeCredential) request with the following CBOR map entry in the "extensions" field to the authenticator:
	- "identity-stick": \<attribute-list\>  
	\<attribute-list\> is a list of all the available identity attributes the authenticator could provide to the service provider. This list MUST reference the attributes according to RFC7643 4.1. If the authenticator has limited capacity encoding as described in **2.1 identity attributes encoding** MIGHT be used.

## Authenticator extension output

