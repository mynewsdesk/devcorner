---
title: Writing commit messages
author: Markus Nordin
layout: post
categories:
  - git
---
Writing correct, nice and to-the-point commit messages is not always easy. Sometimes when we have a lot of changes to commit, it’s easy to write some nonsence just to have it commited and pushed before clocking out at the end of the day. Or sometimes we’re just being a bit lazy or unimaginative.

Like these ones.

*   fix for site fuckup (Jun 10, 2011)
*   added old css (Nov 9, 2011)
*   Fixed weird issue with subdomains (Oct 20, 2011)

Is it easy to understand what the commits are doing?

A good commit message can sometimes save us the time of having to read the code or do a git bisect to understand where a bug might have originated. If done properly over a period of time, the red thread of the features also becomes apparent.

And then there are some that are just weird.

*   Trying to change so that on the publish/edit page the categories are displayed in the locale of the user with the pressroom site (selected\_site) locale used as a fallback. This means that in the finnish pressroom a swedish speaking user gets the category names in swedish and only in finnish if there is no swedish translation. Removed a number of with\_locale(selected\_site) blocks. I also had to change the JavaScript locale in publish from selected\_site to I18n.locale (current locale). The Category#on\_site scope now returns translations both for the site and the current locale. The new Category#name\_with\_fallback(locale) method is used when displaying the category names. It goes to the current locale first and then then falls back on the locale argument (the site locale in publish). TODO: it’s confusing that we now have Category name, name\_with/without\_site and name\_with_fallback methods. Time for a cleanup. (Jun 28, 2010)
*   ooooo, this HURT! (Feb 7, 2011)

I guess there are as many rules and opinions about this as there are developers, but this is something I personally think about when I write my commit message.

*   **Always write about what the code changes, not what you have done** (e.g. never start with “Added …”)
*   **Always write in present**
*   Capitalize the first row, without periods, and** try not to exceed 50 characters** (I’m not a nazi on this)
*   **Don’t be hesitant to add an explanation** as to why the changes have been made (on the third row, leaving the second blank). In fact, if you’re thinking about it, that’s probably a good sign that you need to do it.

If you think I have missed something vital, please comment with your own tips and tricks to write the ultimate commit message!

Here are some of my personal Mynewsdesk Hall of Fame commits:

*   don’t negate, that is a negation (Nov 2, 2011)
*   undefined fucking method (Jan 19, 2012)
*   Pizza! (Dec 15, 2011)

**Update**: As requested by [@patrikstenmark][1], here’s a few examples of good commit messages:

*   Acceptance test for access signup process (May 21, 2012)  
    <span style="font-size: 13px; line-height: 12px;">In this commit, we added a capybara acceptance spec file with two tests. There wasn’t any need to write that we added it, <em>there hardly ever is</em>.</span>
*   Kundo iframe needs transparency attribute in IE (March 29, 2012)  
    <span style="font-size: 13px; line-height: 12px;">We use iframe based <a href="http://kundo.se/">Kundo</a> for support to our customers on top of a div with a background image. We added an attribute to the iframe to allow it to be transparent in IE. Being such a small change, we simply wrote why the change was made.</span>
*   Refactor weekly recipients to dashboard email to own class (May 7, 2012)  
    <span style="font-size: 13px; line-height: 12px;">A refactoring commit that broke out some logic for our weekly summary email into its own module. Small commit message that is to-the-point and no messing about.</span>

Why I think these are good examples is that they adhere to their context. Bigger changes often need a more general summary, smaller changes or bugfixes might want to focus on the why. I don’t think there’s a silver bullet to writing them, just some guide lines and a lot of different contexts you should be aware of and adjust to. Anyway, that’s only what I think.

**Resource links** (thanks [@henrik][2] and [@kalasjocke][3] for pointing out the lack of it):

[A note about Git commit messages][4] by [tpope][5].

[Commit message guidelines for Git itself][6], follow them or [Linus will come and kill you with a hammer][7].

 

 [1]: http://twitter.com/patrikstenmark
 [2]: http://twitter.com/henrik
 [3]: http://twitter.com/kalasjocke
 [4]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
 [5]: http://tpo.pe/
 [6]: http://git.kernel.org/?p=git/git.git;a=blob;f=Documentation/SubmittingPatches;h=ece3c77482b3ff006b973f1ed90b708e26556862;hb=HEAD
 [7]: https://github.com/torvalds/linux/pull/17#issuecomment-5659970