---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Squire envelope payload
description: Our privacy preserving and secure payload for sharing contact tracing data
menubar: docs_menu
---

# Squire Envelope Header

Each national contact tracing team has two competing aims when it comes to payload:-

- Provide the unique information necessary / permissable under their law to enable contact tracing
- Ensure where possible their payload allows for international interoperability

In the Squire Envelope payload for Contact Tracing we provide the outer payload to allow for international
interoperability but leave the inner payload - the data payload for that country - to the national team
to implement.

![Squire Envelope Header](/images/SquirePayloadEnvelope.png)

We recommend each country use the Envelope protocol and embed their payload as an [Inner Payload](/payload/inner),
but there's nothing stopping individual groups from creating their own [Outer payload](/payload/outer) if they wish.
Doing so though will prevent international interoperability between contact tracing apps and devices.

## What does the outer payload provide?

The outer payloads allow for certain standard data to be passed.

Note: All numbers are Big Endian (network order).

- Read ID payload
  - Payload identifier and version - 4 byte unsigned integer - 0x00 for Squire envelope payload V1.0, up to 0x07, wrapping around for further versions (not part of the signature)
  - Inner payload data - See the [common contact tracing header](/payload/common) for options for this content for contact tracing, or custom use cases
- Calling card / ID write payload
  - Same content as for Read ID payload 
- Calling card / nearby devices read payload - most recent devices only
  - Multiple payloads, consisting of:-
    - Payload type - 1 byte integer. 0 = direct read, 1 = direct write, other values reserved for future use
    - Received time - 4 byte signed integer, unix epoch seconds when the echange was observed by the device
    - Payload data - as per individual read/write payloads, above. You MUST either configure a fixed length value in the Squire outer payload, or provide your own custom parser to split each shared device payload.
- Calling card / nearby devices write payload (disabled by default in the Squire protocol)
  - Exactly the same as for calling card read payload - most recent devices only

## Reserved fields

Any protocol and version identify ending in 0-7 (E.g. 0x00-0x07, 0x10-0x17, 0x20-0x27) is reserved
for the use of the Squire Project. You are free to use any other values. 

You do not need to register your protocol IDs with us so long as you use a different Bluetooth
service UUID and characteristic UUID. If you wish to provide an extension to Squire, as we
have done for our [beacon](/payload/beacon) payload, then please do let us know what
range you are using.