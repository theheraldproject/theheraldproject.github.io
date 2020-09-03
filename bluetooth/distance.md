---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Bluetooth Distance Estimation
description: Distance Estimation research
toc: true
toc_title: Distance Estimation
menubar: bluetooth_menu
---

# Introduction

DISTANCE


## Prior practical observations

During the development of the protocol we noticed some trends and approaches that worked well in testing to make the RSSI value calculated more accurate. We ended up rejecting these in favour of a raw RSSI value recorded at a fixed point in time purely because a separate scientific research effort being led by The Alan Turing Institute and the University of Oxford was assessing how RSSI could be processed. These efforts did not result in the development team being asked to take multiple measurements or perform any other 'at time of recording' data processing operations.

Nevertheless we did discover two interesting aspects of RSSI recording and distribution we shall now detail for background information important to the design of a measure to assess accuracy.

### Running mean of RSSI values in Bluetooth

During early development the iBeacon API was assessed. This was eventually rejected as it did not allow a mechanism for easily exchanging cryptographic material necessary to ensure privacy and security of a proximity detection protocol, or provide a continuous variable for distance estimation. Whilst attempting to replicate the style of functionality of the iBeacon API in Core Bluetooth on iOS we discovered that a more accurate RSSI value (and thus distance estimation) could be achieved by taking a simple running mean of the last 5 RSSI readings taken. 

Whilst we did not have time to formally assess this improvement it appeared for short ranges to improve accuracy from 30cm to 10cm within a meter. It was also observed in an evaluation application showing both the iBeacon API and our RSSI mean model to respond quicker to changes in distance between devices, and settle down to an accurate value quicker than the iBeacon API itself. We presumed at the time that the iBeacon API must be doing something similar, but with perhaps more readings over a longer period of time.

Whilst crude, this is one way that abhorrent errors in individual RSSI readings could be alleviated in an algorithm to convert distance estimation analogues to distances. This calculation is out of scope of the paper.

### Modal value of a set of regular RSSI readings in Bluetooth

The author has produced a calibration application and calculated a basic algorithm for distance estimation as a reference model. The results discovered in this brief independent calibration work are of interest in to how bluetooth generally performs for distance estimation and so we publish the summary of them here in order to help inform debate. Other work has since been done by the Fraunhofer institute and the GSMA mobile phone manufacturers consortium to create a more accurate estimation algorithm and to provide calibration data.

I ran two calibration applications running on iOS to rapidly perform RSSI readings between the phones at a known measured distance. The application's user interface allowed the tester to specify the distance, providing the ability to take tens of thousands of readings per distance in a single test cycle before uploading the data for analysis.

Below shows the output of that analysis. The R scripts used to produce this and the application itself is available on request under the MIT license.

This application can be used to determine the 'correct' RSSI reading for two known phones at a particular distance, and thus useful for formal evaluation of a proximity detection protocol's accuracy, and so we discuss the results here.

![RSSI distribution over thousands of readings at different distances](/images/distance-rssi-distribution.png)

The above images show over ten thousands readings per contact event over approximately 4-5 minutes at each distance. The iPhones were kept in the foreground allowing a very high rate of RSSI readings to be taken. This would not be the case in a real application but it was useful to be able to discover RSSI distribution over time at each distance.

The orange dashed line shows the modal RSSI value rather than the mean. The grey lines are standard deviations (based on means of course), but shown either side of the mode rather than mean.

The thick black lines show the cut off of 2 standard deviations from the mode. Some outlier RSSI values are recorded here, but our analysis excluded these from the final mode calculation in order to prevent skew in the data set. (rare occasional abhorrent values over 20 from the actual RSSI value do occur with Bluetooth).

The distributions are not quite normal. It is likely this is due to how radio waves propagate and where the receiver is exactly positioned based on the multiple of wavelengths distance the receiver is from the transmitter.

What is clear though is that the modal value provides a strong signal when compared to mean or median, and is unaffected, given enough readings, by any skew due to not being an exact multiple of wavelengths away from the transmitter.

We further analysed these modal values to determine if they could be used to discover an algorithm for distance estimation that would act as a stop gap prior to further research being done on distance estimation algorithms. Whilst not related to the measures in this paper it is useful to complete the description of our experiences with Bluetooth calibration data. This formula was used in one place in this paper - to convert the 'distance' in the mock data to a target RSSI value, and thus to help estimate any error due to distance analogue bounding in a given protocol and its effect on overall efficacy score.

The result of using these modal values was a logarithmic function that provided the following distribution. Note that the values for RSSI have been processed as a log before plotting. The function is not linear.

![RSSI distance data regression](/images/distance-rssi-regression.png)

This provided the following naive formula which we will call Fowler's formula for want of a better name, named after the author, naturally.

![Fowler's RSSI distance regression formula](/images/distance-rssi-formula.png)

where D is the estimated distance in meters and RSSI-MODE is the modal RSSI value.

As per the chart the R value for the formula's fit was close to -1. This is no surprise as we are basing our regression on the mode rather than mean. The very small p value, however, indicates a good fit to the recorded test values.

It should be noted that this function is only valid for these two iPhones used in the test. There was no standardisation or calibration for particular phone models done. The Fraunhofer Institute's paper TODO CITATION uses a similar log estimation function but based on the mean RSSI value observed whilst generating pairwise phone calibration values rather than the mode.

We recommend an analysis is done in future study to see how many RSSI values would need to be read in quick succession to provide a reliable modal RSSI value close to a point in time in order to see if its use provides a better estimation function. It is possible, however, that research on probabilistic estimation functions has surpassed the accuracy of both the Fraunhofer and Fowler algorithms.

It has been reported that Swiss scientists have developed a probabalistic distance estimation
function which is perhaps more accurate. TODO CITATION

### The effect of movement on any RSSI processing approach

Any detection mechanism that uses multiple RSSI values within a time bucket will potentially provide a good fit if the phones are stationary for every RSSI reading in the set. If instead, however, the phones are on the move on different trajectories then this range of values may increase the error in the distance estimation.

We recommend further research to determine the effect on accuracy of different estimation functions in both static and moving conditions.