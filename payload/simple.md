---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Squire simple inner payload
description: Simple inner payload for contact tracing
menubar: docs_menu
---

# Introduction

This inner payload allows for minimum information sharing - just large enough to enable contact tracing.
This makes it simple, but it's not as secure as the [Squire Secured payload](/payload/secured), and
really only supports decentralised contact tracing.

![Simple payload data](/images/SquirePayloadSimple.png)

In particular the simple payload:-

- Doesn't provide a consistent ID for the device the packet is exchanged with (not a problem for decentralised exposure)
- Doesn't provide remote phone make and model, limiting local distance estimation and thus risk scoring
- Doesn't provide verification of the outer packet's validity itself (i.e. doesn't allow a national authority to additional verify the outer packet signature)
- Does prevent replay and relay attacks
- Does allow verification of the remote contact having come from the right phone, if local history is persisted of contact events

## Simple inner payload content

In addition to the information in the [Squire envelope header](/payload/envelope), the simple
inner paylod provides the following data.

Note: All numeric data is big endian.

- Verification token (16 bytes) - The ClientID (16 bytes) of the other phone, encrypted using this phones local, 
ephemeral, private key (32 bytes) for the time period the outer envelope packet was generated in (allowing for local matching
and verification)