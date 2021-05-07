---
layout: docs
title: Interoperability
description: International Interoperability
menubar: reference_menu
---

# International Interoperability

In an ideal protocol a variety of devices from different countries, states, and businesses will interoperate yet
be able to facilitate each country's own unique requirements. Each country will strike a balance between privacy, security,
freedom of association, and epidemiological analysis and disease control effects.

This page deals with how interopability can be achieved when different countries choose different approaches. They necessarily link to both Protocol and Payload topics.

In this scheme, interoperability is assured even though one country may use a decentralised payload and another a centralised payload.

## Secured payload and International Interoperability example

Below is an example showing the Secured payload, but the same is true of the Simple and your own custom payloads potentially.

If you've not read them yet, please review the
[Envelope header page]({{"/payload/envelope" | relative_url }})
and the
[Secured Payload page]({{"/payload/secured" | relative_url }})

The Envelope header allows routing for any payload. We use the Secured payload as an example because it
provides the requisite security and privacy guarantees and epidemiological information.


![International Interoperability stages](../images/SecuredPayloadInternationalInterop.png)

There are four phases:-

1. Phone A and Phone B detect each other, (optionally) exchange cryptographic material, and share payload data. This is decrypted and validated by Phone B. Only Phone A's data is shown in the above diagram for brevity.
2. Phone B's owner falls ill and shares their contact information with their health service over an encrypted channel. Only the contact with Phone A is shown for brevity.
3. Country B decides to share with Country A the details of Phone A's encounter (but not citizen/phone B's details). Their health service token is decrypted and validated.
4. Phone A downloads the latest 'exposure tokens', recognising its own. It then validates the token, knowing they generated it, and adds the risk subtotal to their current risk.

If the risk total A has now exceeded some threshold then Citizen A can request medical services from their country, and may submit their information. The process above then repeats.

## Second degree notifications

It is possible within, say within country B in the above example, that to gain more control effect over virus spread an additional 'Anonymous exposure token' list is provided. 
Country B could then perform data analysis over all contact information they have on the anonymous contact graph to spot asymptomatic carriers and super spreaders.
Phone B would effectively 'trust' their own country (Country B) to tell them to 'lock down' as a prevention rather than rely on local exposure matching. 

See the [Contact Tracing approaches page]({{"/background/approaches" | relative_url }})
for why second degree notifications are desirable given high virus rates. 
(Note: Second degree notifications are being used in some countries, such as Australia.)

## Formal specification

Please see the [Formal international interoperability specification page]({{"/specs/payload-interop" | relative_url }}) for full details