---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Herald protocol - custom uses
description: Other uses for the Herald protocol
menubar: docs_menu
---

# Custom apps and the Herald Protocol

The low-level Herald protocol allows for regular, reliable, information exchange between mobile devices.

As well as being used for contact tracing, other applications can be built on top of this reliable
mechanism that were not possible before.

## Application examples

There are a range of example for this. See the [Applications page]({{"/applications" | relative_url }}) for details.

## How to apply the Herald protocol for these apps

It's a pretty simple process:-

- Provide your own unique Service and Characteristic UUIDs for your application
- Set the performance needs of your app:-
  - Distance estimation on/off
  - RSSI communication cut off filter (i.e. how near do devices need to be to talk)
  - Minimum time delay between exchanges
- Set your security information
  - Cryptographic material for ClientID rotation
  - Cryptographic public/private key for secure data exchange (optional)
- Provide your own [Outer Payload]({{"/payload/outer" | relative_url }}) callbacks to allow data exchange
  - Given a nearby ID and distance information, what do you want to share with them?
  - Allow passing data via a nearby device (Calling card - useful to workaround certain phone limitations/bugs) (optional)
- Implement necessary callbacks to receive shared data in your app

