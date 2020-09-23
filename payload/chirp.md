---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Chirp Payload Option
description: Chirp style interoperable payload
menubar: docs_menu
---

# DEPRECATED PAGE - See [Herald Simple Inner Payload](/payload/simple)

# Chirp payload

This payload is based on a combination of Diffie-Hellman and [RFC 6238](https://tools.ietf.org/html/rfc6238) TOTP (Time based one-time password) approaches in order to generate a 'chirp' style ID as in the GAEN API, but without the cryptographic issues of predicting an entire day's worth of keys. It also uses HMAC as specified in [RFC 2104](https://tools.ietf.org/html/rfc2104).

## Make up

TODO wordsmith the below techie description:-

Registration: Client would send some entropy to server, server sends some entropy to client. Mutual agreement how the master symmetric key generated from this (E.g. DH). Agree a daily key epoch.

Note: In a decentralised system the master symmetric key is generated on device only. In a centralised system some entropy is shared during registration between device and government and used with a DH key agreement procedure to generate the master symmetric key.

`MasterKey`: 512 bit symmetric key generated either locally on the phone and stored in the secure enclave, or by DH style approach with central server.

`DailyKey`: 256 bit Daily key generated from master symmetric key and the date using HMAC-SHA-256

`RotationKey`: 128 bit key. 15 minute rotation key generated from the daily key and time via HMAC-SHA-256, then trim this code and use the first 16 bytes

Read ID Payload: `BluetoothID = HMACSignature (last 16) || CountryCode (2) || StateCode (2) || TransmissionTime (8 - time_t 64) || RotationKey (16) [ || OptionalMetadata]` (44 bytes)

Write ID Payload: `BluetoothID = HMACSignature (last 16) || CountryCode (2) || StateCode (2) || TransmissionTime (8 - time_t 64) || RotationKey (16) [ || OptionalMetadata]` (44 bytes)

Read Distance Estimation Payload: `Payload = N/A - done via advertising packet’s RSSI. Data held = RSSI || TxPower || RotationKey (Also known: MacAddress) `

Write Distance Estimation Payload: `Payload = HMACSignature (last 16) || RotationKey (16) || TxPower (2) || MyWriteTime (8 - time_t 64) || YourRSSI (1) || YourTxPower (4 float) [ || OptionalMetadata]` (47 bytes)

Note: All values are Big Endian

Note: HMACSignature result is last 16 bytes, whereas the hmac used in the rotation key is the first 16 bytes.

Note: The ephemeral ID/key from the protocol is separate from all keys mentioned in this payload

Note: All `TxPower` are optional. Not provided by iOS. Use a -1 value (NOT 0 - it's a log scale) if not directly supplied.

Note: You will also have the ‘received date time’ from the receiving device

Note: `OptionalMetadata` is data shared between devices for the particular application in question. We don’t believe it is needed currently, but could include GPS location information (where legal/desired), indoors vs outdoors, in pocket / on desk / in hand metadata in order to improve distance estimation. This must also be included as data to calculate the HMAC signature. Only the issuing government can verify the hmac signature. (TODO any way to do this via rotation key in decentralised model?)

## Security & Privacy analysis

Below is the initial security analysis of the attack surface of the above payload as described

### Tracking using a persistent ID

We rotate the rotation key every 15 minutes. This is the same time period as Bluetooth MAC address rotation. This could be rotated more frequently. Ideally rotation should be done at the same time as the MAC address, but this can only be guaranteed at the mobile OS level currently (there's no callback hooks in vendors' Bluetooth stacks).

There is no additional risk posed by this 15 minute rotating ID over and above the mac address rotation period. (Assumes it rotates at the same time as the mac address).

The above method uses a forward secure machanism preventing an attacker from determining a future or past rotation key by observing the rotation keys used by a particular device.

### Ill person tracker

In a deccentralised system, any advertised / general 'chirp' mechanism suffers from the problem of allowing someone to build an 'ill person detector' after they alert the central list of their infection.

This attack works by a tracker app recording the iPhone's persistent name (E.g. "John's iPhone"), and perhaps other persistent Bluetooth information such as encrypted/hashed service IDs and iTunes email details, or even record their face as they enter a shop.

In future this app downloads the keys for ill people, including some of those intercepted on that day. This enables the attacker to identify someone who has become ill since their last visit, and thus target them for harassment.

A centralised system does not suffer from this problem as only the ill person and their health authority ever know that an ill person is linked to this chirp ID.

Our chirp format has an advantage in that the full run of rotation keys cannot be determined from a limited intercepted set of them.

A centralised system of matching would not suffer from this risk.

### Relay and replay attacks misusing a valid ID

An attacker could use a device to intercept many nearby valid rotation keys. A centralised system could validate these rotation keys and thus prevent them being used outside of the time window they were seen in to cause mass exposure events. This is not preventable with a decentralised system.

A relay attack could be used to send the valid ID to a different location and have it recorded as a contact for the same time period in a new geography. Again this attack vector is limited to a 15 minute period.

In a centralised system it would be possible, although computationally and network intensive, to send a message to the originating phone to see if the other side of the contact was themselves seen. This would prevent relay attacks. This does suffer from the issue of 'one way detection', but in our [protocol](/protocol) this is minised thanks to the 'connect back' mechanism in use. This validation functionality would have to be built in to the app and backend external to our protocol and payload specifications.

### Fake Contact network and amplification attacks

It is possible for an attacker to generate fake IDs under any payload for an anonymous app not linked to a persistent government identifier. These IDs can be used by an attacker to say "I'm ill, and honest have been close to these 200 contacts for 2 hours each within 1m".

This type of attack can be prevented through 'average number of alerts per ill person' simple mathematical analysis prior to locking people down. This can only be achieved in a centralised system, as only a centralised system knows how many 'chirp' values represent a given number of people. I.e. whether's a 2 hour period is 1 chirp for 8 people, or 8 chirps for the same person.

An amplification attack occurs where a network of fake IDs is generated and 200 intercepted chirps are spread out between these fake IDs. This would drop each individual notification below the average number of exposure notifications per ill person, and thus not be detected using the above mechanism.

Rotating the chirp every 15 minutes prevents long exposure a little. Using a shorter rotation period (E.g. 1 minute) would be even better, and guarantee each contact to be below the 3.5 minute window at 1m to breach a standard (15 min 2m) risk threshold under the Oxford Model.

It's worth rememberding that the receiving device (the same as the person who says they are ill) is the one calculating the RSSI and thus providing the distance estimator to the backend system (centralised or decentralised). This means they can 'fake' the RSSI. They can also fake the txpower in the received advertisement packet.

Using a write instead of just advertisement data to provide a signed and played-back RSSI from the remote device would catch mismatches in RSSI and txpower data. This extra Bluetooth communication, however, will come at the cost of [higher continuity error](/paper) and for that reason we don't recommend it.

It should be noted that in a decentralised system, a smaller time window means a much bigger list of chirps when a person becomes ill. This could pose a DDoS style network effect risk to a nation's mobile and internet networks.

Applying the 'please verify this contact data' approach to the other side of the communication, as mentioned in the relay and replay section above, would provide a way of validating the data prior to exposure notification. Again this can only be achieved in a centralised system.

### Bio Security - track outbreak spread

In a centralised system with some link to geography (E.g. postal district in the UK - 10 000 households) the spread of the disease can be tracked by ill people submitting their IDs when they become ill to a central.

All apps can also have functionality added such as a symptom tracker, providing even earlier data that can be compared to later test data to spot patterns in symptoms throughout populations.

Our payload when used in a centralised system would provide a rich set of data with low exposure risk error, allowing for high quality bio security monitoring and modelling.

This is not possible in a decentralised solution.

### Bio Security - Identify asymptomatic spreaders

In our payload, when used in a centralised system only, asymptomatic spreaders could be spotted as their chirp IDs would be common across multiple ill people. Statistical modelling could be used to spot such people and ask them to get tested.

This is not possible in a decentralised solution.

## Bio Security - Detect super spreaders

As for asymptomatic spreaders, above, our payload and protocol provide a very rich and accurate data set to allow super spreaders to be detected and communicated with.

This is not possible in a decentralised solution.
