---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Bluetooth OS issues
description: Operating System issues
toc: true
toc_title: Operating System Issues
menubar: research_menu
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

iOS has a FIFO queue on issued Bluetooth communication and replies, making the
order in which bluetooth events happen important. If this goes out of sync,
iOS' Bluetooth callbacks will pause until an expected reply is received.

Herald pro-actively manages what is requested over Bluetooth so this issue
does not occur.

See also the 'iOS writes to characteristics' issue, below.

## iOS infrequent advertising detection

According to the Bluetooth specification every advertising event should be
passed on and raised for action. On iOS this

Failed workaround: Restarting scanning
Failed workaround: Restarting advertising

## iOS and Android support long term bluetooth differently

According to the Bluetooth Specification, Bluetooth devices - including software
applications running on phones - should be resumed when the device restarts if
the app was running when the phone was turned off.

On Android this works successfully, and the app will resume effectively 'in the
foreground' for a period of time before returning to background Bluetooth mode.

On iOS only the background Central and Peripheral managers are resumed, not the
full application code and context itself. Protocol and App designers therefore
have to ensure that all configuration is within the BLE classes with only weak
references (if any) to any external classes.

The Herald API proactively manages this state and is built to ensure these classes
are able to survive a restart of the phone on iOS and Android. This allows people
to start their Herald-based contact tracing app once, and forget about it afterwards.

## iOS doesn't always re-report its power state on resumption of Bluetooth scanning

iOS will report it's 'Central' and 'Peripheral' manager power states as callbacks.
Sometimes when Bluetooth is suspended and resumed there will be a poweredOff message
without a corresponding poweredOn message, even though the device itself is visibly
chatting over Bluetooth.

Herald does not rely solely on iOS' reporting of power state, and has timeouts that
for re-enabling of the central and peripheral management functions if inactivity is
detected for a period of time. This timeout is configurable.

## iOS writes to characteristics must be acknowledged

iOS when acting as a peripheral does not support 'writes without ackowledgement', but will
allow you to start a peripheral with this setting. This causes iOS' bluetooth incoming queue
to pause for up to 30 seconds waiting for a write confirmation, after which you will see
an Error Code 6 - Disconnected message in the iOS logs.

Herald uses writes-with-ackowledgement exclusively.

## iOS has unreliable reads

When an app on iOS acts as a peripheral and accepts incoming read requests, quite regularly
the reads will fail and an Error Code 6 - Disconnected will be issued by iOS. There appears
to be no pattern for this behaviour, and seems to just be a stability problem in iOS'
Bluetooth stack.

Herald minimises the amount of reads it performs for this reason, limiting it to the
payload 'identity'. Writes-with-acknowledgements are preferred over reads. These
identity payloads are exchanged just once per Bluetooth MAC address change.

