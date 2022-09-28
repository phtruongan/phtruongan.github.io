---
layout: post
title: "Mouse-free Copy + Paste on Linux (Ubuntu)"
description: "Fix copy paste on Ubuntu"
comments: true
author: Junjue Wang
keywords: "Copy/Paste, Linux, Ubuntu, Tmux"
---
# Mouse-free Copy + Paste on Linux (Ubuntu)


Recently, a friend who is new to Ubuntu, asks me "Why cannot I copy from
terminal to GUI application in Linux"? A quick google search finds many similar
questions on StackOverflow UbuntuForum. However, none of the solutions given is
satisfying. Therefore, I'd like to share my solution in this post.


Intuitively, copy/paste function should consist of simple
functionalities of selecting a text region, copying the region, and pasting the
region. It should work among any applications a user is using, no matter if the
application has a GUI, e.g. a browser, a text terminal, or an editor running in
terminal mode.


Sadly, this is not the case on linux. The fundamental reason is that GUI
applications are treated differently from non GUI-based applications. Linux GUI
applications use X11 library. These applications act as X11 clients connecting
to a window display environment (X11 server). The copy/paste behaviors are
supported by X11 among GUI applications. Nonetheless, for applications that do not
use X11, e.g. emacs -nw (terminal mode), it is not able to use the copy/paste
function supported by X11.


If you use Emacs's built-in M-w to copy your selection, following table
demonstrates in which combination the following "paste" would succeed.


| Copy To | Emacs         | Emacs -nw (Terminal Mode** | Browser |
| ------------- |:-------------:| -------------------------:|---------:|
| Emacs         | Y             |                          |         |
| Emacs -nw     | N             | Y                        |  |
| Browser       | Y             | N                         | Y |
{: .pure-table .pure-table-bordered}


So how do we fix this? What is a general solution to copy/paste between GUI
applications and terminals?


The solution is *Make sure we are only copying among GUI applications.*. What??
You might think this is silly and not a solution at all. But hear me out. Unless
you are dealing with a console or in a command line tty, you are probably
running inside a desktop environment. The terminal application in Ubuntu
defaults to gnome-terminal, which is a GUI application. In it, mouse can be used
to select the region, Ctrl+Shift+c and Ctrl+Shift+v can be used to copy and
paste to/from gnome-terminal. 


However, this solution involves using the mouse. And reaching out to a mouse is
much slower than using the keyboard. To set a mark without mouse in a terminal,
**Tmux** is here to help.
[Tmux](http://manpages.ubuntu.com/manpages/trusty/man1/tmux.1.html) is a
terminal multiplexing tool most famous for its capability to survive abrupt ssh
connection drops. It has a rich set of features in managing terminals. To our
concern, it has a copy mode, in which one can use to select text inside a
terminal with keyboard. Together with
[tmux-yank](https://github.com/tmux-plugins/tmux-yank) plugin, it is able to use
X11 clipboard to copy/paste to/from other X11 applications. See my ansible role
[here](https://github.com/junjuew/ansible-dotfiles/blob/master/roles/bash/tasks/tmux.yml)
and [tmux configuration
file](https://github.com/junjuew/ansible-dotfiles/blob/master/roles/bash/files/tmux.conf)
to customize tmux and install tmux-yank to enable mouse-free copy/paste between terminal
and X11 apps. With the customization, to do copy/paste, use:
    * Enter copy mode: Ctrl-b Enter
    * Set Mark: v
    * Copy: y
    * Exit copy mode: Escape
    * Paste: Ctrl-b ]
