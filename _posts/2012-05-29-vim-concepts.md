---
title: Vim Concepts
author: Jan Andersson
layout: post
categories:
  - Editor
tags:
  - editor
  - vim
---
**Introduction**

At Mynewsdesk we develop Ruby on Rails applications and the developers may use any text editor they like. This has led to a myriad of different editors in use, from Textmate and Sublime Text to Emacs and Vim. Personally I prefer the Vim editor and have been a user since 15+ years.

There are many ways to approach and learn Vim but surprisingly few tutorials have a closer look at the main concepts of Vim. I think understanding these takes away some of the confusion when using the editor.

**Modes**

Vim has six basic modes; normal, visual, select, insert, command-line and ex-mode. These are basically different states of the editor and a basic cause of confusion for many users.

When starting up the editor it’s in normal mode, which means that most unmodified keys execute a command, rather than inputting text as in most other editors. This makes sense, since most of the time in an editor you don’t really enter text, rather you navigating, moving, changing and searching text.

To enter text you have to enter the insert mode, to select text you enter visual mode etc. There are also six more additional modes which are variations of the basic modes. Read more about the modes by typing *:help vim-modes.*

**Registers**

There are a bunch of registers, which could be compared to temporary variables in a programming language. All the registers has a one character long name and to use them you prefix them with a double quotation mark, such as “- or “x.

Some of these registers are updated by Vim itself, such as “/ that contain the last search and other are user updatable, such as “a to “z. You may also use these by prefixing a command in normal mode with a register, such as “/p for outputting last search or “ayy for copying current line into the a-register.

Read more about the registers by typing *:help registers.*

**Buffers and Windows**

A buffer is a file loaded into the memory of the editor. This would not be of big use without a way to actually view and edit it, and that is done in a window. There is not a 1:1 relationship between them though, a buffer may have one or more windows attached to it (called an active buffer) or no window at all (a hidden buffer), and a window may be with or without a buffer.

When started Vim shows one window but they may be split vertically or horizontally into more windows. You may then move around between them or move around the windows, resize them, open different buffers in them etc. There are ways to run commands in all open buffers, to hide them, remove them from the buffer list, unload them from memory and more.

The buffers may be listed with the command *:buffers* and you may read more about them and windows by typing *:help buffers.*

**Tabpages**

A quite recent addition to Vim is tabpages, added in Vim 7. A tabpage is a collection of one or more windows. This is a nice way to open a buffer, without changing the current window setup. It may for example be used when temporarily opening up another project with another set of files.

It may cause some confusion that the window splits lives in the the tab, rather than each split having its own set of tabs as in some other editors, but used as mentioned above it’s one of the strengths of Vim tabpages.

Read more about the modes by typing *:help tabpage.*

**Other concepts**

This has touched some of the basic concepts that are most commonly used. There are also a bunch of other really useful ones, such as folds, quickfixes, undo branches, text objects and tags. These and much more is described in the Vim help, please go ahead and navigate it with the *:help* command.