An optional flag allows Herald to share payloads (and thus perform reads) more often.
This is useful for legacy payloads that require sharing every 30 seconds. Whilst this
does result in more Bluetooth data over the air, and more read failures, the connection
maintenance mechanisms within Herald allow this to still be reliable. We have tested this
down to sharing payload every 15 seconds with forced rotation of MAC address every 2 minutes,
without stability issues. So if your payload requires this frequency of payload exchange
it is supported in Herald. Note: This does not affect RSSI distance estimation readings,
which are independent of payload sharing. (RSSI uses adverts only, but for devices that
do not suppot advertising, the received RSSI from the remote can be retransmitted back to
the phone that doesn't support advertising using a write-with-ackowledgement.)

## iOS connecting event state reporting

When an app requests a connection to another device in iOS the peripheral's state will change
from disconnected to connecting. When iOS succeeds in establishing a connection, however, it
will sometimes miss (around 10% of the time) firing a state update to the app to update the
status from connecting to connected, even for the exact same communication packets over Bluetooth.
This appears to be an iOS bug.

Herald uses a timeout detection mechanism to spot this state and force disconnection and then a
connect retry.

## Mismatch in characteristic expectations between android and ios

They have to match EXACTLY in order for responses to work. Otherwise they will fail silently. (E.g. subscribe to notify).
This is a silent failure which will only appear as 'Error Code 6 - disconnected' repeatedly in iOS.

Herald ensures that the underlying advertisement of services and characteristics are handled by Herald,
preventing application authors from making mistakes here.

## Android and iOS bluetooth stacks can fail with no warning or fix

Android and iOS Bluetooth OS Stacks will sometimes just fail. This is transparent to the app and does not
result in any errors thrown - it just provides no data. This requires restart of the phone to fix.
There are various mitigations to avoid this. This issue is not limited to older phones - it
happens on all phones and all Android and iOS versions. 

Herald uses a small subset of Bluetooth standard features that have been tested to be reliable
in order to ensure this does not happen. 

The Herald project have a future feature idea to 
detect and alert the parent app (and thus the user) when this occurs (E.g. caused by another app
on the same phone). 

## Some Android devices report capabilities incorrectly

Huawei devices report that they support advertising when they do not.

Some Samsung J-series devices state that they do not support advertising (and thus the programmer cannot request it) even though the hardware should support this capability.

Both of these issues result and exceptions in the Bluetooth stack that, without correct handling or device detection, can cause the underlying bluetooth layer in the app to crash, sometimes undetectably. This leads to contact tracing applications to not actually provide benefits to these phones' users.

Herald uses heuristics to determine the capabilities of the phone it is running on, and dynamically uses the approach necessary. Herald includes support for phones that do not themselves support advertising, but do support scanning, and vice-versa.

Herald does this by correctly trapping errors or detecting these models the programmer can prevent crashes, but this leaves the problem of one-way detection. (I.e. this device can see other devices, but not be seen as an advertiser). 

This in turn can be worked around through a 'write characteristic' allowing these devices to write their identity information and the signal strength they have seen from the advertiser, back to the advertiser. 

This is like tapping a person on the back to let them know you are there and leaving your business card.

## Kotlin vs Java differences

For some reason using Kotlin, the default programming language on Android, rather than Java leaves 
to poor Bluetooth performance and missed callbacks for some state events.

The Herald protocol uses Java for this reason. It can still be used by a Kotlin based application but does not suffer from the performance issues of Kotlin Bluetooth libraries.

## Missing disconnect events

Sometimes iOS will raise a disconnect event on a connection but not send this information to the remote device.

Herald manages the low level connection state and has a series of timeouts or 'last seen' and 'last updated' times
that it uses to detect such a disconnect.

## Android 28 vs 29 bluetooth stacks behave differently

Scan, connection maintenance, and device state handling appear to be very different between Android 28 and 29.

The Herald team test every develop-branch push against a wide range of devices, including older Android
devices, in order to ensure stability on these platforms. Herald only uses Bluetooth standard features
that have been tested to be reliable.

## Unreliability of Timers across Android devices

This problem occurs when scheduling a delay between Bluetooth scans, which are controlled by the
app on Android and not the OS. You can ask for "Awake in 8 seconds". Rather than always awaking just after
8 seconds, Android's default timer class can wait for over 30 minutes.

The solution within Herald is to provide [our own Timer implementation](https://github.com/vmware/herald-for-android/blob/develop/herald/src/main/java/com/vmware/herald/sensor/ble/BLETimer.java)

## Inability to use timers on iOS without extra permissions

Background awake timers on iOS cannot be used without additional permissions. For some
reason an app with background location or audio permissions will be able to use timers
in the background. Otherwise the timer will never trigger after a period of background
activity.

Herald works around this by using Bluetooth callbacks to trigger an activity loop, effectively
waking Herald's activity if another device running Herald is nearby. Herald also 'pushes ahead'
a timer by a few seconds, within the ten second limit of iOS background app activity, so that
the timers stay effective over a long period of time.

An alternative workaround is to use a 'keepalive (notify) characteristic' in Bluetooth. This
does require a persistent connection between devices. This effectively means the activity loop
on one phone sends out an update over Bluetooth, waking up other nearby Herald phones who are
subscribed to that characteristic. Herald does have this mechanism enabled, although we only
keep connections open persistently between two iOS phones. This is an additional background
wake safeguard.

## Adjunct problems

Issues that affect contact tracing apps, but which aren't Bluetooth specific per-se

### Security: Availability of Elliptic Curve encryption support

Elliptic Curve encryption is only available on iOS since iOS 10.7. On Android a third
party library such as Bouncycastle can be used to provide ECIES support.

This means an app that uses ECIES to encrypt payload information, or to communicate
with a back end service, will not support all of the devices currently in use within
a country.

### Security: RSSI, transmit power, and trust

It is possible for attackers to 'fake' certain data sent over Bluetooth especially if they
use a hobby electronics kit to pretend to be a phone.

RSSI (Signal Strength) and Transmit Power (TxPower) are both used for distance estimation
in any contact tracing app. RSSI is calculated on the receiving device based on physical
parameters of the received signal and is not provided by the sender.

Transmit Power is provided by the remote device as a setting within the advertisement
it sends out a few times per second. This could, theoretically, be faked. In reality
most phones (especially iPhones), no matter their battery state, will used a fixed
transmit power. Thus a service that determines the phone make and model during initial
installation can know the likely bounds, or pre-set value, of transmit power without
having to rely on the advertised level.

### Security: Issues on iOS with OS bluetooth advertisements

Previously [published security issues](https://hal.inria.fr/hal-02394619) (External link)
 relate to Apple's manufacturer specific internal
protocols that use Bluetooth as a mechanism for communication. These potentially
reveal more information than current COVID-19 protocols.
