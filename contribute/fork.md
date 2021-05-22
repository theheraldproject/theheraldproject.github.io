---
layout: docs
title: Forking a repo
description: Forking a Herald repo
menubar: docs_menu
---

# Forking a Herald repo

If you wish to work on an item you must first fork one of our repos, and then
create a new feature branch.

1. On GitHub, visit the repo you wish to work on
1. On the 'Code' or home page of the repo, click on 'Fork' in the top right
1. Fork to your own personal account

This gives you a mirror that is linked to the main Herald repo of the same name.

Any commit you push to your own repo before issuing a pull request that mentions
a feature number (issue number) will be liking from the main issue tracker
on the Herald GitHub Issues list for that repo. This lets others know you are
actively working on an item.

It's important you keep you own develop branch (and any in-progress work) synced
with the Herald develop branch. To do this fast-forward (not merge) your 
develop branch before you create a new feature branch.

Continue in this guide by visiting: [Branching]({{"/contribute/branch" | relative_url }}).