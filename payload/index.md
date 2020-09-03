---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: TBD NAME payload design
description: Our privacy preserving and secure payload for slowing disease spread
---

# Introduction

The payload is what allows identity and distance estimation information to be shared.
The payload can also include cryptographic information that allows detection and 
prevention of Relay and Replay attacks, and prevent misuse of mobile contact
tracing by mischievous individuals or hostile state actors.

Whilst our [Protocol](/protocol) allows you to use any payload you wish, we are
suggesting the below envelope payload in order to maximise international
interoperability and security.

Benefits of our payload include:-
- Pseudonymous and privacy preserving
- Can be used for a centralised or decentralised app approach
- Allows for international interoperability
- Lightweight, and supports a range of phones, including older ones

## What every payload must support

Every contact tracing app must allow the sharing of the below information

- Identifying data - to ‘identify’ the nearby mobile phone owner (can be a pseudonymous identifier not linked to name/real ID)
  - Randomly assigned pseudonymous identifier, encrypted with rotating key for privacy
  - Allows a health authority to notify an individual if they are at risk
  - Not a real identity, just enough data to get in contact with the individual. E.g. random ID & third-party messaging ID (think of it like having a PO Box)
- Distance estimation data
  - Distance analogue data (E.g. RSSI for Bluetooth - single or multiple)
  - Calibration / conversion assistance data (E.g. TX Power)
  - Optional: Triangulation / error minimisation data (E.g. three RSSIs from 3 nearby phones)
- Security relevant data
  - Prevent fake/unauthorised client devices
  - Prevent relay attacks
  - Prevent replay attacks
  - Prevent spoofing
  - Provide non-repudiation

Every payload must also consider the following security and privacy issues.

Security requirements:-
- Ensure an identifier belongs to a real registered device/person (prevents ‘election tampering attack’ from hostile state actors) - protect democracy
- Minimise the opportunity for relay/replay attacks, and generation of ‘fake contact networks’
- Prevent denial of service attacks
- Prevent loss of trust from users in the benefits of a contact tracing app - protect people
- Prevent impersonation
- Prevent/discourage malicious use
  - Child wanting to avoid school via self diagnosis

Privacy requirements:-
- Prevent tracking of users as they move about
  - Criminals, law enforcement, oppressive states
  - Commercial / ad tracking
  - To find and harm suspected ill people
  - Protect people from domestic abuse
- Prevent self incrimination with law enforcement
  - Not tracking location of citizenry
- Protect health data (i.e. which ‘id’ is ill/potentially ill)
  - Prevent mob attacks on individuals
- Don’t immediately notify
  - “I’m sat with you, I was notified, so you must be ill!”
Small delay, but not enough to lead to harm

## How our payload supports this

TODO wordsmith the below techie description:-

Registration: Client would send some entropy to server, server sends some entropy to client. Mutual agreement how the master symmetric key generated from this (E.g. DH). Agree a daily key epoch.

Note: In a decentralised system the master symmetric key is generated on device only. In a centralised system some entropy is shared during registration between device and government and used with a DH key agreement procedure to generate the master symmetric key.

`DailyKey`: Daily key generated from master symmetric key and the date

`RotationKey`: 15 minute rotation key generated from the reverse bit information of the first X bytes of the daily key, then trim this code and use the first Y bytes (SHA256 hash)

Use rotation key as shared value in encryption mechanism

Read ID Payload: `BluetoothID = HMACSignature || CountryCode || StateCode || TransmissionTime || RotationKey [ || OptionalMetadata]`

Write ID Payload: `BluetoothID = HMACSignature || CountryCode || StateCode || TransmissionTime || RotationKey [ || OptionalMetadata]`

Read Distance Estimation Payload: `Payload = N/A - done via advertising packet’s RSSI. Data held = RSSI || TxPower || RotationKey (Also known: MacAddress) `

Write Distance Estimation Payload: `Payload = HMACSignature || RotationKey || TxPower || MyWriteTime || YourRSSI || YourTxPower [ || OptionalMetadata]`

Note: All values are Big Endian

Note: For read payload, rotation key is provided in the manufacturer’s data area of the advertisement

Note: All `TxPower` are optional. Not provided by iOS. Use a -1 value (NOT 0 - it's a log scale) if not directly supplied.

Note: You will also have the ‘received date time’ from the receiving device

Note: `OptionalMetadata` is data shared between devices for the particular application in question. We don’t believe it is needed currently, but could include GPS location information (where legal/desired), indoors vs outdoors, in pocket / on desk / in hand metadata in order to improve distance estimation. This must also be included as data to calculate the HMAC signature. Only the issuing government can verify the hmac signature. (TODO any way to do this via rotation key in decentralised model?)


## Centralised or decentralised

In a decentralised protocol, just generate the Daily Key and Rotation Key
using a symmetric master key that is only stored in the secure enclave
of your phone, and not shared with anyone.

In a centralised protocol we recommend a forward secure daily key generated
from a symmetric key which is agreed upon by both server (health authority
providing the app) and client (the mobile app) using a Diffie Helman key
agreement approach.

Using our protocol and envelope payload allows apps from countries using
both centralised and decentralised approaches to interoperate, maximising
interoperability whilst assuring privacy and security.

A decentralised approach has some drawbacks, both for epidemiology and
security. These are described in our [Contact tracing introduction](/background).