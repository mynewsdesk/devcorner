---
title: A workflow for translations
author: Martin
layout: post
categories:
  - Workflow
tags:
  - git
  - i18n
  - Translations
  - webtranslateit
  - workflow
---
As Mynewsdesk is moving into new markets, our internationalization (i18n) efforts have been stepped up a notch. Rails has pretty good i18n support, but maintenance becomes painful when projects scale up \[insert "rails can't scale" joke\]. A better translation process was a big itch to scratch for us.

After exploring a few online translation tools, we settled on [WebTranslateIt.com][1]. It has a nice GUI and user friendly documentation. Our translation files took a while to import, but the payoff was immediate: graphs and stats on how up to date our translations are.

![Language statistics](/images/wp/2011/04/language-stats.png)

The translation process is straightforward and quite pleasant. You pick the untranslated strings in your language of choice, start typing and hit “Save and Next”. When we developers add strings, we `wti push` our changes on the command line. And when translations are done, we `wti pull`.

### A problem with centralized translations

Having a central online service for all translations presents some challenges, however. Our workflow with feature branches doesn’t map well to WebTranslateIt’s system. Here are a few things our workflow needs to support:

*   Translators should be able to work in parallel on new features. We want to push updated strings from a feature branch to WebTranslateIt before they are ready for a merge with the master branch.
*   The team will work on several features simultaneously. A feature might add, update or remove translations. We want a process that can handle conflicts between new features.
*   Translation strings should not disappear and reappear for our translators. They should have a steadily progressing environment, with changes from several features merged together.

To accomplish this, we have the following workflow:

1.  One team works on feature A. They add, update and remove translation strings `a.added`, `common.updated` and `a.removed`, but only in our master language.
2.  In parallel, work is done on feature B, which adds/updates/removes `b.added`, `common.updated` and `b.removed`. Again, only in the master language.
3.  Feature A is ready for translation, but not deployment. It is merged (fast-forward) into branch wti, and our master translation file is pushed to WebTranslateIt with `wti push`. Translation on feature A can begin.
4.  Later, feature B is ready for translation, but not deployment. It too is merged into branch wti and conflicts (on `common.updated`) are resolved. We then do `wti push` once more. Translations on feature  B can now start.
5.  Feature A is ready for deployment. Feature B is not. Translations are ready. Check out the wti branch and do `wti pull`. Review changes to the locale files in Gitx. Stage all translations relating to feature A and commit (`a.added`, `common.updated` and `a.removed`), then all relating to feature B (`b.added` and `b.removed`) and commit.
6.  Check out branch A, and `git cherry-pick` one of the commits from branch wti.
7.  Proceed with normal deployment of feature A.

It is important to tease apart updated translations into several commits in step 5, because we don’t want to `b.removed` to be missing when we deploy feature A.

There are some pitfalls with the process. If the new translation of `common.updated` is specific to feature B and makes no sense in feature A, merging it on top of the changes from A will be a problem when A is deployed before B. One solution is to use a new, more specific translation key in feature B, and avoid the conflict altogether. Generally, the teams need to talk about merge conflicts in step 4.

### A feature request

WebTranslateIt could make this process easier if they implemented some sort of feature support. It would be great if we could tag strings with the feature name, and pull only translations to strings with a specific tag. Tagging could be done in the code with a commenting convention.

In the example below, the string `blog_post.topics` belongs to the categories branch, and translations to it would be pulled with `wti pull --feature categories` or something similar. Even a full `wti pull` would be easier to separate into several commits, as the tag would be evident when reviewing changes.



This would have the double benefit of letting us easily tell translators to focus on a specific feature we want to deploy soon, and also greatly simplify managing parallel feature translations. WebTranslateIt’s support team has been [very][2] [helpful][3] so far, and so we hope this feature request (no pun intended) will be taken into consideration.

 [1]: http://webtranslateit.com
 [2]: http://help.webtranslateit.com/discussions/questions/44-overriding-the-root-node
 [3]: http://help.webtranslateit.com/discussions/problems/350-translation-key-countriesno-becomes-countriesfalse