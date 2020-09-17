---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Bluetooth Crowding
description: Problems as the number of devices nearby increases
toc: true
toc_title: Bluetooth Crowding
menubar: docs_menu
---

# Introduction

Crowding is an issue whereby many Bluetooth enabled devices are in close proximity
and their pairwise communication becomes so frequent that there is an increase in
response time to any discovery and phone to phone communication.

In testing an early version of our protocol we found that regularly reading an 
ID characteristic as well as a
distance estimation data characteristic (E.g. nearby ID list) would cause the
continuity number to drop significantly after 4 phones were communicating.

## Repeated ID fetching issue

This problem occurs most severely on earlier model phones with strange Bluetooth
behaviours. This is especially true of Samsung A10, A20 and A40 devices. These
devices rotate their mac address every few seconds rather than every 15 minutes.
This resulted in our early protocols having to re-read identity information
every few seconds. The more phones are present, the more of these superfluous 
identity requests were needed.

### Workaround

Our current protocol uses an ephemeral ID in the advertising packet's manufacturer
data area to act as an anchor on phones that exhibit this behaviour. This has allowed
us to drastically reduce the continuity error rate on tests where these phones
are present.

## Characteristic operation response time

All protocols use Advertising to some extent. Many non GAEN protocols will also
use a read characteristic to fetch information about a remote phone. This is typically
the ID characteristic, but can also include advanced distance estimation approaches
too.

In our testing we found that Android phones can sometimes take several seconds, and
in extremis some minutes, to complete a full read of another phone's characteristic
data. We suspect, but have not confirmed, this is an issue with how responses
are returned to applications in Android's operating system level Bluetooth stack.

### Workaround

We now use write characteristics wherever possible, and never repeat any reads that
we don't have to (such as ID reads whilst the remote device has the same mac address).
This has resulted in an average window size of 1-4 seconds rather than over 8 seconds
using just read characteristics. It has also removed all longer than 30 second
responses. 

As a result our continuity error rate is much lower now than before by a considerable
factor.

We therefore recommend write-with-acknowledgement wherever possible where information
needs to be exchanged.
