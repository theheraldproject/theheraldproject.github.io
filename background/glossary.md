---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Contact Tracing glossary
description: Demystifying technical terminology
toc: true
toc_title: Glossary
menubar: docs_menu
---

# Introduction

There are a myriad of confusing terms used in contact tracing and proximity detection
protocols and apps. This page acts as a standard glossary. This terminology is used 
throughout our website, research and published papers.

## Contact event

The time period between the point at which two devices in the possession of their owners first detect each other as being proximate until their last detection. This is a pairwise activity, but it is possible that one bluetooth device discovers another at different time periods. This is due to the different power and reliability of the bluetooth chipsets in use on particular phone models. Thus it is usual for a contact event to begin and end with 'one way detection' - i.e. one phone detecting another prior to itself being detected. 

It is also possible for two people to record multiple contact events from each other during a given day. Consider an office environment with a general working area where people congregate and which has meeting rooms spread out throughout the office. Between meetings people will record contact events with the same people who are in the general workspace area. See Accrued Risk, below.

## Detection

The point at which a device first discovers another running the same protocol and takes a distance estimation (E.g. an RSSI reading for Bluetooth). At the same time, or just after, this device is also identified through its transmitted identifier. Detection for the purposes of bluetooth is the point in time that a scan for a bluetooth device running the same protocol discoveries that device, identifies it, and records a distance estimation (Note: RSSI value is provided as part of the discovery callback on Android and iOS bluetooth protocol stacks). It is thus a boolean value between two communicating devices. i.e. has A seen B, and has B seen A? (This is two 'detections')

For the practical purposes of contact tracing, detection frequency is generally limited in order to conserve battery or prioritise the quantity of devices discovered over the frequency of detections. Risk algorithms, though, require a duration of time at a particular distance. Thus in all Bluetooth protocols for proximity detection it is usual to take the point in time the detection happened and count the duration as the time between this detection and the next detection, or this time plus the average target detection frequency. i.e. the final detection segment will be assumed to have a duration of the protocol's detection period rather than the point in time of this last detection. This can lead to false positives when this time window value is large, adding inaccuracy to the calculated risk score. As an example if there was a window of 5 minutes the final segment would be 5 minutes even if the contact went out of range after 1 second. Smaller windows, therefore, will naturally have less false risk accrual.

Thus the duration of a contact event is from the point of first detection until the point of last detection, plus the period of detection scans. A contact event consists of many detections over time.

It should be noted that in this paper we use the measure for continuity to assess the efficacy of detection over time.

## Identification

It is possible that two devices detect each other but do not record enough information to attribute a detection to a contact event because the identity information is not yet shared, or has been received malformed. Whilst detection is most important to measure it should be noted that this is for nought and cannot be used for risk calculations unless the detection can be attributed to an individual who is unless with a disease. Thus only detections that have been attributed to a device can be included within a contact event. For the purposes of this paper when we say detection we always mean an attributed and identified detection with at least one distance estimation reading.

## Transmitter & Receiver

Due to the complexities of communication over unreliable networks - including Bluetooth radio communication - it is common for these low level protocols to perform request/response sequences of varying complexity for each transmission and receipt of data. Both sides are thus 'transmitters' and 'receivers'. This makes a mental model very hard to understand, and specific to the underlying protocol.

Instead of this we propose to treat the advertisement part of a protocol announcing its presence as a contact tracing app protocol to other devices to be the 'transmitter' and the discoverer of this service which then may interact with it as the 'receiver'. Most phones will act as both transmitter and receiver. The above definition helps to provide a standardised terminology across protocol types (Bluetooth, Ultrasound, UWB, etc.)

As an example of the reality in bluetooth, a scan would result in a scan request being sent from the receiver, and advertising data being sent as a response by the transmitter. Thus each device is both transmitting and receiving in reality, but as a mental model one is the 'receiver' and the other the 'transmitter'. In Bluetooth terms the advertiser, or peripheral, is the transmitter whereas the scanner, or central, is the receiver. This may be non intuitive to developers of bluetooth enabled peripherals as, often, it's the mobile phone central that would normally be transmitting most information. An example of this would be audio sent to a bluetooth speaker peripheral. For our purposes though, the above definitions make it easier to understand interactions independent of the underlying proximity detection protocol when it comes to contact tracing applications.

## Accrued Risk

Much of the public advice on Covid-19 has centred around individual pairwise contacts being 'risky'. The most common advice to the public is that a risky contact is one at “2 metres for 15 minutes”. This is high-level simplified advice meant for consumption by individuals, and not a sophisticated epidemiological model. Unfortunately, diseases do not know they are meant to follow these guidelines and so the accrual of risk in reality works much differently in the epidemiological models.

An appropriate analogy here is helpful to understand accrued risk. Consider a nuclear power worker who wears a radiation detection indicator. The bar on this indicator increases whenever that person is exposed to radiation. As they go about their daily routine across different areas of risk the bar grows taller. Perhaps a safety regulation says when this bar reaches a particular risk threshold they must take 14 days off. This is analogous to how disease exposure works with recommendations for getting tested or self isolating for a period of 14 days. Risk is accrued over time across many events, not as the result of a single event.

The time the person must take action to preserve their health is not dependent upon a singly 'risky' event, but rather every event whether it be many low risk events or a single very risky event. The exposure accrued in total is what a corrective health action is based on, and not a 'single risky contact'.

Thus whilst much of the public debate has centred on the simplified 'single risky contact' advise, an application which solely relies on this will discount the accrued risk over time, potentially leading people to accumulate much more risk, and thus viral load, before corrective action is advised or taken - if ever advised. In this paper we do not consider 'single risky events' as they are a poor model when compared to real world epidemiology. This accrued model of risk scoring calculation is discussed in detail in other papers[2]. Thus our discussion on accrued risk may differ from the original protocol authors' explanations of their own protocols, but for good reason, as the above definition represents the reality of disease exposure.

## Proximity Detection Protocol

This is a protocol designed to enable two mobile devices in possession of a person to detect, identify, and record data useful for distance estimation and thus later risk score calculation. We may refer to just 'protocol' in this paper. It is the underlying protocol and not the application built on it. Examples include The Google-Apple protocol (aka Gapple or GAEN) or the new [protocol](/protocol) defined by ourselves.

## Contact Tracing

This is an application and set of associated healthcare control processes used to take proximity detection data and provide a healthcare response to slow the spread of a disease within a population. The “NHS Covid-19” mobile app is the UK contact tracing application. These applications always have a central healthcare system used to report individual (even is pseudo anonymous) status to, and to help report on and manage the spread of a virus. 

Thus the 'centralised' and 'decentralised' classifications of proximity detection protocols is a misnomer as every protocol must have one or more centralised support systems. For the Google-Apple (GAEN) protocol this would be the ID server that hosts ill people's IDs as well as the a country's healthcare systems that the contact tracing apps built on it reports to. For other protocols this is just the backend healthcare system for a particular health jurisdiction. (State, Country)

It should be noted that a single blanket 'risk threshold' that applies to an entire country's population may not be as useful to controlling the spread as one that can be varied by city or county. To avoid blanket local lockdowns, for example, a national health authority may wish to lower the risk threshold in a particular area in order to remove the need for a local lockdown. With a decentralised model there may be no way to track the accrual of risk in a particular area in advance of positive cases being identified, reducing the ability of a health care system to respond in a timely manor to an outbreak before a spike in a local population has already occurred.