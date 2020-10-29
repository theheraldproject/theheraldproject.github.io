---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Herald payload design
description: Our privacy preserving and secure payload for slowing disease spread
menubar: docs_menu
---

# Payloads Introduction

The payload is what allows identity and distance estimation information to be shared.
The payload can also include cryptographic information that allows detection and 
prevention of Relay and Replay attacks, and prevent misuse of mobile contact
tracing by mischievous individuals or hostile state actors.

Whilst our [Protocol]({{"/protocol" | relative_url }}) allows you to use any payload you wish, we are
suggesting the below envelope payload in order to maximise international
interoperability and security.

The Herald protocol allows any payload to be transported over it. We do
also provide a recommended envelope header for all apps, and
two contact tracing example payloads:-

![Herald Payload Contents](../images/Payloads.png)

## Payload options

We provide a recommended [Herald Envelope header]({{"/payload/envelope" | relative_url }}) that allows
international interoperability between any app, centralised or decentralised,
whether for contact tracing or other uses.

We are working on a DRAFT [Herald Secured inner payload]({{"/payload/secured" | relative_url }}) as the inner payload
as this provides maximum epidemiological information to the health authority whilst
maintaining privacy by keeping the contact event 'node' identified from the contact
graph on individuals' mobile phones only.

Benefits of our secure payload include:-
- Pseudonymous and privacy preserving
- Can be used for a centralised or decentralised app approach
- Allows for international interoperability
- Lightweight, and supports a range of phones, including older ones

We also provide a simpler [Herald Simple inner payload]({{"/payload/simple" | relative_url }}) for basic
decentralised contact tracing.

You are, of course, free to use your own [Herald-compatible Custom inner payload]({{"/payload/inner" | relative_url }})
or roll your own [Custom payload]({{"/payload/outer" | relative_url }}) entirely 
(especially if you are not writing a contact tracing app!)

We are also working on a [Beacon Payload]({{"/payload/beacon" | relative_url }}) for use in Stores and Restaurants or areas of environmental exposure (E.g. gas, chemicals). Please 
[log an issue on GitHub](https://github.com/vmware/herald/issues) if you are interested in this.

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
  - Provide non-repudiation (i.e. ensuring a contact event is validated from both participants)

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

## Centralised or decentralised

The combined Herald inner and envelope payloads provide a hybrid model.
A contact graph where only one side of the graph is identified is present
on the health authority's server, and all exact contact event pairwise information
(but no identity information) is stored on each phone. The exchange method
also prevents spoofing, relay, and replay attacks.

In a pure decentralised protocol, just generate the ClientID (rotation key)
using a symmetric master key that is only stored in the secure enclave
of your phone, and not shared with anyone.

In a pure centralised protocol we recommend a forward secure ClientID generated
from a symmetric key which is agreed upon by both server (health authority
providing the app) and client (the mobile app) using a Diffie-Hellman key
agreement approach.

Using our protocol and envelope payload allows apps from countries using
both centralised and decentralised approaches to interoperate, maximising
interoperability whilst assuring privacy and security.

A pure decentralised approach has some drawbacks, both for epidemiology and
security. These are described in our [Contact tracing introduction](../background).