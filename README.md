# Repo Doc Writer

A Claude skill that writes the two documents every repository ends up needing, `README.md`
and `description.md`, as separate files, in UK English, in the voice of whoever built the
thing.

That's the whole job. It writes. It doesn't audit, score, or run repair loops over
documentation you already have.
---
Author: TABARC-Code
---
## Why this exists separately

There's already a bigger sibling, `github-project-writer`, which does the full treatment:
evidence hierarchies, an eight-category rubric, a bounded Kaizen loop, adversarial checks,
five output modes. All useful when you're going back over documentation that's drifted
away from the code.

Most of the time you don't want any of that. You've just finished something, the repo is
sitting there with no README, and you want two files written properly the first time.
Loading a review engine to draft from scratch is like booking a structural survey before
you've poured the foundations.

So this one drafts, and only drafts.

## What you get

Two markdown files, written on disk, kept deliberately apart.

`README.md` is operational. What the thing is, what it needs, how you install it, how you
configure it, what a successful first run looks like, where the security boundaries sit,
what breaks and why. The question it answers is "can I use this".

`description.md` is the reasoning. Why the project is shaped this way, what the trade-offs
were, which alternatives got rejected, what state it's honestly in, and what someone should
understand before they change anything. The question it answers is "why is it like this".

They get drafted together rather than in sequence, because material surfaces in the wrong
one constantly. A design decision you uncover while explaining an awkward setup step
belongs in the description. A constraint you find while writing the architecture section
often turns into a setup line back in the README.

## Status

Version 1.0.0. Implemented and usable.

One file, no scripts, no dependencies. Nothing here executes. If you were hoping for
something more impressive, I can only apologise.

## Installation

Drop the folder into your skills directory.

On Claude.ai, upload the packaged `.skill` file and hit Save on the file card. In Claude
Code, put it wherever your skills live and restart the session so it gets picked up.

There's no build step and nothing to configure.

## Using it

Ask in ordinary language. It triggers on README requests, repo descriptions, setup guides,
"idiot's guide" sections, project write-ups, and the vaguer end of things like "document
this" or "what do I say about this repo".

Give it the source if you can. Manifests, entry points, config files, routes, tests, the
Dockerfile. It reads for how things are used rather than what they're called, so a function
named `handleEverything` won't become a documented feature unless something actually calls
it.

Without source it'll still write you both files, but expect more hedging. That hedging is
deliberate and you shouldn't strip it out unless you know the claim holds.

## What it won't do

It won't invent commands, install steps, environment variables, benchmarks, contributors,
release history, changelog entries, supported platforms or passing tests. If a section
would have to be fabricated to exist, the section gets deleted instead.

It won't audit your existing docs. That's `github-project-writer`, and the two are meant to
sit side by side.

It won't quietly document the version of the project you meant to build. If the code and
the intent disagree, the disagreement gets written down.

## House rules it applies

UK English, Oxford commas, contractions where they fit. Em dashes used sparingly and
unspaced. No arrow glyphs or decorative icons, `>` and `<` where a flow needs showing.
Numbers spelled out to nine, numerals from 10.

Roughly 2% dry cynicism, which is a direction rather than a measurement.

Prose can be informal. Fragments, sentences starting with And, the odd loose construction.
Commands and paths cannot. Nothing inside a code fence is ever roughened up for texture,
for reasons that should be obvious to anyone who has copied a broken command out of a
README at two in the morning.
===
## Known limitations

Accuracy is capped by whatever you hand it. Point it at a project with no source and stale
docs and you'll get a lot of qualified language, which is correct behaviour but not very
satisfying.

The cynicism budget isn't measurable and drifts upward when the subject is funny. Section
10 catches it sometimes.

Number style was set to "spell out to nine" as a sensible default. It wasn't specified, so
change it in section 3 if you'd rather it went to ninety-nine.

There are no evals. The description was written to trigger broadly on the assumption that
skills tend to under-trigger, but that hasn't been measured.

## Licence

Not set. Pick one before publishing the repository.
