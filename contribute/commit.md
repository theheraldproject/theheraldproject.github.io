---
layout: docs
title: Commits
description: Adding a new commit
menubar: docs_menu
---

# Adding a new commit

There are a few rules and best practices to adhere to on commits.

## Optional: Add yourself as a contributor

By issuing a PR to an open issue you have the right to include your
information in the AUTHORS.md file at the root of the repository.
Please do that as if your Pull Request (PR) is accepted, you have done
enough to include yourself in that file!

## Developer Certificate of Origin

The Herald Project, inline with all other Linux Foundation projects, requires
that a DCO be agree to by every developer. In order to attest to this, 
EVERY SINGLE COMMIT must end with a signing line.

We don't mean a PGP signature, we mean a line like the below:-

`Signed-off-by: Joe Bloggs <joe@bloggs.com>`

NOTE: The EXACT spelling and space between the name and the `<` character.

If this is not present for every one of your commits, then the DCO bot will
fail when you issue a Pull Request, and your changes cannot be merged. There
are no exception.

The wording of the DCO can be read here:-
[https://developercertificate.org/](https://developercertificate.org/)

It is recommended to commit after each individual piece of a feature is completed.
E.g. you may write a failing test, make it pass, and then commit that.

## Keep commits small and atomic

Features should be individual atomic smallest-feature items of work rather than
large changes.

As an example, the recent addition of the Analysis API involved serveral commits.
Sample classes, Sample List, and tests. Then higher level iterator classes, then
analysis API primitives, and then the final algorithm API with algorithm samples.

This large a feature being implemented is a rarity though, and only generally for
a new component API that is not useable until the whole API is working.

## Makes commit titles short, messages detailed

A brief title is good. Less than 120 characters ideally. E.g. `Part of #169. Sample API passing tests.` is sufficient for a title.

A description would start with the same content, but perhaps add 2-4 bullet points
highlighting key implemented features, changes, or known issues with this commit.

As always, the commit message should end with the DCO Signature line, as above.

Continue in this guide by visiting: [Pull Requests]({{"/contribute/pullrequest" | relative_url }}).