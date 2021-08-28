---
layout: docs
title: Herald Documentation
description: Herald developer and integration documentation
menubar: docs_menu
---

# Documentation

Why Herald exists, how to get the code, and how to integrate Herald in to your mobile app.

## Background

Herald is an open source Bluetooth Low Energy (BLE) based protocol licensed under the Apache-2.0 license for the reliable exchange of information between a range of mobile phones.
Herald consists of a low-level [BLE protocol]({{"/protocol" | relative_url }}), with a choice of data [payloads]({{"/payload" | relative_url }}). The [Herald Envelope](payload/envelope) and
[Herald Secured]({{"/payload/secured" | relative_url }}) payloads are examples for contact tracing applications that provide epidemiological information, individual privacy, ensure 
security, and allow international interoperability - even between 'centralised' and 'decentralised' contact tracing applications.

This site also details our low-level [Bluetooth research findings]({{"/bluetooth" | relative_url }}). These include issues with [distance estimation]({{"/bluetooth/distance" | relative_url }}), Mobile phone 
[hardware issues]({{"/bluetooth/hardware" | relative_url }}), and [OS limitations]({{"/bluetooth/os" | relative_url }}).

The aim of this research is to provide a much more reliable way to perform local information interchange between mobile phones. 
This has many potential applications, including [commercial use]({{"/protocol/commercial" | relative_url }}), but the key one we're 
concerned with is the enable more reliable Bluetooth Proximity Detection to improve 
[Contact Tracing]({{"/background" | relative_url }}) applications worldwide in order to help stop the spread of COVID-19.

Our research has been split into several parts. We've also provided an independent introduction to mobile app 
based [contact tracing]({{"/background" | relative_url }}) and provide explanations of the [key terminology]({{"/background/glossary" | relative_url }}) 
of such a system. This aims to demystify the technology and increase public trust.

The remainder of this page breaks down the key areas of the documentation on this site.

## Get the code or get involved

To see information on integrating Herald with your own application, please see the [developer & integration guide]({{"/guide" | relative_url }}).

For those wishing to contribute to Herald's development, we also have a
[Contributor's Guide]({{"/contribute" | relative_url }}).

You can get the code from one of the many [Herald Project Repositories](/download).

## API documentation

You can see the latest API documentation and code coverage reports
on the [API Documentation page]({{"/guide/api" | relative_url }})

## Introduction to mobile app based contact tracing

A [layman's introduction]({{"/background" | relative_url }}) to contact tracing and mobile app proximity detection.

## Paper: Measuring the efficacy of mobile proximity protocols

A [scientific paper suggestion a fair and independent approach to measuring the efficacy of a mobile phone based contact tracing protocol]({{"/paper" | relative_url }}), providing a direct calculation of efficacy that can be used with the Oxford Risk Model to calculate the extent to which such an approach can control the spread of a virtus in a given population.

## Comparing protocols: Formal testing 

A [list of results]({{"/efficacy/results" | relative_url }}) ourselves and others have calculated using our [standardised testing]({{"/efficacy/method" | relative_url }}) method for various existing proximity protocols used for contact tracing today.

## Herald Protocol: Low-level reliable bluetooth communication protocol

During our research on creating an efficacy measure we created the [Herald Bluetooth LE protocol]({{"/protocol" | relative_url }}) implementation to test the ideas. We found a welath of issues in Bluetooth on mobile phones, identified the issues, and workaround them in order to provide a robust communication protocol. 

This low level protocol is independent of any information you pass over it. You could use it for contact tracing proximity detection, but also other uses within your own applications. This low-level protocol provides reliable communications between proximite mobile devices.

## Herald Envelope & Secured payloads: Privacy preserving payloads that can counter hostile actors

We have created a design for a [payload]({{"/payload" | relative_url }}) that not only preserves 
privacy, but also provides the right levels of epidemiological insight to 
prevent virus spread. We show this can be done in a way that ensures security 
of the contact tracing system, and can be trusted by people worldwide as they 
go about their lives. The protocol has international interoperability built 
in and prevents attack from hostile groups or state actors.

The protocol also supports other sample payloads. You can plug in your own 
for your specific application needs. See 
[Custom Outer Payloads]({{"/payload/outer" | relative_url }}) for details.

## Bluetooth low level engineering discoveries

We found a lot of issues in mobile phone Bluetooth implementations and have 
worked around a great many of them. You can read about our 
[Bluetooth discoveries and workarounds here]({{"/bluetooth" | relative_url }}).