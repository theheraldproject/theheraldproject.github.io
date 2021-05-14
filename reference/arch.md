---
layout: docs
title: Architecture Documentation
description: Herald architecture and design information
menubar: reference_menu
---

# Architecture documentation

Herald's high level design is documentated in diagram form
in our [Archi HTML export]({{"/documents/architecture/index.html" | relative_url }}). The 
[raw archi file](https://github.com/theheraldproject/theheraldproject.github.io/blob/develop/documents/herald-architecture.archimate) 
can be downloaded from GitHub.

The best diagram to start with is the 
[Masters views index]({{"/documents/architecture/index.html" | relative_url }}).
Note that all diagrams can be clicked to open up new diagrams and entity information.

## Class documentation

We deliberately don't use Archimate to provide UML style class diagrams. We use agile development
and so keeping diagrams up to date with live code would be a foolhardy exercise.

Instead we provide Doxygen documentation for the 
[C++ reference implementation API]({{"/api/cpp/namespaces.html" | relative_url }}).
Each class here shows its inheritance diagram.

## Something missing?

If you think some documentation is missing, please head on over to 
[#herald-general on the LFPH Slack](https://slack.lfph.io/) or open a new
[Feature request on GitHub](https://github.com/theheraldproject/theheraldproject.github.io/issues/new).