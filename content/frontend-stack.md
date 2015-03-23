Title: The Juntoo Frontend Stack
Date: 2015-03-23
Category: Frontend
Author: Jacob Oscarson

# The Juntoo Frontend Stack

# TL;DR

Critical parts of the Juntoo frontend are:

  * [Node.js][1] - for developing critical algorithms without the baggage
    of a full browser
  * [Mocha][2] - as far as I know, the most sane Javascript test suite runner
  * [Browserify][3] - normalizing a lot of the API's btw Node.js & browser
    makes it easier to get value from using Node.
  * [React.js][4] - the most advanced UI toolkit for the browser today
  * [JSDOM][5] - makes it sane to simulate UI code in Node.js

[1]: https://nodejs.org/
[2]: http://mochajs.org/
[3]: http://browserify.org/
[4]: http://facebook.github.io/react/
[5]: https://github.com/tmpvar/jsdom

# Discussion

The front-end part of the Juntoo stack is the most complicated.
Therefore, that's where I consider I need to invest the most in
tooling convenience. From a much more functional perspective, that
means 5 critical requirements:

1. I must have access to a step-debugger to study execution path or
   where data structures go wrong when I'm adding features.

2. It must be reasonably convenient to do complete end-to-end
   functional experiments when a new feature is in it's most early
   phase of development.

3. The API must have some kind of familiar, standardized "feel".

4. The API must be able to express the concept of 'modules'.

5. Iteration speed (time from change to test/experiment of changed
   code) must be minimal.

My attempt to fulfill these requirements looks like this now:

## REPL/Debugger

So far, the most full-featured debugger enviroment have been Google
Chrome's Developer Tools. This worked very well up until resently
(late autumn 2014), but downsides have started to creep up:

* After the OS X Yosemite "update" I'm experiencing performance
  problems.
* Issues that I can only speculate relates to
[this](https://code.google.com/p/chromium/issues/detail?id=169468) are
more and more starting to show what looks worringly like incorrect
stack traces and worse: manually set breakpoints have a tendency to
break at another line when where I set them.

I'm starting to have serious doubts about Developer Tools / V8's
ability to accurately express the stack to the debugger. Grr. For pure
unit tests, I have a method of falling back on print debugging via
Node.js, but hey, it's 2015!

## Modules

In the seemingly eternal wait for ES6 modules, the most established
ecosystem with a way of handling modules is the Node.js one, via their
`require()` call. Therefore, I early decided that the program should
be able to execute most of the front-end logic in Node.js if given
sufficient simulation of browser-specific API's and data
structures. This helps performance when studying extremely small parts
of the system in isolation and provides a certain amount of
familiarity for other (hypothetical for now) developers whith
experience of Node.js. An additional advantage is that the program can
use the [events](https://nodejs.org/api/events.html) API, which is
stable and well known by now.

The initial implementation of this was done via
[Browserify](http://browserify.org/). So far, it has served me well,
but lately I've started to fall behind on updates, and that blocks
quite a few
