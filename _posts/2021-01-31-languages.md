---
layout: archive
title: "The world's 'best' programming language"
---

I borrowed the title from a debate I attended many years ago at a local meetup group. Four people were on a panel debating over which programming language was 'the best in the world'. I suspect it was tongue-in-cheek but I imagine less-experienced developers (like myself at the time) would have gone 'all-in' intellectually.

The arguments presented were over the syntax, how 'fast' it is, what trendy companies use it, whether it has static-typing etc.

But what makes a programming language truly great is not whether it's compiled, interpreted, dynamically- or statically-typed or even its raw performance.

This is a fact.

We can all write great software in a language in any one of these configurations, conversely we can all write sub-par software with them. x86 assembler is blisteringly fast, but I would have no idea how to write good software with it.

I find these are often points raised by those new to programming - they've learnt in one of these kinds of languages, and the next step in their learning journey is to tout the benefits of it. I've found it also be made by those who have only known a single technology or language their entire career. This isn't inherently bad, but it does lead to voices across the internet making these kinds of claims without nuance. When it comes to software development there is _always_ room for more nuance.

Ergo, whether it's compiled, or interpreted etc is patently the _wrong argument to make_. What we should really be using as the standard for a great programming language or framework is the following metrics which comprise _developer productivity_:

## Community

A programming language is only as strong as its community, how responsive it is to change, how helpful that community can be to solve problems encountered by those entering it for the first time and the level of discussion that occurs within it. An example of a good community is the Python community, a frankly enormous community of data scientists, software engineers, hackers, students etc.

The sheer volume of people within it is what gives it a diverse voice and plethora of information to support those within it. The same goes for the Javascript community, a phalanx of UI and web developers seeking to solve the same kinds of problems.

## Tooling

Without solid tooling it is difficult to navigate the mire that can be a new programming language ecosystem. I've found having powerful, well-made CLI / build tools to be the difference between picking up a language for fun, or switching to it fulltime for development. Recently I switched from using Python for my personal scripting to Go, which has a fast and simple API for creating executables, running tests and installing dependencies. The decision was not based on the language internals, but the tooling surrounding it.

Another example of great tooling is Elixir, with a hugely powerful REPL, easy-to-use testing framework and simple build tool ([mix](https://elixir-lang.org/getting-started/mix-otp/introduction-to-mix.html)) it seems to 'just work'. The documentation is largely understandable, and it behaves as expected. Generating that beautiful documentation as well is unlike anything I've ever seen in a programming language, and it also means every library has the same layout and is easy to navigate.

Likewise, Javascript tooling (while sometimes mercurial in its compatibility) is also extremely useful - from things like [Mock Service Worker](https://mswjs.io/) to [yarn](https://yarnpkg.com/) these are tools which 'just work' while also providing enough output and feedback to react to when things go wrong. This ties in with the community; enough problems have been encountered by people that all manner of issues have been dealt with on their respective Github issue pages.

I will say, an example of _bad_ tooling is Python. The tools exist, (and boy do the tools exist) but they vary in their usefulness and friendly API's. For instance the auto-formatter [black](https://github.com/psf/black) is one of my favourites. It has little configuration and just works. It does one thing and does it well. On the other hand there is the doc generation tool [sphinx](https://www.sphinx-doc.org/en/master/), which requires draconian formatting, has hard-to-understand documentation and there seem to be a dozen ways to do the same thing. Although maybe the sheer level of configuration and customisation is the reason for this.

`pip` as a package manager is _fine_ for the most part but can spout horrible low-level errors which can only be solved by googling the error, or seeing them enough times to know what the issue is. As far as I can tell the only decent way to package Python apps is to bundle them into an entire Docker container.

## Documentation

Without up-to-date, detailed and accurate documentation a language is only as good as you are at tinkering or googling. Maybe this ties into the community metric, but a set of core documentation which is the single source of truth for a language can go a long way to helping novices integrate into the way of thinking within a new language, as well as getting up to speed with little confusion or [yak-shaving](https://imgur.com/gallery/t0XHtgJ).

A great example of good documentation (although I haven't used it in a long time) is (surprisingly) the PHP docs. What makes them so useful are the user-contributed notes after each section in the reference guide. These docs have become the singular place to go (aside from Stack Overflow) for PHP idioms and code examples. [Here's a great example](https://www.php.net/manual/en/control-structures.match.php). Say what you will about the language itself, but there's no doubt these docs have contributed to PHP's initial dominance on the server-side web development front.

I've not had many problems with documentation in a language, but it's one of those things that you don't notice until it doesn't exist. An example for me in the past has been Oracle's PL/SQL (or Oracle's documentation in general) which reads like a mathematics textbook more than a guide for a budding DBA or developer. Perhaps reflective of Oracle's general position within the tech community.

## Final points

The arguments over the fine details of programming languages tend to miss the forest for the trees. The real metric to success is _developer productivity_. If you can be productive in a programming language then it's inherently valuable. At their core, they are a tool to solve a problem and if you can solve that problem in a short amount of time without sacrificing the velocity of future changes and improvements then you're on to a winner.

Anyway, they say the best programming language to write in is the one you are most familiar with, and I guess that's the cop-out of my whole point. The best programming language was all the friends we made along the way.
