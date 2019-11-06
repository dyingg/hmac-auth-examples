# Mesh API Authentication

The Mesh API uses [HMAC](https://en.wikipedia.org/wiki/HMAC) Authentication for authenticating requests. 
Each request made to the API must carry a "signature", which is an output of the HMAC-SHA1 algorithm. 
The signature created by concatenating selected HTTP headers and using a secret key. 

When the API receives the request, it will fetch the secret and perform the same signature calculation. If the signature that
is provided in HTTP request matches, then the operation is allowed, otherwise the request will be rejected and 401 status code returned. 

## Making Request

In order to authenticate your request, the following HTTP request headers must be included:

* `x-mesh-date` - timestamp of request in format of [RFC 7231](https://tools.ietf.org/html/rfc7231#section-7.1.1.1). Example: `Wed, 06 Nov 2019 21:37:48 GMT`. See [details](###Timestamp).
* `x-mesh-nonce` - unique identifier of the request that is sent to the API. See [details](#Nonce).
* `Authorization` - composed of four components: an algorithm declaration (scheme), api key, list of header names that used in signature, and the calculated signature. All those components structured in format that described in the [next section](#constructing-authorization-header).

### Constructing authorization header

has the following format `HMAC-SHA1 ApiKey={api-access-key};SignedHeaders=x-mesh-date,x-mesh-nonce;Signature={base64-encoded-signature}`.
The signature itself contains all headers in the order they appear in `SignedHeaders`, each one separated by line terminator `\n` (without line terminator at the very end) and each line is: `{lower_case_header_name}:{header_value}`

### Timestamp

* Because the signature is tied to the time stamp of the request, it can't reuse authorization headers
* replay attacks
* clock scew
### Nonce

* https://en.wikipedia.org/wiki/Cryptographic_nonce
* reply attack
* when to retry

## Examples

This repository contains an example in the following languages:

* [Node.js](./node.js) (JavaScript)
* [Node.js](./node.ts) (TypeScript)
* [Postman](./postman)
* [Python 3+](./python3)
* [.NET Core](./dotnet) (C#)
* [Java](./java) 8+
