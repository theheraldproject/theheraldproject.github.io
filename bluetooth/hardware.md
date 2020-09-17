---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Bluetooth Hardware issues
description: Hardware issues affecting mobile phone's hardware performance
toc: true
toc_title: Hardware Issues
menubar: docs_menu
---

# Introduction

This section describes hardware issues encountered during testing and 
evaluation.

## Bluetooth version support

Bluetooth Low Energy as a technology was introduced in Bluetooth 4.0 and 
extended in 4.2 and 5.0. It consists of advertisements across all three 
advert channels, and 7 or more (in later versions) data transport channels 
chosen through negotiation during a connect event.

Thus not all phones that support Bluetooth support Bluetooth Low Energy, 
although practically any phone released since 2010 support Bluetooth Low 
Energy.

In our research we found 94.49% of the UK population has a mobile smartphone 
(all ages), and 97.5% of those support Bluetooth Low Energy (summer 2020).

## Battery life impact

If constant scanning and interaction occurs then mobile phones will run 
out of battery very quickly. They will also get uncomfortably hot.

### Workaround

The simplest workaround is to perform a scan for 1-3 seconds but with a 
gap of a few seconds between each scan-identify-estimate interaction.

In our testing this results in a battery drain of approx 6% in 8 hours of 
use. This is a minor drain and much smaller than using WhatsApp for the 
same period of time. Any reasonably designed protocol should be able to 
achieve this low level of drain.

## Lack of advertising support

The below devices (and perhaps the entire series of devices) do not support 
Bluetooth Advertising. This means that protocols that are 'advertising only' 
such as P3PT and GAEN (Apple-Google) can never support information transfer 
from these devices or detection of these devices for the purpose of contact 
tracing.

- Samsung J6
- All Huawei phones tested (Psmart, P20)
- Many older phones

This is a major issue as in our research approx 14.37% of UK mobile phones 
currently in use (Summer 2020) do not support advertising. This is a much 
larger issue than the proportion of the population without a phone. This 
is a major percentage of the population and so a workaround should be used.

### Workaround

The way around this is realise that this device can still discover, read and 
write to bluetooth peripherals. This the best workaround is for this device 
to write its own information, and the RSSI it has seen from the device it 
is writing to, to the device that does support advertising.

## Interaction forces bluetooth mac address changes

Some hardware will rotate its mac address (network identity) on every interaction rather than
every 15 minutes as standard. The effect of this
is that it becomes a 'new device' every time it does this, meaning the remote device
seeing it forgets this devices identity, causing it to ask for identity information again. 

This can reduce
detection and continuity measures in performance terms and also use a lot more battery, as well
as taking up more bandwidth exchanging this information, increasing crowding issues.

The following devices are known to present this issue:-

- Samsung A10
- Samsung A20
- Samsung A40
- Samsung Galaxy Note 8

Note: Samsung A70 seems fine. Samsung S series seem fine.

### Workaround

By using the 'spare' space in the advertising packet a rotating ephemeral identifier can be provided. 
By using this ID rather than just the mac address a device seeing the advertisement with a new mac 
address will still know the underlying identity of the device has not changed and so will not 
need to 'read' identity from the device.

It should be noted that the ephemeral spare space is approx 31 bytes and so is too short to
exchange a cryptographically secure identifier that would prevent reverse engineering of identity.
This means it's still desired to read the full identity (in our protocol over 100 bytes), but
the ephemeral identity does provide a means to minimise data exchange.

This reduces identity requests from once per 4 seconds, say, per device that discovers it to once
every 15 minutes (on mac address and ephemeral identity rotation, as normal).
