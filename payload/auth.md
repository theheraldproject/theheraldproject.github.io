---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Authenticated association
description: Kerberos style interoperable payload
menubar: reference_menu
---

# DEPRECATED PAGE - See [Herald Secured Inner Payload](/payload/secured)

# Authenticated association payload

This method uses a Kerberos [RFC 4120](https://tools.ietf.org/html/rfc4120) style authentication approach. Instead of a centralised server acting as the Ticker Granting System (TGS), however, each mobile app acts as one. A centralised healthcare database would instead act as the 'service', and use the tickes uploaded by the ill person to validate and notify the other users of exposure.

This approach has the advantage that any pairwise exposure can be validated without a centralised server checking with each mobile phone that the contact was valid, and also therefore not revealing a list of ill people.

We will use AES-SHA-256. We will also use pkinit style initiation as per [RFC 4556](https://tools.ietf.org/html/rfc4556).

## Payload specification

ClientID = 15 minute TOTP token generated from the device's country registration ID and the start validity datetime (32 bytes)

IdentityReadPacket = `PublicKeyForTicketIssuing (32)`

TicketReadPacket = `TGSSessionKey (encrypted using PUBLIC key of other device) (16) || TGT (Uses secret key of device being read from, ValidityStartTime (8), ValidityEndTime (8), CountryCode (2), StateCode (2), ClientID(32)) overall 52 || CTSTicket (for health care system's exposure notification 'service') (ClientID (32), ValidityStartTime (8), ValidityEndTime (8), ClientServerSessionKey (16), all encrypted using the server daily secret key for this issuing mobile phone) overall 64 || ClientServerSessionKey (for country's central system) (16)` overall 148 bytes


TODO ISSUE INSTEAD OF A CLIENT ID A 'YOUVE BEEN EXPOSED' SESSION KEY, ONLY DECRYPTABLE BY THE ISSUING MOBILE PHONE.


TicketWritePacket = As above, but issuer role reversed. Only one or the other is needed. Both are included as not all devices will support advertisements (and thus reading from and being written to themselves).

Note: Ideally we would use Write packets everywhere, initiated once a client read's the other's public key. This minimises cross talk of reads, and 'I'm not ready yet' read responses if the public key hasn't been read yet.

## Server communication specification

On a device identifying as 'fallen ill', it uploads the following information for each contact event

ContactEventDetails = `TimestampReceived || MyClientID (as issued to this phone we were in contact with) || CTSTicket || Authenticator || ObservedClientID || RiskExposureDetails (E.g. RSSI, txpower)` 

Note: Could the risk exposure details and the observedclientid be encrypted with the TGS Session key in order to allow a central to know about the contact but not reveal the ObservedClientID? Thus be privacy preserving whilst providing contact level information?


## Security & Privacy analysis

Below is the initial security analysis of the attack surface of the above payload as described

### Tracking using a persistent ID

We rotate the rotation key every 15 minutes. This is the same time period as Bluetooth MAC address rotation. This could be rotated more frequently. Ideally rotation should be done at the same time as the MAC address, but this can only be guaranteed at the mobile OS level currently (there's no callback hooks in vendors' Bluetooth stacks).

There is no additional risk posed by this 15 minute rotating ID over and above the mac address rotation period. (Assumes it rotates at the same time as the mac address).

The above method uses a forward secure machanism preventing an attacker from determining a future or past rotation key by observing the rotation keys used by a particular device.

In our payload above the client ID is itself never passed in the clear, but the public key could be used as a persistent identifier. As this is passed in a read, it could be re-generated for each device interaction, if needed, or on a very regular basis. This would prevent it's use as a tracking ID as each read would be different.

### Ill person tracker

In a deccentralised system, any advertised / general 'chirp' mechanism suffers from the problem of allowing someone to build an 'ill person detector' after they alert the central list of their infection.

This attack works by a tracker app recording the iPhone's persistent name (E.g. "John's iPhone"), and perhaps other persistent Bluetooth information such as encrypted/hashed service IDs and iTunes email details, or even record their face as they enter a shop.

In future this app downloads the keys for ill people, including some of those intercepted on that day. This enables the attacker to identify someone who has become ill since their last visit, and thus target them for harassment.

A centralised system does not suffer from this problem as only the ill person and their health authority ever know that an ill person is linked to this chirp ID.

By issuing a session ticket and only passing that value in the list of 'ill interactions', only the communicating device can identify that person. The identity data listened to over the network is not used to identify the ill person. Thus the barrier of entry for an ill person tracker app would be to register (illegally - Computer Misuse Act) a valid account with a nation's health authority, and be notified of that risk interaction when they fall ill.

This effectively prevents the building of an effective mass-surveilance ill person tracker, for both centralised and decentralised approaches.

### Relay and replay attacks misusing a valid ID

An attacker could use a device to intercept many nearby valid rotation keys. A centralised system could validate these rotation keys and thus prevent them being used outside of the time window they were seen in to cause mass exposure events. This is not preventable with a decentralised system.

As each ticket is generated for a single end device both replay and relay attacks would fail as the receiving device would not be able to decrypt the session key, and thus know it was neither from the real device nor intended for them.

### Fake Contact network and amplification attacks

It is possible for an attacker to generate fake IDs under any payload for an anonymous app not linked to a persistent government identifier. These IDs can be used by an attacker to say "I'm ill, and honest have been close to these 200 contacts for 2 hours each within 1m".

This type of attack can be prevented through 'average number of alerts per ill person' simple mathematical analysis prior to locking people down. This can only be achieved in a centralised system, as only a centralised system knows how many 'chirp' values represent a given number of people. I.e. whether's a 2 hour period is 1 chirp for 8 people, or 8 chirps for the same person.

An amplification attack occurs where a network of fake IDs is generated and 200 intercepted chirps are spread out between these fake IDs. This would drop each individual notification below the average number of exposure notifications per ill person, and thus not be detected using the above mechanism.

In the above payload the ID information is exchanged only between connected devices, and the CTSTicket is constructed such that it can only be used for the receiving device. This prevents distribution of a set of observed information from one device to a wide range of other devices. This eliminates an amplification attack - instead an attacker would need to please a great many fake devices near a target of interest in order to generate such an attack.

### Bio Security - track outbreak spread

In a centralised system with some link to geography (E.g. postal district in the UK - 10 000 households) the spread of the disease can be tracked by ill people submitting their IDs when they become ill to a central.

All apps can also have functionality added such as a symptom tracker, providing even earlier data that can be compared to later test data to spot patterns in symptoms throughout populations.

Our payload when used in a centralised system would provide a rich set of data with low exposure risk error, allowing for high quality bio security monitoring and modelling.

This is not possible in a decentralised solution where we somehow encrypt the observer client ID and the risk exposure details.

### Bio Security - Identify asymptomatic spreaders

In our payload, when used in a centralised system only, asymptomatic spreaders could be spotted as their chirp IDs would be common across multiple ill people. Statistical modelling could be used to spot such people and ask them to get tested.

This is not possible in a decentralised solution where we somehow encrypt the observer client ID and the risk exposure details.

## Bio Security - Detect super spreaders

As for asymptomatic spreaders, above, our payload and protocol provide a very rich and accurate data set to allow super spreaders to be detected and communicated with.

This is not possible in a decentralised solution where we somehow encrypt the observer client ID and the risk exposure details.
