---
title: Meet Flower, the Flowdock bot
author: Richard
layout: post
categories:
  - Services
  - Workflow
tags:
  - flowdock
  - flower
---
[Flowdock](http://www.flowdock.com) is a team collaboration and group chat service, similar to Campfire.

![Flower bot](/images/wp/flower-bot.jpg "Flower bot") Flowdock is also the group chat service of choice here at the R&D dept. To make it more fun and/or usable, we made this bot.

[Flower, the Flowdock bot][1] was written in Ruby, and made to be easy to extend with your own commands.

### How do I use it?<span style="color: #444444; line-height: 24px;"> </span>

1.  Sign up for Flowdock if you haven’t already
2.  Create a new Flowdock user account for your Flower bot. Give it a nickname, for example “Bot”, or “Steve”.
3.  Download the [source code from Github][1]
4.  Copy and rename `config.yml.example` to `config.yml`, and fill it with your settings
5.  `rake run`
6.  Mention your Bot in the chat to command it

### Writing my own commands

You should write your own commands to make Flower fun and/or useful for your team.

This is easy. Simply create a class like this in lib/commands that inherits Flower::Command:
{% highlight2 ruby %}
class MyCommand < Flower::Command
  respond_to %w(testing test)

  def self.respond(command, message, sender, flower)
    # Do something with "flower"
    flower.say("Only testing")
  end

  def self.description
    "Text that describes your command. It's used in the built-in Help command."
  end
end
{% endhighlight2 %}

`MyCommand.respond` will be invoked when a message prefix matches what you `respond_to`. Arguments passed are:

*   command – The matched command
*   message – The entire message
*   sender – A hash with sender user id/nick info: `{:id => 123, :nick => "Jonas"}`
*   flower – The Flower instance, that can `say` or `paste` something. Both methods can take the option `{:mention => sender[:id]}` to highlight a message to that user in the chat.

A few commands can be found in our separate command repository at Github: <https://github.com/mynewsdesk/Flower-Commands>

The really fun and useful commands still remains to be written, but Flower provides us with a nice API.

So what do you think? What commands should be implemented next? Is anyone out there using Flowdock?

 [1]: https://github.com/mynewsdesk/Flower