---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Squire common contact tracing header
description: Common header data for contact tracing interoperability
menubar: docs_menu
---

# Common Squire Contact Tracing data

Technically this header is part of the inner payload, but the data is common to both 
the [Squire simple inner payload](/payload/simple) or 
the [Squire secured payload](/payload/secured) and can even be used with your own
contact tracing specific custom [Inner payload](/payload/inner)

## What does the common header provide?

The below information is appended to the [Squire Envelope payload](/payload/envelope):-

Note: All numbers are Big Endian (network order).

- Read ID payload
  - Client ID - 16 byte Ephemeral rotating identifier (generated via a callback in the Squire protocol). Could use TOTP to be generated. (not required)
  - Routing code - 4 byte freeform area, specific to the protocol identifier, for routing data or specifying legal jurisdiction/company ownership. For contact tracing this is used as follows:-
    - Country code - 2 byte unsigned integer, ISO-3166-1 NUMERIC code for a country (E.g. 826 is the UK)
    - State code - 2 byte unsigned integer, defined by the country in country code, to identify the part of the country or a commercial or other entity within it
  - My TX power - 2 byte float, current tx power of the device whose ID is being read
  - Inner payload data - remaining bytes are reserved for the inner payload. MUST be either fixed length, or you can provide a parser that detects the end of this payload (See calling card / read characteristic, below)
- Calling card / ID write payload
  - Same content as for Read ID payload except after TX power and before inner payload you also have the following (included in the signature)
  - Your RSSI - 2 byte signed integer. RSSI seen of the written to phone by the writer. Needed to support the (14% in the UK) phones that do not support advertising and so cannot be read from
- Calling card / nearby devices read payload - most recent devices only
  - Multiple payloads, consisting of:-
    - Payload type - 1 byte integer. 0 = direct read, 1 = direct write, other values reserved for future use
    - Received time - 4 byte signed integer, unix epoch seconds when the echange was observed by the device
    - Payload data - as per individual read/write payloads, above. You MUST either configure a fixed length value in the Squire outer payload, or provide your own custom parser to split each shared device payload.
- Calling card / nearby devices write payload (disabled by default in the Squire protocol)
  - Exactly the same as for calling card read payload - most recent devices only

## How do I specify an inner payload?

Implement the callbacks to provide a [custom inner payload](/payload/inner) data or use 
the [Squire simple inner payload](/payload/simple) or the [Squire secured payload](/payload/secured).

## How does international interoperability work?

Each receiving device records the inner payload along with all the other information but may not interpret
its content directly. it's important, therefore, that the issuing country is content with the inner payload
being recorded by a foreign visitors device and when they return home that data could be shared with
the issuing state's systems for contact tracing reasons. 

It needs to be shared because it forms part of the signed data, and will be shared with the issuing country
by that country's health authority should the visitor become ill. This allows countries to implement
their own signing and crytographic approach within the inner payload if they wish.

Worked example (countries mentioned don't necessarily use the Squire protocol):-
- Natalie is from Australia and uses the Victoria contact tracing app
- Jess is from the UK and uses the England app
- Natalie visits the UK and spends 2 hours at a bar talking to Jess. She returns to Australia and falls ill
- Natalie submits her contact information for the past 14 days, that includes Jess' packets
  - Victoria state's health authority now has 8 Client IDs for Jess, but doesn't know they're for the same person, and a bunch of RSSI information
    - Victoria state's health authority doesn't need to know the make and model of Jess' phone, as she's the one that is exposed
- Victoria state's health authority confirms Natalie is ill, and sends "Hey, a person in my country fell ill on X date, and met someone for your country."
  - This includes the entire contact packet shared by Jess, including the time of receipt.
  - This MAY optionally include 'exposure verifier' data, known only to Natalie and Jess' phones, allowing Jess' phone in a decentralised system, or the England health authority in a centralised system, to verify this data was sent by Jess and received by Natalie. (See the [Secured inner payload](/payload/secured) for an example approach)
- England's state health authority processes the payload (outer and inner) and notifies the user of the exposure if necessary
  - Confirms the validity of the contact by verifying the inner packet provided by Jess' phone, and validating the signature of the whole packet
  - In a centralised system, the ClientID would be associated with a particular unique ID, and so England would know it's a single person, Jess, exposed for 2 hours and thus needs notifying. If contact details are already known, a manual contact tracing team could make contact. Alternatively (or as well) a message could be sent to England's app on Jess' phone making her aware, given advice on self isolation and testing, and requesting she submit nearby contact details ([Second order](/background/glossary) contact tracing)
  - In a decentralised system, the entire payload or a portion thereof could be sent to all phones, and distance estimation and exposure could be calculated locally. Ideally the inner payload would be linked via time (E.g. TOTP) to the client ID, the client ID would be predictable only by Jess' phone, and the inner payload verifies the time in an encrypted form, preventing replay attacks
