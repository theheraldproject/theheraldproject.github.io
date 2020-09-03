---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: The Fair Efficacy Formula paper
description: Our fair efficacy formula for comparing proximity detection protocols
---

# The Fair Efficacy Formula paper

We took the efficacy measure mentioned in the Ferretti et al paper and reasoned
the constituent parts that affect the probability of detection of 30 second
segments of any contact event.

The resultant formula is below:-

![Fair efficacy formula](/images/paper-measure-formula.png)

Where:-

- DET-Rate is the Detection rate - the proportion of pairings that were detected (one-way)
- Delta flag buckets - The number of 30 second windows across contact events that were correctly observed
- Longevity - The difference between delta flag buckets in the first and final hours of the 8 hour test
- P-tested - Made of all of the above from physical testing
- Mean error DA - The mean error in distance analogue estimation at a range of distances vs the expected distance analogue value (E.g. RSSI)
- Delta error RT1 - The error in distance estimation analogue in the most hazardous (nearest) part of a contact evening. RT1 is the period around minimum distance that would constitute a risk score equivalent to 15 minutes at 2 metres. This is separate at in the Oxford Risk Model the vast majority of the risk score is achieved at the nearest distance (under 8 metres).
- PROB-di - This is the probability of detection from the Ferretti et al paper, and is calculated from the above measures
- P-squared-spec - The population of a country of interest (UK in our paper) that has a mobile phone with the right hardware, OS and software versions to support the protocol under assessment
- FRAC-re - The fractional RE percentage representing the control effect of an app on the virus (a value of 0.9). From the Ferretti et al paper
- E-T - The Efficacy of Contact Tracing measure as per Figure 3 in the Ferretti et al paper.

E-T is what we call the final 'proximity protocol efficacy measure' and is the value that describes efficacy, and the main output of our paper.

The paper itself goes on to define a range of additional measures that are of use when comparing protocols that are quite similar.

These are comprised in totality of:-

- Population reach, by specification - percentage of a country's population that can be serviced by a protocol (theoretical, no testing required)
- Population reach, by testing - percentage of supported user's that will be effectively serviced by a protocol
- Continuity coverage, one of provides continuity coverage, provides partial continuity coverage, or does not provide continuity coverage - discrete measure, requires a dynamic test involving both maximum range and close proximity for a short period of time
- Continuity bucket counts missed percentage \delta flag_{BUCKETS}\rightarrow0 - measure with known maximum values during a test window, ideally uses a dynamic test to max range, but could provide a useful measure for shorter ranges especially if introduced suddenly from faraday bags
- Continuity contact event start \delta t_{START}\rightarrow0 and end \delta t_{END}\rightarrow0 times - measure tending to 0 seconds, requires a dynamic test to max range
- Completeness \delta Error_{RT_{1}}\rightarrow0 and \delta Error_{RT_{2}}\rightarrow0 or for shorter tests, just \delta Error_{RT_{CE}}\rightarrow0 - two scaled continuous measures tending to 0, one out from minimum distance analogue to risk threshold, and the second out to double the risk threshold for longer tests. For short tests use expected total measured risk error instead
- Accuracy distance analogue scaled error percentage, mean \overline{Error_{DA}}\rightarrow0 and variance S_{Error_{DA}}^{2}\rightarrow0 - scaled continuous measures tending to 0, both measures required for meaningful analysis
- Accuracy distance analogue scaled error, min Error_{DA_{MIN}}\rightarrow0 and max Error_{DA_{MAX}}\rightarrow0 - scaled continuous measures tending to 0, useful only to provide an indication of why variance is high. We shall not consider this as necessary to design a specific test for
- Accuracy measured time window interval mean \overline{DUR_{WINDOW_{MEASURED}}}\rightarrow0 and variance S_{DUR_{WINDOW_{MEASURED}}}^{2}\rightarrow0 in seconds - time continuous measures whose mean must be less than the epidemiologically useful minimum window period (30 seconds), and whose mean and variance ideally should tend to 0 for maximum accuracy. Mainly useful to compare protocols that achieve high percentage (95%+) bucket counts.
- Longevity - the variation absolute (positive) percentage score of variation over a working day

## Why is it needed?

There was no standardised way to measure proximity detection and continuity of
coverage of a contact event other than simple "Percentage of devices detected
during an entire test" style high level metrics.

Whilst useful, those simplistic metrics miss out a variety of low level Operating System, 
Hardware, and especially protocol behaviours that can lead to a misrepresentation or
misunderstanding of efficacy in epidemiological terms.

Epidemiologists need to calculate exposure risk based on time and distance of any contact
event. The closer people are together, the higher risk. The length of the 'exposure window'
will determine how fine grained such risk scores are. Performance across a range
of devices prevalent in a particular population will determine a protocol's maximum reach.

All of these measures need to be understood in order to truly score and compare the
effectiveness of any Proximity Detection Protocol and therefore the contact tracing 
applications and health service responses built on this underpinning.

## Why is it fair?

We took a step back and considered what physically happens between two mobile devices
during a contact event. We then reasoned what actions could occur to reduce the chances
of a part (window) of a contact event from being correctly recorded. 

We then created dummy data for a simple theoretical contact event. We then applied
an existing risk scoring model to show how a 'perfect protocol' and a protocol that
exhibits some deliberate limitation could modify the observed risk.

This then led us to create a formula that took in to account all limiting factors
that a user of such a contact tracing app could encounter in a particular day.

## Didn't you build this for your own protocol? Aren't you biased?

No. Although it is true that some of the authors of our paper and protocol worked on
previous COVID-19 app efforts for governments, we built this measure independent of any 
particular protocol and before we wrote our
new protocol from scratch. We wanted to be sure we could encourage all teams to
use our measure to communicate their own protocols' effectiveness with their
country's population. The hope is to increase use of and trust in contact tracing
applications worldwide.

Once we completed the design of the formula we created a new protocol based on our 
new knowledge of
what affects efficacy, and used the formula to test and measure its performance,
and directed our efforts at the mechanisms that would provide most epidemiological
advantage. 

Our [protocol](/protocol) has therefore a high score on the measure,
but that's because we've been using the formula and testing approach for longer.
The formula wasn't designed to show our protocol in a good light. We would
encourage all teams to apply our formula to their work and rapidly iterate to improve 
their efficacy. This way more lives can be saved.

## What doesn't it cover?

The paper does mention some items out of scope of our research that others are 
already working on, or where published material already exists. This includes:-

- Converting distance analogue (E.g. RSSI in Bluetooth) to distance (Although we do mention some observations we have on [distance calibration](/bluetooth/distance))
- Anything outside of the mobile phone - We only deal with phone to phone communication in our measure - we do not deal with isolation efficacy or speed to isolation metrics
- Any technology-specific variations - The measure is independent of any technology, although we often explain it in terms of Bluetooth because this is the predominant technology available today
- Manipulation of multiple readings - We leave these discussions to those designing end to end
contact tracing systems. Mechanisms include local triangulation to get a more accurate distance estimation, or performing multiple readings in quick succession to provide more accuracy
- Payload specifics - We use a 129 byte payload in our tests of our own protocol, but our measure does not mention this specifically. The affect of any such length of transmission on efficacy will be caught in the detection and continuity measures and so there was no need to consider payload specifics, only measure how often a particular protocol takes estimation readings for another device

## Follow on work

TODO from paper
