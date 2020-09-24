---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Fair efficacy formula - formal test results
description: How to fairly measure efficacy, and results we have generated.
menubar: efficacy_menu
---

# Testing Results

Below is a list of the Protocols we have tested formally, or who have independently tested
using our Fair Efficacy Measure.

Note: The theoretical max efficacy in the UK given our phone mix, and perfect radio conditions, is 76.39%.

The Ferretti et al paper [[6]](../paper/bibliography#a-6) shows a 'same day' efficacy requirement of 50%,
with an error range between 30 and 60%. This assumes a 90% effective lockdown of ill people. 
Thus in our paper we call:-

- 60%+ 'Very effective'
- 50%+ 'Effective'
- 30%+ 'limited effectiveness'
- Less than 30% 'not effective'

|Protocol|Latest results|Test reports|
|---|---|---|
|<b>Herald Test App</b><br>(Herald Protocol)<br>(Tolerates 1 of 2 phones without advertising)|<b>Efficacy: 63.46%<br>(Very effective)</b><br>Pspec^2: 81.51%<br>Detection: 100%<br>Continuity: 93.06%<br>Completeness: 98.36%<br>Accuracy: 95.95%|Dev team tests: [2020-08-31](../results/herald-2020-08-31)<br>Independent tests: None|
|<b>Theoretical UK Max</b><br>(UK phone mix)|<b>Max Efficacy: 76.39%<br>(Very effective)</b><br>Pspec^2: 84.88%<br>Detection: 100%<br>Continuity: 100%<br>Completeness: 100%<br>Accuracy: 100%|Dev team tests: N/A (Simulation)<br>Independent tests: N/A (Simulation)<br>Simulation: [2020-08-31 Simulations (ODS)](../documents/protocols-paper-synthetic-data.ods)|
|<b>5-minute annealed, exact RSSI</b><br>(Theoretical, simulated)<br>(Requires BLe Advertising)|<b>Efficacy: 7.64%<br>(Not effective)</b><br>Pspec^2: 42.32%<br>Detection: 100%<br>Continuity: 20.93%<br>Completeness: 95.87%<br>Accuracy: 100%|Dev team tests: N/A (Simulation)<br>Independent tests: N/A (Simulation)<br>Simulation: [2020-08-31 Simulations (ODS)](../documents/protocols-paper-synthetic-data.ods)|
|<b>5-minute annealed, bucketed RSSI</b><br>(Theoretical, simulated)<br>(Requires BLe Advertising)|<b>Efficacy: 5.99%<br>(Not effective)</b><br>Pspec^2: 42.32%<br>Detection: 100%<br>Continuity: 20.93%<br>Completeness: 75.18%<br>Accuracy: 100%|Dev team tests: N/A (Simulation)<br>Independent tests: N/A (Simulation)<br>Simulation: [2020-08-31 Simulations (ODS)](../documents/protocols-paper-synthetic-data.ods)|
|<b>2-minute exact, exact RSSI</b><br>(Theoretical, simulated)<br>(Requires BLe Advertising)|<b>Efficacy: 9.53%<br>(Not effective)</b><br>Pspec^2: 42.32%<br>Detection: 100%<br>Continuity: 25.58%<br>Completeness: 97.81%<br>Accuracy: 100%|Dev team tests: N/A (Simulation)<br>Independent tests: N/A (Simulation)<br>Simulation: [2020-08-31 Simulations (ODS)](../documents/protocols-paper-synthetic-data.ods)|
|<b>Your Protocol here!</b>|<b>Efficacy: ?</b><br>Detection: ?<br>Continuity: ?<br>Completeness: ?<br>Accuracy: ?|If you have used our [Fair Efficacy Formula](../efficacy/paper) and <br>[Formal Tests](../efficacy/method) then we will happily host your <br>top level results here, no matter whose protocol you use|