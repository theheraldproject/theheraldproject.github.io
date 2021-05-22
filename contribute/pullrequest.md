---
layout: docs
title: Pull requests
description: Issuing a Pull Request for a Herald repo
menubar: docs_menu
---

# Issuing a Pull Request for a Herald repo

Below are things to consider before submitting a PR, and how to submit one.

## Things we do not accept PRs for

These include, but are not limited to:-

- Minor spelling/grammer fixes
- Line formatting / minor style fixes
- Trivial removal of compile warnings for some platforms
- Anything which is not a feature improvement / change / bug fix
- Any item which is a matter of personal belief or preference

## Pull Request from your feature branch to our develop branch

Don't issue a PR from your develop branch, but instead from your feature branch. E.g. from `origin/feature-169` to `theheraldproject/develop`.

DCO commit checks will automatically run, as will unit tests. Please wait 2 minutes
after submitting the PR until green ticks appear, and fix any issues there first.

If you issue a PR but someone else has merged changes for the same classes to develop
since you started work, you will be asked to merge the current develop branch into
your feature branch. This is a simpler task rather than requiring our core committers
to take time away from feature work to merge your changes to the develop branch out of line.

You have now finished the contributor guide. Click to return to the [Docs list]({{"/docs" | relative_url }}).