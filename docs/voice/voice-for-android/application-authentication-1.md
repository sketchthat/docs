---
title: "Application authentication"
excerpt: ""
---
A user identity must be provided when initiating a Sinch client. The first time the application instance and the Sinch client are running on behalf of a particular user, they are required to register against the Sinch service. This is mostly handled transparently by the Sinch SDK, but it works slightly differently depending on which authentication scheme you choose to use.

The step of registering a user identity against the Sinch service requires the application instance to be authenticated and authorized to perform the user registration. Once the application instance has successfully registered the user identity, it will also have obtained the necessary credentials to perform further authorized requests for that specific user, for example, calling.

Two different authentication schemes are available: authentication by client access to application secret and authentication supported by application server.

## Authentication by client access to Application Secret

This application authentication scheme is based on giving the application direct access to the Application Secret, which enables the Sinch Client SDK in the application to self-sign an authorized request to perform user registration. Choosing this authentication scheme corresponds to initiating the Sinch client by using the factory method that takes both an Application Key and an Application Secret.

Using this authentication scheme is the quickest way to get started as the client application instances can directly perform authorized requests against the Sinch service.
[block:callout]
{
  "type": "warning",
  "title": "Caution",
  "body": "It is not recommended to have the application secret in plain text in the source code in the release version of the application."
}
[/block]
## Authentication supported by application server

This application authentication scheme is based on the client application instance not having direct access to the Application Secret. Instead, when the Sinch client needs to perform an authorized request to register a user identity against the Sinch service, it needs to be provided with an authentication signature and a registration sequence to perform the registration. This should be provided by the application’s backend service, for example, by using a HTTP request over an SSL connection.

This scheme has the benefit of the application secret never being directly accessible by the client applications and provides a better level of security as well as flexibility.
[block:callout]
{
  "type": "info",
  "body": "The need for the Sinch client to request an authentication signature and registration sequence is only required once per user and device–not on every application launch.",
  "title": "Note"
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/0711e55-authentication_via_application_server.png",
        "authentication_via_application_server.png",
        686,
        497,
        "#f5f5f5"
      ],
      "sizing": "smart"
    }
  ]
}
[/block]
### Generating the signature

The *Application Server* is responsible for generating a valid signature for each registration request that it accepts as a valid user registration. The *sequence* is a [cryptographic nonce](http://en.wikipedia.org/wiki/Cryptographic_nonce), and must be a monotonically increasing value. The signature is then generated as as follows (pseudogrammar):
[block:code]
{
  "codes": [
    {
      "code": "string userId;\nstring applicationKey; // E.g. \"196087a1-e815-4bc4-8984-60d8d8a43f1d\"\nstring applicationSecret; // E.g. \"oYdgGRXoxEuJhGDY2KQ/HQ==\"\nuint64 sequence = previous_sequence + 1; // E.g. previous_sequence = 0\n\nstring stringToSign = userId + applicationKey + sequence + applicationSecret;\n\n// Use a Base64-encoder that don't introduce line-breaks, \n// or trim the output signature afterwards.\nstring signature = Base64.encode(SHA1.digest(stringToSign));",
      "language": "objectivec"
    }
  ]
}
[/block]
For example, in Java:
[block:code]
{
  "codes": [
    {
      "code": "// Generating the Signature - Java\n// import java.security.MessageDigest;\n// import org.apache.commons.codec.binary.Base64;\n\nString userId; \nString applicationKey; // E.g. \"196087a1-e815-4bc4-8984-60d8d8a43f1d\";\nString applicationSecret; // E.g. \"oYdgGRXoxEuJhGDY2KQ/HQ==\";\nlong sequence; // fetch and increment last used sequence\n\nString toSign = userId + applicationKey + sequence + applicationSecret;\n\nMessageDigest messageDigest = MessageDigest.getInstance(\"SHA-1\");\nbyte[] hash = messageDigest.digest(toSign.getBytes(\"UTF-8\"));\n\nString signature = Base64.encodeBase64String(hash).trim();",
      "language": "java"
    }
  ]
}
[/block]