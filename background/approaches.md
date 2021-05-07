---
layout: docs
title: Technological approaches
description: Technology approaches to contact tracing
toc: true
toc_title: Technology Approaches
menubar: docs_menu
---

# Introduction

There are many approaches that are standard in Bluetooth protocols that are relevant for
contact tracing protocols generally. These are summarised in the below list.

## Standard approaches

These are approaches used in many protocols

### Advertising

All Bluetooth protocols store some information in their adverts. All have service and 
characteristic information. Some use the extended space in their adverts to act as the sole 
means of payload transmission.

This approach is limited as many devices (16% of UK mobile devices) do not support 
advertising, only reading others' adverts.

### First degree contacts

This is where one phone interacts with each phone nearby directly, not via another device.
When someone becomes ill just their contact information is used to notify other potentially
ill people. In this approach we do not immediately ask first degree contacts for their
contact information too, unless they become ill or test positive. All current protocols
support this approach. 

See Second and Third degree methods below for enhancements.

## Extended approaches

This are further approaches that can be used to increase effectiveness that are not common place.
 We have implemented all of these in our own [Protocol]({{"/protocol" | relative_url }}).

### Write characteristic

We have found that Android phones' speed when reading characteristics is much slower than its 
support for 'write and ackowledge'. Using write instead of read for distance estimation allows
a much lower continuity error, increasing accuracy, and reducing mean window times from above 8
 seconds (minutes for some phones) to 0.5 - 4 seconds, depending on handset and number of 
 devices nearby.

We there use a write characteristic wherever possible in [our own protocol]({{"/protocol" | relative_url }}).

Note: You have to use 'write and acknowledge' as iPhones do not support 'write without 
acknoeldgement' when they act as 'peripherals' (advertisers) themselves.

### Calling card characteristic

Once a device starts maintaining a list of recent contact events, distance estimations, and identifiers
it can act as a notice board. Other devices can ask what nearby devices it has seen in the last few
seconds and can log these contacts.

This is useful in two situations:-
- Some devices do not support Bluetooth Advertising themselves, but can discover and connect to
other devices that are advertising. This is 14% of UK phones, for example, which is quite large
- Apple iOS background bluetooth advertising bug - iOS suffers from a bug where applications on
two backgrounded iOS devices are not told about each other even though the iOS operating system
itself sees the background advertisements. Two two backgrounded iPhones cannot detect each other.
Apple has implemented a workaround for this in their own GAEN protocol, but has not made this
fix generally available for all apps running on their platform.

By using a Calling Card approach, two such devices like those described above can communicate
via an intermediary phone that does not exhibit these behaviours.

This leaves you with two distance estimations. One from phone A to phone B, and one from
phone B to phone C. So long as the identifiers and distance estimation values for A-B and B-C
are known by both phone A and C, they can still estimate each others' minimum and maximum potential
distances.

As an extension to this, the more phone B's you have the more accurate you can be. You could use
multiple calling card readings to triangulate distance. In fact this approach could also make
distance estimation more generally accurate for all phones if done as routine, overcoming
some of the issues in [distance estimation]({{"/bluetooth/distance" | relative_url }}) calculations.

### Custom Android timers

The background 'wake after X seconds' timer on Android sometimes becomes 'stuck' and does
not awake for many minutes. Creating a custom timer based on OS events allows applications
to more effectively perform regular timed actions from a main event loop. This in turn
increases detection and greatly lowers continuity error in our testing.

### Pseudo Device Address

Some older Android phones change their network identifiers so often that the airwaves become 
crowded with communication. We use the advert manufacturer's data area to include an 
ephemeral pseudo device address that changes on the same period as the MAC address (15 minutes) to work 
around this issue. This is described in detail in the 
[Herald Formal Protocol Specification]({{"/specs/protocol" | relative_url }}).

## Additional efficacy approaches

There are several mathematical extensions to a centralised contact tracing protocol that 
could raise the control factor on the spread of a virus. These are briefly described below, 
but not quantified in this paper:-

### Taking multiple readings per window

Rapidly taking multiple RSSI readings per device will allow an estimation of RSSI modal value. 
A modal value will be more reliable than a single reading of a mean of a small number of 
values, but requires more readings to be accurate.

### Low estimation filter

If RSSI values on Bluetooth, for example, are below a certain level, do not try to exchange 
IDs until it is nearer. There will be a normalised probability function that can predict for 
all phones the likelihood that a phone is within the 8 metre range of epidemiological 
interest[3]. Filtering out some phones from information exchange can lead to higher 
throughput and less cross talk, improving continuity and minimising data that needs to be 
held. It also helps with privacy.

### Second Degree contact tracing

Rather than just asking those who themselves have had direct confirmed exposure to one or 
more people with COVID-19 to isolate for 14 days, these people's contact data could be 
asked for from first degree contacts at the same time prior to them being tested for COVID-19 
in order to accelerate the control effect of automated mobile app based contact tracing. 
This would be useful near the peak of an outbreak.

This mechanism and its additional effectiveness is described in the Fraser Group paper
[Effective Configurations of a Digital Contact Tracing App. Retrieved 31 August 2020. ](https://github.com/BDI-pathogens/covid-19_instant_tracing/blob/master/Report%20-%20Effective%20Configurations%20of%20a%20Digital%20Contact%20Tracing%20App.pdf) 
See Figure 4 and its accompanying text.

### Third Degree contact tracing

As above, but for one more degrees of contact. Would lead to a large number of people self 
isolating. Useful just before a national lockdown is required.

### Use of Beacons in populated areas

Rather than using QR codes in venues or other complex manual mechanisms, instead provide a 
companion app for phones and tablets in restaurants, cinemas, bars, workplaces and other 
venues where people gather. This would capture the presence of someone using a contact tracing 
app in a privacy preserving way (compared to handing over your personal details every place 
you visit) and allow for automated notification. It also minimises the cost to businesses 
of assisting with contact tracing, and is likely to increase the number of premises 
complying with assisting with contact tracing measures.

Herald implements 'check-in' beacon support as of v1.2 (Dec 2020). Please see the
[Beacon Payload overview]({{"/payload/beacon" | relative_url }}).
or the [Beacon Payload Formal Specification]({{"/specs/payload-beacon" | relative_url }}).
for further details.