---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: docs
title: Fair efficacy formula - formal test results
description: How to fairly measure efficacy, and results we have generated.
menubar: reference_menu
---

# Testing Results

Below is a list of the Protocols we have tested formally, or who have independently tested
using our Fair Efficacy Measure.

## Traceable interaction fraction

The OpenABM simulation published by Oxford uses a 'traceable interaction fraction' as its efficacy input.
This is based purely on the efficacy of the technical control method and does not factor in population effects.
This also gives an easy to understand percentage score, with 100% being 'perfect control'.
As this is the same scale no matter where in the world a protocol is deployed, we're now quoting this as our
headline number. The fair efficacy paper shows how to use Fair Efficacy Testing results to determine the traceable
interaction fraction number.

## Fair Efficacy 'efficacy' score

This statistic includes population effects such as the prevalance of particular phone models and phone uptake within a country.

Note: The theoretical max efficacy in the UK given our phone mix, and perfect radio conditions, is 46.57%.

The Ferretti et al paper [[6]]({{"/efficacy/bibliography#a-6" | relative_url }}) shows a 'same day' efficacy requirement of 30%,
with an error range between 10 and 57%. This assumes a 90% effective lockdown of ill people.
Thus in our paper we call:-

- 57%+ 'Very effective'
- 30%+ 'Effective'
- 10%+ 'Limited effectiveness'
- Less than 10% 'not effective'

See the [Fair Efficacy Paper page]({{"/efficacy/paper" | relative_url }}) for details on how these values are defined and derived in testing.

## The latest results

|Protocol|Latest results|Test reports|
|---|---|---|
|<b>Herald Test App</b><br>(Herald Protocol)<br>(Tolerates 1 of 2 phones without advertising)|Traceable Interaction Fraction: <b>80.10%</b><br>Efficacy: 41.37%<br><b>(Effective)</b><br>Pspec^2: 48.71%<br>Detection: 100%<br>Continuity: 99.991%<br>Completeness: 98.36%<br>Accuracy: 95.95%|Herald team tests: <br />v1.1 Release: [2020-11-28]({{"/results/herald-2020-11-28" | relative_url }})<br />v1.2.0-beta3 in development: [2021-01-31]({{"/results/herald-2021-01-31" | relative_url }})<br>See the bottom of this page for historical Herald results. |
|<b>Alberta Trace Together (ABTT)</b><br>(Herald Protocol)<br>(Tolerates 1 of 2 phones without advertising)|Traceable Interaction Fraction: <b>83.77%</b><br>Efficacy: 47.00%<br><b>(Effective)</b><br>Pspec^2: 66.80%<br>Detection: 100%<br>Continuity: 93.19%<br>Completeness: 98.36%<br>Accuracy: 95.95%|Herald team tests: <br />v2.0 Release: [2021-03-22]({{"/results/abtt-2021-03-22" | relative_url }}) <br>This uses the final Herald v1.2.0 release.<br> Max theoretical efficacy in Canada is 55.20%|
|<b>Theoretical UK Max</b><br>(UK phone mix)|Traceable interaction fraction: <b>100%</b><br>Max Efficacy: 46.57%<br><br>(Effective)</b><br>Pspec^2: 51.75%<br>Detection: 100%<br>Continuity: 100%<br>Completeness: 100%<br>Accuracy: 100%|Dev team tests: N/A (Simulation)<br>Independent tests: N/A (Simulation)<br>Simulation: [2020-08-31 Simulations (ODS)]({{"/documents/protocols-paper-synthetic-data.ods" | relative_url }}) |
|<b>5-minute annealed, exact RSSI</b><br>(Theoretical, simulated)<br>(Requires BLe Advertising)|<b>Efficacy: 4.46%<br>(Not effective)</b><br>Pspec^2: 24.67%<br>Detection: 100%<br>Continuity: 20.93%<br>Completeness: 95.87%<br>Accuracy: 100%|Dev team tests: N/A (Simulation)<br>Independent tests: N/A (Simulation)<br>Simulation: [2020-08-31 Simulations (ODS)]({{"/documents/protocols-paper-synthetic-data.ods" | relative_url }}) |
|<b>5-minute annealed, bucketed RSSI</b><br>(Theoretical, simulated)<br>(Requires BLe Advertising)|<b>Efficacy: 3.49%<br>(Not effective)</b><br>Pspec^2: 24.67%<br>Detection: 100%<br>Continuity: 20.93%<br>Completeness: 75.18%<br>Accuracy: 100%|Dev team tests: N/A (Simulation)<br>Independent tests: N/A (Simulation)<br>Simulation: [2020-08-31 Simulations (ODS)]({{"/documents/protocols-paper-synthetic-data.ods" | relative_url }}) |
|<b>2-minute exact, exact RSSI</b><br>(Theoretical, simulated)<br>(Requires BLe Advertising)|<b>Efficacy: 5.56%<br>(Not effective)</b><br>Pspec^2: 24.67%<br>Detection: 100%<br>Continuity: 25.58%<br>Completeness: 97.81%<br>Accuracy: 100%|Dev team tests: N/A (Simulation)<br>Independent tests: N/A (Simulation)<br>Simulation: [2020-08-31 Simulations (ODS)]({{"/documents/protocols-paper-synthetic-data.ods" | relative_url }}) |
|<b>Your Protocol here!</b>|<b>Efficacy: ?</b><br>Detection: ?<br>Continuity: ?<br>Completeness: ?<br>Accuracy: ?|If you have used our [Fair Efficacy Formula]({{"/efficacy/paper" | relative_url }}) for details|

## Herald historical results

You can read all past Herald results on the [Herald Historical Results]({{"/results/herald-historical" | relative_url }}) page.
