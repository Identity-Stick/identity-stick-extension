# Attributes encoding
The extension identity-stick references attributes according to [RFC7643 4.1.](https://tools.ietf.org/html/rfc7643#section-4.1.1). To abbreviate the attribute names the following encoding MUST be followed.

# Overview of encoding
The encoding MUST use either the name of the attribute itself, the abbreviation or the supported number. If unsupported attributes need to be used, the corresponding unsupported number MAY be used, but relying parties MAY ignore them.

| **Attribute**                      | **Abbreviation** | **Supported By identity-stick** | **Supported number** | **Unsupported number** |
|------------------------------------|------------------|---------------------------------|----------------------|------------------------|
| *Singular Attributes*              |                  |                                 |                      |                        |
| userName                           | uN               | Supported                       | 0                    | 100                    |
| name                               | n                | Supported                       | 1                    | 101                    |
| formatted                          | n-f              | Supported                       | 2                    | 102                    |
| familyName                         | n-fN             | Supported                       | 3                    | 103                    |
| givenName                          | n-gN             | Supported                       | 4                    | 104                    |
| middleName                         | n-mN             | Supported                       | 5                    | 105                    |
| honoricPrefix                      | n-hP             | Supported                       | 6                    | 106                    |
| honoricSuffix                      | n-hS             | Supported                       | 7                    | 107                    |
| displayName                        | dP               | Supported                       | 8                    | 108                    |
| nickName                           | nN               | Not Supported                   |                      | 109                    |
| profileUrl                         | pU               | Not Supported                   |                      | 110                    |
| title                              | t                | Supported                       | 9                    | 111                    |
| userType                           | uT               | Not Supported                   |                      | 112                    |
| preferredLanguage                  | pL               | Not Supported                   |                      | 113                    |
| locale                             | l                | Supported                       | 10                   | 114                    |
| timezone                           | tz               | Not Supported                   |                      | 115                    |
| active                             | a                | Not Supported                   |                      | 116                    |
| password                           | p                | Not Supported                   |                      | 117                    |
|                                    |                  |                                 |                      |                        |
| *Multi-Valued Attributes*          |                  |                                 |                      |                        |
| emails                             | e                | Supported                       | 11                   | 118                    |
| phoneNumbers                       | pNs              | Not Supported                   |                      | 119                    |
| ims                                | ims              | Not Supported                   |                      | 120                    |
| photos                             | phs              | Not Supported                   |                      | 121                    |
| addresses                          | add              | Supported                       | 12                   | 122                    |
| groups                             | gr               | Not Supported                   |                      | 123                    |
| entitlements                       | en               | Not Supported                   |                      | 124                    |
| roles                              | r                | Not Supported                   |                      | 125                    |
| x509Certificates                   | x                | Not Supported                   |                      | 126                    |
|                                    |                  |                                 |                      |                        |
| *Group schema*                     |                  |                                 |                      |                        |
| displayname                        | g-dN             | Not Supported                   |                      | 127                    |
| members                            | g-m              | Not Supported                   |                      | 128                    |
|                                    |                  |                                 |                      |                        |
| *Enterprise User Scheme Extension* |                  |                                 |                      |                        |
| employeeNumber                     | eN 29            | Not Supported                   |                      | 129                    |
| costCenter                         | cC               | Not Supported                   |                      | 130                    |
| organization                       | o                | Not Supported                   |                      | 131                    |
| division                           | d                | Not Supported                   |                      | 132                    |
| department                         | dpm              | Not Supported                   |                      | 133                    |
| manager                            | m                | Not Supported                   |                      | 134                    |
|                                    |                  |                                 |                      |                        |
| additionalInfo                     |                  | Supported                       | 13                   | 135                    |
| birthdate                          |                  | Supported                       | 14                   | 136                    |