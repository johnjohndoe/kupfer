

Guidelines and Policy
=====================

Contributing
------------

You can clone git from its official repository at git.gnome.org, see:

    http://git.gnome.org/browse/kupfer/

You can structure your changes into a series of commits in git. A series
of well disposed changes is easy to review. Write a sufficient commit
log message for each change. Do not fear writing down details about
why the change is implemented as it is, if there are multiple
alternatives. Also, if you forsee any future possibilites or problems,
please describe them in the commit log.

It is not easy to write good commit messgages, because writing is an
art. It is however essensial, and only by trying it, can you improve.

You may publish your changes by sending an email to the mailing list,
<kupfer-list@gnome.org>. You can attach your changes as patches, or you
may also just attach a link to your published git repository.

You can find kupfer's `git repository at github`__ and fork it there,
for easy publication of your changes.

If you suggest your changes for inclusion into Kupfer, make sure you
have read the whole *Guidelines and Policy* chapter of this manual. And
take care to structure your changes, do not fear asking for advice. Good
Luck!

__ http://github.com/engla/kupfer


Icon Guidelines
---------------

Consider the following:

* A Leaf is an object, a metaphor for a physical thing. It can have as
  detailed icon as is possible.

* An Action is a verb, a command that can be carried out. Choose its
  name with care. The icon should be simple, maybe assign the action
  to a category, rather than trying to illustrate the action itself.
  For example, all text display actions use the "bold style" icon, in
  some icon themes simply a bold "A".

.. important::

    Actions should have stylized, simple icons. Leaves and Sources
    should have detailed, specific icons.


Coding style
------------

Kupfer python code is indented with tabs, which is a bit uncommon. (My
editor is set to tabs of size four.) Otherwise, if you want to
contribute to kupfer keep in mind that

* Python code should be clear
* Kupfer is a simple project. Do simple first. [#simple]_

Python's general style guideline is called `PEP 8`_, and you should
programmers should read it. The advice given there is very useful when
coding for Kupfer.

.. _`PEP 8`: http://www.python.org/dev/peps/pep-0008/

.. [#simple] Writing simple code is more important than you think.
             Read your diff (changes) when you are finished writing a
             feature. Can you make it more simple to read? If you can
             make it simpler, often a more effective algorithm comes out
             of it at the same time. All optimizations have a price,
             and unless you measure the difference, abstain from
             optimizations.


Specific Points
---------------

Using ``rank_adjust``
.....................

A declaration like this can modify the ranking of an object::

    class MyAction (Action):
        rank_adjust = -5
        ...

1. Often, this is useless. Don't use it, let Kupfer learn which actions
   are important.

2. If the action is destructive, the adjust should be negative. Never
   positive. For example *Move to Trash* has a negative 10
   ``rank_adjust``.

3. If the action is very general, and applies to almost everything but
   still should never be the default for anything, the adjust should be
   negative.


Using ``super(..)``
...................

Many of kupfer plugin code uses super statements such as::

    super(RecentsSource, self).__init__(_("Recent items"))

We have learnt that it is not so practical. Therefore, when writing new
code, you should however use the following style::

    Source.__init__(self, _("Recent items"))

Why? Because the second version is easier to copy! If you copy the whole
class and rename it, which you often do to create new plugins, the
second version does not need to be updated -- you are probably using the
same superclass.

Text and Encodings
..................

Care must be taken with all input and output text and its encoding!
Internally, kupfer must use ``unicode`` for all internal text.
The module ``kupfer.kupferstring`` has functions for the most important
text conversions.

Two good resources for unicode in Python are to be found here:

| http://farmdev.com/talks/unicode/
| http://www.amk.ca/python/howto/unicode

**Always** find out what encoding you must expect for externally read
text (from files or command output). If you must guess, use the locale
encoding.
Text received from PyGTK is either already unicode or in the UTF-8
encoding, so this text can be passed to ``kupferstring.tounicode``.

Note that the gettext function ``_()`` always returns a unicode string.

.. vim: ft=rst tw=72 et sts=4
.. this document best viewed with rst2html
