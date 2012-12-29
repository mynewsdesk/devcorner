---
title: The pair programmers prompt
author: Billskog
layout: post
categories:
  - git
---
At Mynewsdesk we love pair programming and do it all the time. To avoid manually changing our git configurations we use tools like [hitch][1] and [git-pair][2]. But I have found that we are forgetful and sometimes leave our pair configuration untouched when we stop pairing. To stop us from committing bugs in other people’s names we thought it was a good idea to display the pair info in the prompt along with the current git branch. 
![Prompt](/images/wp/2012/09/Screen-Shot-2012-09-13-at-4.17.27-PM.png "Prompt")
Just add this snippet to your `.bash_profile`.

{% highlight2 bash %}
function branch {
  ref=$(git symbolic-ref HEAD 2> /dev/null)
  if [ $? -eq 0 ]; then
    echo " "${ref#refs/heads/}""
  fi
}

function hitch_pair {
  source ~/.hitch_export_authors
  if [ `git symbolic-ref HEAD 2> /dev/null` ] \
    && [ -n "${GIT_AUTHOR_EMAIL:+1}" ]; then
    echo $GIT_AUTHOR_EMAIL|awk -F "[+@]" '{ print " "$2"+"$3 }'
  fi
}


YELLOW="\[\033[0;33m\]"
GREEN="\[\033[0;32m\]"
CYAN="\[\033[0;36m\]"
NO_COLOUR="\[\033[0m\]"

export PS1="$YELLOW\w$GREEN \$(branch)$CYAN\$(hitch_pair)$NO_COLOUR$ "
{% endhighlight2 %}

 [1]: https://github.com/therubymug/hitch
 [2]: https://github.com/mynewsdesk/git-pair