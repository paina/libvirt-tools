libvert-tools by paina / libvert Hooks Dispatcher
=================================================

A litte explanation in English is in the script itself.
If motivated, I'll update this README.
However improovement of the script is more important... Sorry.

日本語でちょっと (A bit in Jpanese)
-----------------------------------
へたれな英語を晒すのも勇気がいりますが、まぁ一応、英語で大分かきました。

それはそうとして、これは何かと言いますと、libvert にはデーモン起動とか、
ゲスト起動とかしたときにスクリプトをフックする機能があるのですが、
標準入力から XML 流し込まれたりで大変だったので、簡単ではありますが Ruby
スクリプトを書きました。

忘れたころにまたアップデートすると思います。ご質問はなんなりと下記連絡先へ。

What is this
------------

There is a hook function in libvert.
[Official explanation is available in libvert site.](http://www.libvirt.org/hooks.html
"Hooks for specific system management")

Motivation of which I use this function is to enable NDP proxy on briding interface
when guest boots.
I tried `pre-up` function in `/etc/network/interfaces`, however if there are many guests,
name of the interface may not be fixed.

Therefore I decided to use the hook in libvert. It is a little difficult to operate it
because libvert pour long long XML into script which hook cathes.
I have to find interface name from the XML.

This tiny script makes it a little easier. Condition to do some actions can be described
as object composition of Ruby. Inputed XML is parsed by REXML, and find required dara by
XPath. (Though condition description by Ruby object is a litte complex. I may make function
which converts LTSV ([Labeled Tab-separated Values](http://ltsv.org)).) Actions can be
programmed as Ruby methods.

Anyhow this script can 'dispatch' hooks from libvert and do something. Have fan!

How to use
----------

Place this script as $SYSCONFDIR/libvirt/hooks/{daemon,qemu,lxc,network} 
and set executable. Symbolic link is good practice.

This script is hooked by libvert with 4 arguments and configuration
information as XML from stdin.

This script is executed by like this. e.g. preparing QEMU guest starting:

    path/to/libvirt/hooks/qemu guest_name prepare begin -

1st argument is 'object', 2nd is 'operation', 3rd is 'sub-operation'
and 4th is 'extra argument'. If there is none, '-' is set.

This script dispatch 'action' according to the arguments, and execute
the action described as a Ruby method.

When executing action, XML from stdin is usable as REXML document.

License and Contact
-------------------

Copyright © SATO Taisuke <<paina@paina.jp>> 2014

Licensed under The BSD 2-Clause License. See `LICENSE` file.

Questions, proposals, pull requests(!?) etc. are welcomed.