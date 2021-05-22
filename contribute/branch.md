---
layout: docs
title: Creating a Branch
description: Creating a feature branch
menubar: docs_menu
---

# Creating a feature branch

All Herald projects use the GitFlow mechanism for managing feature work.
This enables multiple developers to be working on both short term and long term
features with minimal cross interference.

For an intro to how we use feature branches, please visit the
[Release management section of our public Miro board](https://miro.com/app/board/o9J_kidPGWQ=/?moveToWidget=3074457358810022477&cot=14).

To creat a feature branch, find a feature you wish to work on in the SAME repo.
There are multiple issue trackers, one per repo - be sure to read the correct one.

Each GitHub issue has an issue number. E.g. 169. You will need to create a feature
branch off of `develop` called `feature-ISSUENUM` - so in our example, `feature-169`.

Be sure to create this feature branch off of develop, not from the main branch.
Pull requests to the wrong branch will be rejected.

Once you have created this locally, push it to your fork of the Herald repository
you are working on. This ensures you have access and connectivity to upload
your new branch work.

## Optional but helpful

If you are working on an item, please add a comment to that issue number. That
ensures others don't accidentally pick up the same task. 

In addition, if you see an 'assignee'
to an issue, please don't work on it without that persons knowledge as they may
well already be working on it. (E.g. if they are a core committer).

It's also very helpful if you mention `Part of #ISSUENUM.` in your end of day 
commit. This lets people know this item is still being actively worked on, 
and push it to your fork's feature branch so others can see it.
So in our example, I'd perhaps say `Part of #169. Authentication working, Authorisation next`.

Continue in this guide by visiting: [Style guidelines]({{"/contribute/style" | relative_url }}).