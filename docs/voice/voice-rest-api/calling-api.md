---
title: "Calling API"
excerpt: ""
---
[block:code]
{
  "codes": [
    {
      "code": "URI:\nhttps://callingapi.sinch.com/[version]\nor\nhttps://api.sinch.com/calling/[version]\n\nCurrent  version is \"v1\"",
      "language": "text"
    }
  ]
}
[/block]
## Overview

This API exposes calling-related functionality in the Sinch dashboard.

### Methods
[block:code]
{
  "codes": [
    {
      "code": "[GET]       /configuration/numbers/ \n[POST]      /configuration/numbers/\n[GET]       /configuration/callbacks/applications/{applicationkey}\n[POST]      /configuration/callbacks/applications/{applicationkey}\n[GET]       /calling/query/number/{number}\n[PATCH]     /calls/id/{callId}\n[GET]       /calls/id/{callId}\n[GET]       /conferences/id/{conferenceId}\n[PATCH]     /conferences/id/{conferenceId}/{callId}\n[DELETE]    /conferences/id/{conferenceId}\n[DELETE]    /conferences/id/{conferenceId}/{callId}\n[POST]      /callouts",
      "language": "text"
    }
  ]
}
[/block]
### Errors
[block:code]
{
  "codes": [
    {
      "code": "[Error Codes]\n    40001 - Illegal parameters\n    40002 - Missing parameter\n    40003 - Invalid request\n    40301 - Invalid authorization scheme for calling the method\n    40108 - Invalid credentials\n    40400 - Unable to get user\n    50000 - Internal error",
      "language": "text"
    }
  ]
}
[/block]
## Get Numbers

