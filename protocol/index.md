---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: TBD NAME protocol design
description: Our privacy preserving and secure protocol for slowing disease spread
---

# Introduction

Our sample protocol takes on a variety of privacy and security issues whilst allowing epidemiologists and medical authorities appropriate levels of information to allow them to react to an epidemic and control it using mobile phones.

Note that the Protocol is described here, but the Payload that we are recommending for COVID-19 control is specified in the [Payload](/payload) section. The Bluetooth Protocol we describes is generally useful for any application that requires high fidelity, high density, low frequency data exchange between mobile phones.

## Highlights

- Supports over 96% of UK phones in use. (98% of UK people have a smartphone with Bluetooth Low Energy support.) TODO CITATION FROM OFCOM REPORTS
- Provides 100% detection and a continuity error rate of under 1% in formal testing (See [Results](/results) ) - achieving an over 65% efficacy score TODO QUOTE EXACT FINAL SCORE FROM TESTING
 - Works around the infamous 'iOS cannot be detected in the background' OS bug in iPhones
- Doesn't require operating systems updates in order to be used or improved

## Key Features

- Works on all Android and iPhones dating back to 2010 (Supports over 96% of UK mobile phone users)
- Supports phones and Operating Systems with known Bluetooth performance issues
  - Especially those 16% of UK phones that do not support Advertising, and so cannot be served by other protocols (P3PT, GAEN) - via the write characteristic approach
- Provides a number of approaches to workaround the 'iOS detection in the background' bug in iOS to a point where detection and continuity is superior to existing protocols
  - Allows our protocol to accurately capture the 50% of UK phones that are iPhones where other non-GAEN protocols do not
- Provide distance estimation (RSSI) data with a mean periodicity/windowing time of 4 seconds (easily beating the measure we describe in the [measurements paper](/paper) which requires 1 reading every 30 seconds) - more regular than protocols that today bucket readings to once every 3.5 - 5 minutes (GAEN)
  - Allows a much more accurate Risk Score to be calculated, preventing false positive and false negative COVID-19 exposure alerts
- Supports a pluggable Payload system, allowing the same protocol to be used for a variety of Bluetooth applications, including Contact Tracing
  - Supports large Identity and Distance Estimation payloads (such as those using Elliptic Curve encryption for added privacy and security)
  - We provide a sample [payload](/payload) implementation that protects privacy, ensures security and trust, whilst providing useful epidemiological information

## How it achieves this

- Hardware Approaches
  - Ephemeral ID in advert approach: Some phones also rotate mac address every few seconds rather than once every 15 minutes forcing the ID payload to be fetched, and increasing delay. We work around this using an ephemeral ID approach.
  - Uses a 'write characteristic' allowing phones that cannot advertise (16% approx of UK phones) to tell other phones they are neaby, and still provide regular distance estimation data
  - Android bluetooth stack randomly fails in some circumstances, so our protocol has been extensively tested and checked for these situations to prevent them.
- Low battery usage during the day (thanks to maximum interaction frequency)
  - Only 6% over 8 hours in testing (way better than even having WhatsApp running in the background!)
- Various iOS in the background techniques
  - Allow background iOS phone detection from Android in all conditions (the 'scan, then interrogate' approach)
  - Allow background iOS phone detection from iOS via a nearby Android phone (The 'calling card' characteristic), which can also support more accurate distance estimation via triangulation
  - Optional: Allow background iOS phone detection from iOS directly with location permission turned on (does NOT capture GPS location, just uses the permission to 'awake on screen activation' which happens on average every 10 minutes, and stays active for 5-10 minutes)
- Uses 'connect back' on incoming request to connect to phones that we cannot see, but who see and interact with us (avoiding 'one way detection')
- Uses it's own internal timer implementation on Android to avoid the 'randomly long wait' issue with Android native timers

