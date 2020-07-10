# Identity Stick Extension (identity-stick)
This extension enables relying parties and authenticators to exchange identity attributes.

## Extension identifier
identity-stick

## Operation applicability
[Registration](https://www.w3.org/TR/webauthn/#registration-extension) and [Authentication](https://www.w3.org/TR/webauthn/#authentication-extension)

## Client extension input
[create()](https://w3c.github.io/webappsec-credential-management/#dom-credentialscontainer-create):



[get():](https://w3c.github.io/webappsec-credential-management/#dom-credentialscontainer-get)An JavaScript object defined as follows:
```
dictionary identityMessage{
	required integer option; //indicating the type of response
        optional USVString data; //JSON format
};


partial dictionary AuthenticationExtensionsClientInputs{
	integer indentity-stick;
};
```
The different types are listed below.
| type | definition |
|------|------------|
| 0    | request    |
| 1    | challenge  |
| 2    | data       |
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
Depending on the 
## Authenticator extension output

