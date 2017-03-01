.. title: other languages
.. slug: other-languages
.. date: 2017-03-01 10:55:53 UTC+05:30
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text

.. contents::
   
HTML / CSS
----------

HTML header
~~~~~~~~~~~

.. code:: html

    <html lang="en">

-  Here ``lang`` is an 'attribute', with value ``en``.
-  ``html`` is the tag.
-  ``head`` has metadata, one of which is ``title`` (to fill the tab
   bar).
-  ``html`` has tags for 'article', 'header', and 'figure' now!

CSS
~~~

-  Use classes to segregate your content. Call it with a leading '.' in
   css. e.g.

.. code:: css
          
          .site-nav-header { width: 300 px }

-  ID's on the other hand can only be used once per html page. use with
   leading '#' in css. e.g.

.. code:: css

          #main-title { color: green }

FORMS
~~~~~

.. code:: html

    <label for="nickname">Please enter your nickname</label>
    <input type="text" id="nickname" name="nickname">

-  The label's ``for`` should match the input's ``id``
-  The ``name`` is what is passed to the backend as a variable name
-  ``input type`` can be a lot of things, like 'email' or 'submit'

-  All of these: labels, inputs etc are inline-block elements and are
   therefore stacked horizontally.
-  To align them better, use div's, which are container tags that break
   up these horizontal elements into vertical stacks.

There are 3 groups of elements in the way the browser stacks them:

-  inline: span, em, strong (all treated horizontally)
-  block level: p, div, article (browser inserts CRLF)
-  inline block level : input, textarea (can be resized)

tcl: xml parsing example
------------------------

.. code:: tcl

    package require tdom
    set dom [dom parse $XML]     
    set recording [$dom documentElement]
    set datamode [$recording firstChild]
    set session [$datamode nextSibling]
    $session attributes *
    $session getAttribute session_id
    set participant [$session nextSibling]
    set dom [dom parse $XML]     
    set recording [$dom documentElement]