**\[GET\] /configuration/numbers/**

Get information about your numbers. It returns a list of numbers that you own, as well as their capability (voice or SMS). For the ones that are assigned to an app, it returns the application key of the app.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[GET] https://callingapi.sinch.com/v1/configuration/numbers/",
      "language": "text"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Response
[block:code]
{
  "codes": [
    {
      "code": "[ResponseBody]\n    NumberItem[] numbers\n\n[NumberItem]\n    string - number\n    string - applicationkey\n    string - capability",
      "language": "json"
    }
  ]
}
[/block]
**number** The phone number or list of numbers that you own in E.164 format.

**applicationkey** indicates the application where the number(s) are assigned. If the number is not assigned to an app, no application key is returned.

**capability** the capability of the number. It can take these values:

- voice
- sms

*Example*
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"numbers\": [\n    {\n      \"number\": \"+19160001112222\",\n      \"capability\": \"voice\"\n    },\n    {\n      \"number\": \"+14151112223333\",\n      \"applicationkey\": \"11983f76-12c8-1111-9515-4785c7b49ca8\",\n      \"capability\": \"voice\"\n    }\n  ]\n}",
      "language": "json"
    }
  ]
}
[/block]
### Errors

    ``50000`` - Internal error

## Update Numbers

**\[POST\] /configuration/numbers/**

Assign a number or a list of numbers to an application.

### Request
[block:code]
{
  "codes": [
    {
      "code": "[RequestBody]\n    string[] - numbers\n    string - applicationkey\n    string - capability   ",
      "language": "json"
    }
  ]
}
[/block]
**number** The phone number or list of numbers in E.164 format.

**applicationkey** indicates the application where the number(s) will be assigned. If empty, the application key that is used to sign the request will be used.

**capability** indicates the DID capability that needs to be assigned to the chosen application. Valid values are “voice” and “sms”. Please note that the DID needs to support the selected capability.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[POST] https://callingapi.sinch.com/v1/configuration/numbers/",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n    \"numbers\":[\"+14151112223333\"],\n    \"applicationkey\":\"11983f76-12c8-1111-9515-4785c7b67ca8\"\n}",
      "language": "json"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Response
[block:code]
{
  "codes": [
    {
      "code": "[ResponseBody]\n    Empty",
      "language": "json"
    }
  ]
}
[/block]
### Errors

    ``50000`` - Unable to update number
    ``40001`` - Unable to update numbers
    ``40003`` - Invalid Application Key

## Un-assign Number

**\[DELETE\] /configuration/numbers/**

Un-assign a number from an application.

### Request
[block:code]
{
  "codes": [
    {
      "code": "[RequestBody]\n    string - number\n    string - applicationKey\n    string - capability",
      "language": "json"
    }
  ]
}
[/block]
**number** The phone number in E.164 format.

**applicationKey** indicates the application from which the number will be un-assigned. If empty, the application key that is used to sign the
request will be used.

**capability** indicates the DID capability. Valid values are “voice” and “sms”. Please note that the DID needs to support the selected capability.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[DELETE] https://callingapi.sinch.com/v1/configuration/numbers/",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n    \"number\":\"+14151112223333\",\n    \"applicationKey\":\"11983f76-12c8-1111-9515-4785c7b67ca8\",\n    \"capability\": \"voice\"\n}",
      "language": "json"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Response
[block:code]
{
  "codes": [
    {
      "code": "[ResponseBody]\n    Empty",
      "language": "json"
    }
  ]
}
[/block]
### Errors

    ``50000`` - Unable to update number
    ``40001`` - Unable to update numbers
    ``40003`` - Invalid Application Key

## Get Callbacks

**\[GET\] /configuration/callbacks/applications/{applicationkey}**

Get callback URLs.

### Request

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[GET] https://callingapi.sinch.com/v1/configuration/callbacks/applications/94983f76-1161-6655-9515-4785c7b67ca8",
      "language": "text"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Response
[block:code]
{
  "codes": [
    {
      "code": "[ResponseBody]\n    UrlItem url\n\n[UrlItem]\n    string - primary\n    string - fallback ",
      "language": "json"
    }
  ]
}
[/block]
**primary** is your primary callback URL.

**fallback** is your fallback callback URL. It is used only if Sinch platform gets a timeout or error from your primary callback URL.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"url\" : { \n    \"primary\" : \"http://primary.com\",\n      \"fallback\" : \"http://fallback.com\"\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
### Errors

    ``50000`` - Unable to get configuration
    ``40003`` - Invalid Application Key

## Update Callbacks

**\[POST\] /configuration/callbacks/applications/{applicationkey}**

Update callback URLs.

### Request
[block:code]
{
  "codes": [
    {
      "code": "[RequestBody]\n    UrlItem url\n\n[UrlItem]\n    string - primary\n    string - fallback",
      "language": "json"
    }
  ]
}
[/block]
**primary** is your primary callback URL.

**fallback** is your fallback callback URL. It is used only if Sinch platform gets a timeout or error from your primary callback URL.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[POST] https://callingapi.sinch.com/v1/configuration/callbacks/applications/94983f76-1161-6655-9515-4785c7b67ca8",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"url\" : { \n    \"primary\" : \"http://primary.com\",\n      \"fallback\" : \"http://fallback.com\"\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Response
[block:code]
{
  "codes": [
    {
      "code": "[ResponseBody]\n    Empty",
      "language": "json"
    }
  ]
}
[/block]
### Errors

    ``50000`` - Unable to update configuration
    ``40003`` - Invalid Application Key

## Query Number

**\[GET\] /calling/query/number/{number}**

Get information about the requested number.

**number** The phone number you want to query.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[GET] https://callingapi.sinch.com/v1/calling/query/number/+46(0)73-017-0101",
      "language": "text"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Response
[block:code]
{
  "codes": [
    {
      "code": "[ResponseBody]\n    QueryNumberResponse\n\n[QueryNumberResponse]\n    NumberItem - number\n\n[NumberItem]\n    string - countryId\n    string - numberType\n    string - normalizedNumber\n    bool - restricted\n    Money - rate",
      "language": "json"
    }
  ]
}
[/block]
**countryId** ISO 3166-1 formatted country code

**numberType** can be one of the following values:

>   - Unknown
>   - Fixed
>   - Mobile
>   - Other

**normalizedNumber** E.164 normalized number.

**rate** is the cost per minute to call the destination number.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"numberItem\": {\n    \"countryId\": \"SE\",\n      \"numberType\": \"Mobile\",\n        \"normalizedNumber\": \"+46730170101\",\n          \"restricted\": false,\n            \"rate\": {\n              \"currencyId\": \"USD\",\n                \"amount\": 0.0368\n            }\n  }\n}",
      "language": "json"
    }
  ]
}
[/block]
### Errors

    ``40001`` - Invalid number specified
    ``50000`` - Internal error

## Manage Call

**\[Patch\] /calls/id/{callId}**

Plays messages in an ongoing call and optionally hangs up the call.

### Request
[block:code]
{
  "codes": [
    {
      "code": "[RequestBody]\n    SVAML\n\n[SVAML]\n    Instruction[] - instructions\n    Action - action",
      "language": "json"
    }
  ]
}
[/block]
This method can be used to play messages in an ongoing call and potentially perform an action, such as hanging up the call. There are two instructions available to play a message. The *PlayFiles* instruction can be used to play an IVR message, while the *Say* instruction can be used to play a text-to-speech message. The message, if specified, is played only on the caller side. A caller can, for example, hear a message saying the total minutes have expired and that the call will be disconnected.
[block:callout]
{
  "type": "warning",
  "body": "This method can only be used for calls that are originating from or terminating to the PSTN network.",
  "title": "Important"
}
[/block]
For more information on playing messages and performing actions on calls see the `Callback API <callbackapi>`.

*Example IVR*
[block:code]
{
  "codes": [
    {
      "code": "[PATCH] https://callingapi.sinch.com/v1/call/id/4398599d1ba84ef3bde0a82dfb61abed",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"instructions\": [\n    {\n      \"name\": \"playFiles\",\n      \"ids\": [\n        \"welcome\"\n      ],\n      \"locale\": \"en-US\"\n    }\n  ],\n    \"action\": {\n      \"name\": \"hangup\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
*Example text-to-speech*
[block:code]
{
  "codes": [
    {
      "code": "[PATCH] https://callingapi.sinch.com/v1/call/id/4398599d1ba84ef3bde0a82dfb61abed",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"instructions\": [\n    {\n      \"name\": \"say\",\n      \"text\": \"Hello, this is a text to speech message\",\n      \"locale\": \"en-US\"\n    }\n  ],\n    \"action\": {\n      \"name\": \"hangup\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
*Example start-recording*
[block:code]
{
  "codes": [
    {
      "code": "[PATCH] https://callingapi.sinch.com/v1/call/id/4398599d1ba84ef3bde0a82dfb61abed",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"action\": {\n    \"name\": \"continue\",\n      \"record\": true\n  }\n}\n",
      "language": "json"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Errors

    ``40003`` - Invalid request
    ``40400`` - Call not found
    ``50000`` - Internal error

## Get Call Result

**\[GET\] /calls/id/{callId}**

Gets information about a specific call.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[GET] https://callingapi.sinch.com/v1/calls/id/4398599d1ba84ef3bde0a82dfb61abed",
      "language": "text"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Response
[block:code]
{
  "codes": [
    {
      "code": "[ResponseBody]\n    GetCallResultResponse\n\n[GetCallResultResponse]\n    identity - from\n    identity - to\n    string - domain\n    string - callId \n    int - duration \n    string - status\n    string - result\n    string - reason\n    time - timestamp\n    object - custom\n    money - userRate\n    money - debit\n    decimal - debitMinutes",
      "language": "json"
    }
  ]
}
[/block]
**from** contains the caller information.

**to** contains the callee information.

**domain** can be either “pstn” for PSTN endpoint or “mxp” for data (app or web) clients.

**callId** is the unique identifier of the call.

**duration** shows the duration of the call in seconds.

**status** contains the status of the call.

**result** contains the result of a call. It may have one of the following values:

    ``"N/A"`` | ``"ANSWERED"`` | ``"BUSY"`` | ``"NOANSWER"`` | ``"FAILED"``

**reason** contains the reason why a call ended. It may have one of the following values:

> ``"N/A"`` | ``"TIMEOUT"`` | ``"CALLERHANGUP"`` | ``"CALLEEHANGUP"`` | ``"BLOCKED"`` | ``"NOCREDITPARTNER"`` | ``"MANAGERHANGUP"`` |``"CANCEL"`` | ``"GENERALERROR"``
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "This method can only be used for calls that are originating from or terminating to the PSTN network."
}
[/block]
## Get Conference Info

**\[GET\] /conferences/id/{conferenceId}**

Gets information about an ongoing conference

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[GET] https://callingapi.sinch.com/v1/conferences/id/myConference",
      "language": "text"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Response
[block:code]
{
  "codes": [
    {
      "code": "[ResponseBody]\n    GetConfrenceInfoResponse\n\n[GetConferenceInfoResponse]\n    Participant[] - participants\n\n[Participant]\n    string - cli\n    string - id\n    int - duration\n    bool - muted",
      "language": "json"
    }
  ]
}
[/block]
**cli** shows the phone number of the PSTN participant that was connected in the conference, or whatever was passed as CLI for data originated/terminated calls.

**callId** is the callId of the call leg that the participant joined the conference.

**duration** shows the number of seconds that this participant was connected in the conference.

**muted** shows if the participant is muted currently.

*Example*
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"participants\": [\n    {\n      \"cli\": \"+46708168731\",\n      \"id\": \"abcabcabcabca\",\n      \"duration\": 14,\n      \"muted\": false\n    },\n    {\n      \"cli\": \"+46708425201\",\n      \"id\": \"cdecdecdecde\",\n      \"duration\": 12,\n      \"muted\": false\n    }\n  ]\n}",
      "language": "json"
    }
  ]
}
[/block]
### Errors

    ``40400`` - Conference not found

## Mute/Unmute Conference Participant

**\[PATCH\] /conferences/id/{conferenceId}/{callId}**

Mutes or unmutes a participant in an ongoing conference

*Example*
[block:code]
{
  "codes": [
    {
      "code": "[PATCH] https://callingapi.sinch.com/v1/conferences/id/myConference/abcdef01234",
      "language": "text"
    }
  ]
}
[/block]
### Request
[block:code]
{
  "codes": [
    {
      "code": "[RequestBody]\n    ConferenceCommand\n\n[ConferenceCommand]\n    string - command",
      "language": "text"
    }
  ]
}
[/block]
**command** can be either “mute” or “unmute”

*Example*
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"command\": \"mute\"\n}",
      "language": "json"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Errors

    ``40400`` - Conference not found

## Kick Conference Participant

**\[DELETE\] /conferences/id/{conferenceId}/{callId}**

Kicks a participant from an ongoing conference

### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

## Kick All Conference Participants

**\[DELETE\] /conferences/id/{conferenceId}** - Kicks all participants from an ongoing conference

### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

## Conference and Text-To-Speech Callouts<span id="callouts"></span>

**\[POST\] /callouts**

Requests a call to be initiated from the server

### Request
[block:code]
{
  "codes": [
    {
      "code": "[RequestBody]\n    CalloutRequest\n\n[CalloutRequest]\n    string - method\n    TtsCalloutRequest? - ttsCallout\n    ConferenceCalloutRequest? - conferenceCallout",
      "language": "text"
    }
  ]
}
[/block]
There are currently two types of callouts that are supported: conference callouts and text-to-speech callouts.

### Conference callout

With conference callout, the server initiates call to a phone number and when the call is answered, it is connected to a conference room. The same API can be used multiple times to connect multiple phone numbers in the same conference room.

### Request
[block:code]
{
  "codes": [
    {
      "code": "[ConferenceCalloutRequest]\n    string - cli\n    Identity - destination\n    int - maxDuration\n    bool - enableAce\n    bool - enableDice\n    bool - enablePie\n    string - custom\n    string - locale\n    string - conferenceId\n    string - greeting\n    string - mohClass",
      "language": "json"
    }
  ]
}
[/block]
**cli** is the number that will be displayed as caller

**destination** identifies the endpoint that should be called.

**type** can be “number” for PSTN endpoints, or “username” for data endpoints (app or web clients).

**endpoint** is either the number of a PSTN endpoint, or the username for a data endpoint.

**domain** can be either “pstn” for PSTN endpoint or “mxp” for data (app or web) clients.

**custom** can be used to input custom data.

**locale** specifies the language for the Text-to-speech message. Supported languages: \* en-US: English, United States \* en-AU: English, Australia \* en-CA: English, Canada \* en-GB: English, United Kingdom \* en-IN: English, India \* es-ES: Spanish, Spain \* es-MX: Spanish, Mexico \* es-CA : Spanish, Catalunya \* fr-FR: French, France \* fr-CA: French, Canada \* ja-JP: Japanese \* ko-KR: Korean \* pt-BR: Portuguese, Brazil \* pt-PT: Portuguese \* zn-CN: Chinese, China \* zh-HK: Chinese, Hong
Kong \* zh-TW: Chinese, Taiwan \* da-DK: Danish \* de-DE: German \* fi-FI: Finish \* it-IT: Italian \* nb-NO: Norwegian \* nl-NL: Dutch \* pl-PL: Polish \* ru-RU: Russian \* sv-SE: Swedish

**greeting** is the text that will be spoken as a greeting.

**conferenceId** shows the Id of the conference to which the participant will be connected.

If **enableAce** is set to true and the application has a callback URL specified, you will receive an ACE callback when the call is answered. When the callback is received, your platform must respond with a svamlet, containing the “connectconf” action in order to add the call to a conference or create the conference if it’s the first call. If it is set to false, no ACE event will be sent to your backend.

*Example ACE response when* `enableAce` *set to true*
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"instructions\": [],\n    \"action\": {\n      \"name\": \"connectConf\",\n        \"conferenceId\": \"myConference\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
If **enableDice** is set to true and the application has a callback URL specified, you will receive a DiCE callback when the call is disconnected. If it is set to false, no DiCE event will be sent to your backend.

If **enablePie** is set to true and the application has a callback URL specified, you will receive a PIE callback after a runMenu action, with the information of the action that the user took. If it is set to false, no PIE event will be sent to your backend.

*Example of conference callout*
[block:code]
{
  "codes": [
    {
      "code": "[POST] https://callingapi.sinch.com/v1/callouts",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"method\" : \"conferenceCallout\",\n    \"conferenceCallout\" :\n    {\n      \"cli\" : \"46000000000\",\n        \"destination\" : { \"type\" : \"number\", \"endpoint\" : \"46000000001\" },\n          \"domain\" : \"pstn\",\n            \"custom\" : \"customData\",\n              \"locale\" : \"en-US\",\n                \"greeting\" : \"Welcome to my conference\",\n                  \"conferenceId\" : \"my_conference_room_id\",\n                    \"enableAce\": false,\n                      \"enableDice\" : false\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
### Text-to-speech

With the text-to-speech callout, the server initiates a call to a phone number and plays a synthesized text messages or pre-recorded sound files.

### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.

### Request
[block:code]
{
  "codes": [
    {
      "code": "[TtsCalloutRequest]\n    string - cli\n    Identity - destination\n    string - domain\n    string - custom\n    string - locale\n    string - text\n    string - prompts\n    bool - enableAce\n    bool - enableDice",
      "language": "json"
    }
  ]
}
[/block]
**cli** is the number that will be displayed as caller

**destination** identifies the endpoint that should be called.

**type** is a parameter inside the “destination” object. It can be “number” for PSTN endpoints, or “username” for data endpoints (app or web clients).

**endpoint** is a parameter inside the “destination” object. It is either the number of a PSTN endpoint, or the username for a data endpoint.

**domain** can be either “pstn” for PSTN endpoint or “mxp” for data (app or web) clients.

**custom** can be used to input custom data.

If **enableAce** is set to true and the application has a callback URL specified, you will receive an ACE callback when the call is answered. When the callback is received, your platform must respond with a svamlet, typically containing a “playFiles” instruction and a “hangup” action. If it is set to false, no ACE event will be sent to your backend.

*Example ACE response when \`\`enableAce\`\` set to true*
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"instructions\": [\n    {\n      \"name\": \"playFiles\",\n      \"ids\": [\n        \"#tts[hello world]\"\n      ]\n    }\n  ],\n    \"action\": {\n      \"name\": \"hangup\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
*Example ACE response when \`\`enableAce\`\` set to true*. In this example, the result will be the same as it would with the enableAce set to false - the message specified in the `TtsCalloutRequest` will be played
[block:code]
{
  "codes": [
    {
      "code": "{\n  \"instructions\": [],\n    \"action\": {\n      \"name\": \"continue\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
If **enableDice** is set to true and the application has a callback URL specified, you will receive a DiCE callback when the call is disconnected. If it is set to false, no DiCE event will be sent to your backend.

**locale** specifies the language for the Text-to-speech message. Supported languages: \* en-US: English, United States \* en-AU: English, Australia \* en-CA: English, Canada \* en-GB: English, United Kingdom \* en-IN: English, India \* es-ES: Spanish, Spain \* es-MX: Spanish, Mexico \* es-CA : Spanish, Catalunya \* fr-FR: French, France \* fr-CA: French, Canada \* ja-JP: Japanese \* ko-KR: Korean \* pt-BR: Portuguese, Brazil \* pt-PT: Portuguese \* zn-CN: Chinese, China \* zh-HK: Chinese, Hong Kong \* zh-TW: Chinese, Taiwan \* da-DK: Danish \* de-DE: German \* fi-FI: Finish \* it-IT: Italian \* nb-NO: Norwegian \* nl-NL: Dutch \* pl-PL: Polish \* ru-RU: Russian \* sv-SE: Swedish

**text** is the text that will be spoken in the text-to-speech message.

**prompts** is an advanced alternative to to using “text”. You can then supply a “;”-separated list of prompts. Either prompt can be the name of a pre-recorded file or a text-to-speech string specified as “\#tts\[my text\]”. To upload and use pre-recorded files, you need to contact Sinch for support.

*Example of text-to-speech callout*
[block:code]
{
  "codes": [
    {
      "code": "[POST] https://callingapi.sinch.com/v1/callouts",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"method\" : \"ttsCallout\",\n    \"ttsCallout\" :\n    {\n      \"cli\" : \"46000000000\",\n        \"destination\" : { \"type\" : \"number\", \"endpoint\" : \"46000000001\" },\n          \"domain\" : \"pstn\",\n            \"custom\" : \"customData\",\n              \"locale\" : \"en-US\",\n                \"text\" : \"Hello, this is a synthesized message.\"\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
*Example of text-to-speech callout*
[block:code]
{
  "codes": [
    {
      "code": "[POST] https://callingapi.sinch.com/v1/callouts",
      "language": "text"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "{\n  \"method\" : \"ttsCallout\",\n    \"ttsCallout\" :\n    {\n      \"cli\" : \"46000000000\",\n        \"destination\" : { \"type\" : \"number\", \"endpoint\" : \"46000000001\" },\n          \"domain\" : \"pstn\",\n            \"custom\" : \"customData\",\n              \"locale\" : \"en-US\",\n                \"prompts\" : \"#tts[Hello, this is a synthesized message];myprerecordedfile\",\n                  \"enabledice\" : true\n    }\n}",
      "language": "json"
    }
  ]
}
[/block]
### Authorization

This is a protected resource and requires an `application signed request <applicationsignedrequest>` or `basic auth <basicauthorization>`.