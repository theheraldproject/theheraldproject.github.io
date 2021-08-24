---
layout: docs
title: Frequently Asked Questions
description: Frequently Asked Questions about Herald
menubar: about_menu
---

# Frequently Asked Questions

Below are some common questions we get asked about Herald

## Section 1: The Herald Project

### What is Herald?

Herald is an Application Programming Interface (API) that enabled mobile apps,
wearables, and beacons to detect each others' presence, estimate distance,
and exchange a variety of types of information in a reliable mechanism.

### How does Herald provide proximity information?

Today Herald uses Bluetooth Low Energy (BLE) to do this. There's nothing in Herald
tied to just this one protocol. This means that as technologies change the Herald team
can look at adopting additional mechanisms. Examples include high frequency audio and
ultra wide band (UWB) radio.

### Where did Herald come from?

Adam Fowler was the originator of this project. After working on DCT apps in early 2020
Adam decided to research the fundamental need as part of the Fair Efficacy Formula paper.
To test the ideas in this paper he created a Proximity API. Once he knew it was effective
he had to share it. As Adam worked at VMware Inc. he asked the VMware opensource team for help.
With their assistance code hosting, web hosting, a name and logo were created. Herald was born
in July 2020 and was published in August 2020.

### Who 'owns' Herald?

Herald is now a 'Series' under the Linux Foundation Public Health (LFPH). This means
Herald is its own entity managed by its volunteers with help and advice provided by LFPH.
Herald was donated to LFPH by VMware Inc. but is now managed by the Herald Project's
Contributors.

### Do I have to pay to use Herald?

No. Herald software, documentation, and hardware designs are all free and open source.

### What do you mean free!?! Don't you charge for your IP?

No. Free. We license Herald under permissive opensource licenses. This means you are
free to use it within either free or even commercial software if you wish.
These licenses include patent protection clauses too, protecting our adopters.

### What support do you provide?

We're an opensource project consistently of volunteers who work on this in our limited
free time. We provide best-efforts support via GitHub issues only. We do maintain close
contacts with Public Health Authority (PHA) adopters and help where we can to ensure
the software is beneficial to everyone, but we don't provide formal support or try
to achieve any KPIs / SLAs on our support turnaround times. 

The opensource Herald team do work with commercial organisations that could provide
support. We don't recommend any though - you'll have to decide on them for yourselves.

## Section 2: Herald and Digital Contact Tracing (DCT)

### Is Herald a DCT app?

No. Herald is the name of the opensource project that provides an API for device proximity
detection and data sharing. This is a foundational technology used in several types of
application, one of which is Digital Contact Tracing.

Whilst we provide demonstration software as 'Herald sample apps' these are strictly NOT
for production use. Herald is used within others' DCT apps for mobile phones and wearables
though.

If you are looking for an end-to-end whitelabled Digital Contact Tracing system, look
at our [OpenTrace system]({{"/opentrace" | relative_url }}) pages.

### How does Herald compare against *INSERT NAME* DCT protocol?

Herald has been developed to ensure it is the most effective DCT protocol at providing
regular, accurate distance analogue (E.g. RSSI) and risk calculation capabilities which
DCT app teams can use to achieve their specific needs.

Herald detects devices quickly, communicates with them quickly, and exchanges information
between devices rapidly and reliably. Herald does this whilst minimising data exchange,
 data storage, and battery use.

If you want to know how it compares against *INSERT NAME* DCT protocol, then try
applying the [Fair Efficacy Formula]({{"/efficacy/paper" | relative_url }}) 
approach to that DCT protocol. And then let us know
the results as we'll happily publish them publicly on our website, no matter what they say.

### Can the Herald team provide me with a DCT app?

The Herald team consist of volunteers working in their spare time on eHealth solutions.
We do not get paid for this work and we do it on a best efforts basis. This means we
unfortunately cannot directly create DCT apps for your organisation.

There are companies that can provide this service. Whilst we don't recommend any - 
that's a decision for you - we can pass along some names privately if it is of interest.

### What is Herald's secret sauce?

We looked at the epidemiological need, then built Herald to meet it. We didn't create
a restrictive software product and then try to make it fit. We continue to carry out
new research in to areas that no-one else does. 

We're working on accurate distance conversion from RSSI - an area of research that 
some, we believe, have prematurely abandoned. We are also publishing millions of datapoints
on distance and RSSI readings for a variety of devices, and have built automated data
collection robots to do this. All of these approaches and our data are free for other
research teams to use under a permissive license.

### What's your end goal with Herald and OpenTrace?

We aim to put an end to blanket geographic lockdowns for new diseases and disease variants.
We will provide foundational technology to avoid a repeat of the COVID-19 crisis, and 
continue to perform research to perfect and improve this technology.

We want to make the entire end to end system low cost, reliable, and give people the ability
to deploy it to all communities worldwide, helping to protect the health of everyone.

We also believe this technology can have eHealth benefits outside of disease control.
We continue to be on the lookout for new application areas for the Herald API.