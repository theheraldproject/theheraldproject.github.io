---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Efficacy Test Methods
description: The methods we recommend as standard for our formula
toc: true
toc_title: Efficacy test methods
menubar: efficacy_menu
---

# Formal testing methods

This section details the formal tests we used in order to validate the formula in
[The Paper]({{"/efficacy/paper" | relative_url }})

In the end we determined you only needed to run the two required tests in order
to get all the data you need, with a single long run of the office test the best
to provide replicable data.

Only test results gained by using this method shall be considered for publication on
this project website. We welcome results from any protocol teams using these tests.

## Set up

The set up procedures are mentioned for both th [iOS test app GitHub site](https://github.com/vmware/herald-for-ios) and the [Android test app GitHub site](https://github.com/vmware/herald-for-android) for the Herald protocol.

## Required tests

Below are the required tests. These provide the correct input for the Fair Efficacy Formula.

### Office test (required)

Replicates an office environment with people sat nearby each other.

Set phones up in two to four rows of up to 5 devices per row at a fixed 2m separation, in a grid. Run the test for 30 minutes. Ideally introduce each phone 'fully backgrounded' for Bluetooth in order to prevent bias due to newly interrupted phones (i.e. replicate the 'worst case scenario' rather than 'sunny day' phone activity option). Phones must not be artificially awoken or played with during this test as this could skew the sensor readings.

Proves continuity coverage, continuity, and completeness with multiple measurements per test.

Run this test for 8 hours in order to gain a relevant longevity measurement.

### Formal accuracy test (required)

In order to fairly test the accuracy of a distance analogue two phones shall have to have their distance analogues measured at a known distance. This is the 'expected distance analogue' value. Do this for distances of 3m, 2m, 1.2m, 0.8m, 1m, 1.5m, 2.5m, 3.5m for as long a time as necessary to determine the true expected distance analogue value. Then in a test of the actual application and protocol place the phone at each distance for exactly 1 minute, then move to the next distance, and so on during the same test. Log data from both sides of the communication.

Now you can use a script to compare the expected distance analogue value (E.g. -51 RSSI at 2m) against the individual distance analogue values within each 1 minute period. It is expected each protocol have at least 2 readings in each period, thus providing readings that can be analysed. The reason for the 1 minute period is to try to replicate a real scenario of approaching someone, talking, then moving away again just like as colleagues working in an office might.

This test shall be used only for distance accuracy (mean error) calculations.

### Continuity window test (required if source code not available of detection routine)

A set of pairwise tests shall be carried out in order to establish the actual in-practice window detection size of a given protocol. This is mainly required if the protocol and its test app do not provide the necessary metrics in formal testing as a matter of course.

This test comprises of two phones with the app running in a radiation shielded environment being introduced for one or more multiples of 30 seconds, and counting how many one way detections occur. Comparing the most common handsets across windows of 30 seconds up to that protocol's declared estimation interval will reveal its actual minimum mean detection time, and the detection time required to guarantee 100% detection.

It should be noted that testing application only in scenarios with more than 5 minute intervals is not realistic to modern life or the 30 second required minimum epidemiological window for risk estimation. Tests should therefore be of irregular numbers of minutes for contact events, at irregular distances in order to best expose limitations of particular protocols' implementations.


## Optional tests

Whilst not necessary to gain the input variables for our formula, the below tests may be of
use to protocol development teams, and so we mention them here.

### Star test (optional)

Have a central phone surrounded by other phones at a fixed distance away. Proves continuity coverage, accuracy and completeness with multiple measurements per test. Better than previous office test for accuracy measurement as all phones are equi-distant from the measurement phone (the centre of the star is the only measuring phone)

### Parade test (optional)

Two phones (one iPhone and one Android phone) are placed 1m apart from each other. Other phones individual are walked toward these phones, held for 30 seconds at the distance in question (must at least measure 0.8m - i.e. within the 1m Oxford model risk score distance), before being walked away again.

This test is useful in order to prove 30 second continuity measures, and to provide completeness at a variety of ranges. By varying the minimum range this can also provide a good indication of accuracy across all distance ranges.

Measures Continuity coverage and CE start and end times, also allows bucket counts and completeness, should be done with a phone pair with continuity measures compared to the star test to determine any negative effects of multiple devices being present in the star test. This is especially useful for Bluetooth based protocols where older phones are limited in the number of devices detected and interacted with.

### Common but older or cheaper phones (optional)

As we have found in our testing many older phones, or newer but cheaper Android phones, do not support Bluetooth Advertising.
These are commonly held by those who cannot afford more expensive models that do have this support. There is a danger, therefore,
that concentrating on just the latest or most popular phones could lead to the most disadvantaged people in society from
not being covered by a protocol. As these people generally live in high density housing this poses a problem for
controlling disease spread.
