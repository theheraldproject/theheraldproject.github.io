---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Bluetooth OS issues
description: Operating System issues
toc: true
toc_title: Operating System Issues
menubar: docs_menu
---

# Introduction

OS based issues

## Operating system support

Some older Operating Systems do not support BLE. These handsets are very rare, however, and 
typically date to early 2010. In the UK less than 2% of mobile phones will have this issue. 
Bluetooth Low Energy support is uniquitous in 2020 in the UK.

## OS hides underlying bluetooth activity

iOS doesn't provide MAC addresses, or allow 'show me all BLe devices nearby' style scans.
Application developers can only request to be notified of devices running specific services.
Normally this isn't an issue, but iOS has a bug whereby it does not 'see' other iOS devices
who are advertising whilst in the background (screen off, app in background, the typical state of
a contact tracing application). This bug has existed since 2011 when introduced in an iOS update.
This is not a deliberate privacy feature of iOS. Apple has been notified and is yet to issue a 
fix. (as of Summer 2020, iOS 13.6.1)

Until Apple fix this bug in passing advertising information to applications iOS to iOS direct
background detection cannot be fixed.

### Workaround

The first workaround is to enable Location services in the application. The app doesn't have to
request a GPS location, but having this permission allows the app to subscribe to the 'screen on'
event. So every time a user looks at their phone (once every 10 minutes according to research)
the background bluetooth app will be knocked in to foreground mode for 5-10 minutes, allowing direct
detection. (It should be noted that Android apps have to be given the location permission 
already just to use Bluetooth, so this is no great barrier in itself)

The other, more reliable and less privacy concerning, mechanism is to use a nearby Android device
as a relay. iOS devices will see other Android (FG or BG) or foreground iOS devices when 
iOS is in the background, just not other
iOS backgrounded devices. Thus they can exchange identity information with these devices. By leaving
these 'calling cards', devices can request 'have you seen anyone else nearby in the last few seconds?'
These nearby identities can be exchanged. 

The remaining challenge is that of distance estimation. Instead of a pairwise single distance with
error bound you now have two sets of RSSI information - from Phone A to Phone B, and Phone B to Phone C
. Thus you have a minimum and maximum distance, each with an error bound. You therefore need two sets
of RSSI and transmit power information to do a distance estimation.

In reality, the more phones that are nearby the easier it is to be accurate. This is because receiving
multiple sets of nearby data allows a phone to triangulate the relative position and thus distance
of the backgrounded iOS device(s). The errors that result are not significantly greater than a single
distance estimation and so do not represent a degradation of distance estimation.

## iOS background discovery and discoverability

One-hot encoding (binary)
New variable hex encoding (iOS 13.5, 13.5.1)

Workaround from android: Ask for services and chars from any device seen (even if not 
decipherable in manufacturers data)
Workaround in iOS: Write characteristic
Pairwise iOS workaround: Calling card characteristic


## Concurrent connections limitations


## iOS 30 second response timeout

## iOS infrequent advertising detection


Failed workaround: Restarting scanning
Failed workaround: Restarting advertising

## iOS and Android support long term bluetooth differently

On restart - android

Resume in background - iOS

## iOS doesn't always re-report its power state on resumption of Bluetooth scanning

## iOS writes to characteristics must be acknowledged

## iOS peripheral details caching

Cannot rely on iOS' reports of remote device state whatsoever

Workaround: Build own state machine based on 'last successful X'

## Mismatch in characteristic expectations between android and ios

They have to match EXACTLY in order for responses to work. Otherwise they will fail silently. (E.g. subscribe to notify)

## Some Android devices report capabilities incorrectly

Huawei devices report that they support advertising when they do not.

Some Samsung J-series devices state that they do not support advertising (and thus the programmer cannot request it) even though the hardware should support this capability.

Both of these issues result and exceptions in the Bluetooth stack that, without correct handling or device detection, can cause the underlying bluetooth layer in the app to crash, sometimes undetectably. This leads to contact tracing applications to not actually provide benefits to these phones' users.

### Workaround

By correctly trapping errors or detecting these models the programmer can prevent crashes, but this leaves the problem of one-way detection. (I.e. this device can see other devices, but not be seen as an advertiser). 

This in turn can be worked around through a 'write characteristic' allowing these devices to write their identity information and the signal strength they have seen from the advertiser, back to the advertiser. 

This is like tapping a person on the back to let them know you are there and leaving your business card.

## Kotlin vs Java differences

For some reason using Kotlin, the default programming language on Android, rather than Java leaves 
to poor Bluetooth performance and missed callbacks for some state events.

### Workaround

Our low-level Bluetooth protocol uses Java for this reason. It can still be used by a Kotlin based application but does not suffer from the performance issues of Kotlin Bluetooth libraries.

## Missing disconnect events

## Android 28 vs 29 bluetooth stacks behave differently

## Android bluetooth stack can fail with no warning or fix

Requires restart of phone
Various mitigations to avoid

## Unreliability of Timers across Android devices

## Inability to use timers on iOS without extra permissions

## Workarounds with keepalives

## Workarounds with customer timers and triggers


## Adjunct problems

Issues that affect contact tracing apps, but which aren't Bluetooth specific per-se

## Security: Availability of Elliptic Curve encryption support

## Security: RSSI, transmit power, and trust

## Security: Issues on iOS with OS bluetooth advertisements

Previously published security issues