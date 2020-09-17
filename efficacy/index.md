---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Efficacy Measures
description: How to fairly measure efficacy, and results we have generated.
menubar: efficacy_menu
---

# Efficacy

Whilst there has been much debate about privacy of Contact Tracing applications
there has been little research on creating a general purpose fair measure of
efficacy for the Proximity Detection Protocols that under pin those applications.

There is little point in creating a privacy preserving protocol that provides little
or no epidemiological control of virus spread. Equally, there is little to be gained
from creating a perfectly tracking solution that reveals citizens' exact whereabouts
at all times.

We have concentrated on creating a way to measure the technical efficacy of any
protocol - be it Bluetooth based or othwerwise - on providing information that is
of use for epidemiological control of disease spread.

## The measure & the paper

In our soon to be published paper we describe a method for formally assessing the real
world performance of any proximity detection protocol and its distance analogue (E.g.
RSSI readings for Bluetooth based protocols) that links in with the Fraser Group's at 
Oxford University's previous work on stopping the spread of COVID-19 by using a mobile
application.

We define the 'Efficacy of Contact Tracing' measure used in their paper, and create
a fair and standardised assessment for the 'probability of detection' aspect of that
measure.

This measure and it's exact make up is summardised in the [fair Efficacy Formula Paper](./paper)
page, and detailed precisely in our paper. We shall post a link here once the paper is
published.

## Test results

We then took this measure and applied it to a new protocol we designed from scratch
to work around known Bluetooth communication issues across mobile phones. By using this
formula to highlight shortcomings in certain tests we were able to rapidly within a
month create [a new low level Bluetooth protocol](/protocol) that has a very high 
efficacy score.

As part of preparation for publication we have also been looking at other existing
code bases in the world and carrying out testing on those using the same
[test methodology](./method). We have documented a range of 'top sheet' results in
our [results pages](./results). Please open a GitHub issue if you would like us
to include your own test results.

