---
title: New Rails Plugin to shell out with verbose error handling
author: peter
layout: post
categories:
  - Ruby on Rails
---
I was annoyed with the fact that when you use Ruby backticks (i.e. `some_shell_command`) to shell out to an external program and the path to the program is wrong then no exception is raised. In addition, using system and backticks [doesn’t allow you to capture standard error][1]. I decided to write a little plugin called [shell][2] that allows you to do things like this:

{% highlight2 ruby %}
status, output, error = Shell.run(“/opt/local/bin/curl http://example.com”)
# Raise exception on failure:
status, output, error = Shell.run!(“/opt/local/bin/curl http://example.com”)
{% endhighlight2 %}

It turns out some commands will indicate success with an exit code of 0 but still write to standard error. I have put in a special case in the Shell class that treats “No such file or directory” as a failure eventhough we have a zero exit code in that case.

I’m curious to hear from if there is a simpler and more idiomatic way to do this. It seems odd that I have to write a Rails plugin to invoke external programs in an elegant and convenient way.

 [1]: http://tech.natemurray.com/2007/03/ruby-shell-commands.html
 [2]: http://github.com/mynewsdesk/shell/tree/master