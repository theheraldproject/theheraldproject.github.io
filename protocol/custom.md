---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Squire protocol - custom uses
description: Other uses for the Squire protocol
menubar: docs_menu
---

# Custom apps and the Squire Protocol

The low-level Squire protocol allows for regular, reliable, information exchange between mobile devices.

As well as being used for contact tracing, other applications can be built on top of this reliable
mechanism that were not possible before.

## Application examples

There are a range of example for this, with a few summarised below

- Communication apps
  - File sharing between Android and iOS devices, reliably
  - Local 'same location' peer to peer applications, such as instant messaging or gaming apps
- Safety apps
  - Using beacons in high-risk areas, an employee exposure app could accurate record exact exposure to hazardous environments
  - Also using beacons, know where to deep clean if an employee does fall ill at your large campus
  - Check in app - Walk around and be let in to secure areas automatically
  - Rescue app - E.g. for skiing/snowboarding avalanche rescue - find the hidden/non visible person. Could be fire in a large building, or rescue on a tube train

## How to apply the Squire protocol for these apps

It's a pretty simple process:-

- Provide your own unique Service and Characteristic UUIDs for your application
- Set the performance needs of your app:-
  - Distance estimation on/off
  - RSSI communication cut off filter (i.e. how near do devices need to be to talk)
  - Minimum time delay between exchanges
- Set your security information
  - Cryptographic material for ClientID rotation
  - Cryptographic public/private key for secure data exchange (optional)
- Provide your own [Outer Payload](/payload/outer) callbacks to allow data exchange
  - Given a nearby ID and distance information, what do you want to share with them?
  - Allow passing data via a nearby device (Calling card - useful to workaround certain phone limitations/bugs) (optional)
- Implement necessary callbacks to receive shared data in your app

