# Project Description

Author: TABARC-Code
---
## The short version

A write-only Claude skill that produces `README.md` and `description.md` for a repository,
as two separate files, in a maintainer's voice. It is the drafting half of a pair. The
auditing half is `github-project-writer`, and the split between them is the main design
decision here.
---
## The problem

Two problems, actually, and they pull in opposite directions.

The first is the familiar one. Generated project documentation fails predictably: invented
commands that look plausible, a TODO promoted to a feature, a test file read as proof that
tests pass, and a vocabulary of robust-seamless-comprehensive that carries no information
at all. Underneath every one of those is the drafter treating its own fluency as evidence.

The second is subtler. The tool built to prevent all that, `github-project-writer`, grew to
905 lines and then, after being split, to a core file plus three references carrying a
rubric, eight Kaizen stages, adversarial checks and five output modes. Every bit of it
earns its place when you're repairing documentation that has drifted. None of it earns its
place when there's no documentation yet and you just want two files.

Loading a review engine to write a first draft is not a neutral cost. It shapes the output.
A skill built around scoring and repair produces prose that has been scored and repaired,
which tends to read like it.

## The approach

Cut the review machinery out entirely and keep the parts that actually govern a first
draft: what may never be invented, how to weigh conflicting evidence, the house voice, the
two document outlines, and the traffic between them.

What's left is a linear sequence. Read the material, classify the claims, split them
between the two documents, draft both together, write the files. There is no loop back.
The nearest thing to a review pass is a flat checklist in section 10, and it's a checklist
rather than a rubric on purpose. A checklist you either pass or fix. A rubric invites
another pass, and another, and the marginal value of pass four is close to nothing.

Evidence weighting survived because it's the mechanism that stops fabrication, and it costs
about 20 lines. Everything downstream of it, the scoring and the classification labels and
the internal statuses, got compressed into five hedge words that appear in the output
directly.

## Architecture

One file. `SKILL.md`, around 330 lines, no references directory, no scripts, no assets.

That's a departure from the sibling skill, which was deliberately split so its
per-document specifications would only load when needed. Splitting made sense there
because the file was 905 lines and half of it was irrelevant to any given job. Here the
whole file is relevant to every job, so a split would mean an extra read for no gain.

The internal order is the working order. Sequence, then non-negotiables, then evidence,
then voice, then the two document outlines, then the flow between them, then output rules,
then the checklist. Reading top to bottom is the same as doing the work in order.

## Design decisions

**Write-only, rather than a mode flag on the existing skill.** A `--no-audit` switch on
`github-project-writer` would have been less duplication. It would also have meant loading
the rubric and the Kaizen stages in order to be told not to use them, and the presence of
scoring criteria in context changes how a draft comes out even when the score is never
computed. Two skills, one job each.

**A checklist instead of a rubric.** Section 10 is binary. Rubrics with 0 to 3 bands are
useful when the point is measuring improvement across iterations. This skill has no
iterations, so a band structure would just be decoration with numbers on.

**Informality allowed in prose, forbidden in code.** The house style asks for human
texture, including the occasional loose construction. Applied indiscriminately that's a
disaster, because a README's value is largely in the exactness of its commands. So the
line runs at the code fence: prose can be rough, anything executable cannot. This is
stated explicitly in section 3 rather than left to judgement, because "write like a human"
plus "write commands correctly" is not obviously a single instruction.

**Cynicism set to 2%.** Down from the 4% used elsewhere in the ecosystem. Documentation is
read by people who are stuck, and someone who is stuck has a much lower tolerance for wit
than someone reading an essay.

**Two files, always.** Even for a project small enough that one would cover it. The moment
merging becomes an option for small projects, the definition of small starts moving, and
the separation between "how do I use it" and "why is it like this" is the most useful thing
the skill enforces.

**Numbers spelled out to nine.** This wasn't specified and the default was chosen rather
than derived. Flagged here so it can be changed rather than inherited by accident.

## Trust and boundaries

Nothing here executes. No filesystem access of its own, no network calls, no credentials.
It's instructions, and everything it does happens through whatever tools the calling
session already has.

The boundary that matters is evidential. The skill can only see what it's handed. It can't
run a test, execute a command or confirm that a documented endpoint answers. Section 2's
rule about only claiming passing tests when the result was supplied is enforced by
convention, because there is no mechanism available to enforce it any other way.

Anything it writes about security in a target project describes boundaries. It doesn't
audit them, and it should never be read as having done so.

## Current state

Working: the full drafting sequence, both document outlines, the bidirectional handoff,
the voice rules, the output rules, the ship checklist.

Untested at scale: everything. This is version 1.0.0 and it hasn't been run across a
spread of real repositories yet. The design is sound on paper, which is worth roughly what
that phrase usually is.

Unknown: triggering behaviour. The description was deliberately written broad and slightly
pushy, on the general principle that skills under-trigger rather than over-trigger. Whether
it collides with `github-project-writer` in practice is exactly the sort of thing that
needs an eval set and doesn't have one.

## Operational notes

No dependencies, no install step beyond placing the folder, plain markdown to edit.

If you change the section numbering, check sections 6, 8 and 9, which refer to section 1
and section 10 by number.

The most likely thing you'll want to change is the voice block in section 3. It's written
as a list of separate rules rather than a paragraph precisely so individual lines can be
swapped out without unpicking the rest.

## Limitations and awkward bits

There's real overlap with `github-project-writer`. Both carry a version of the same never-
invent list, the same evidence hierarchy and the same document outlines. Keeping them in
sync is manual, and it won't happen, so expect them to drift. The alternative was a shared
reference file that both load, which would have coupled two skills that are meant to be
independently editable.

Section 3 tells the drafter to allow informal constructions. That instruction is inherently
fuzzy and it's the one most likely to produce inconsistent results between runs. It stayed
because the alternative, a strictly correct register, is the exact flat institutional voice
the skill exists to avoid. Fuzzy and right beats crisp and wrong.

The feedback loop in section 9 is written down but not persisted. Signals noticed during
one session don't survive to the next. Closing the loop properly means editing this skill
by hand, which section 9 tells you to do and which nobody does at half eleven on a Friday.
---
## Future direction

Planned, none of it built:

- an eval set covering trigger overlap with `github-project-writer`, since the boundary
  between "write me docs" and "fix my docs" is fuzzier in real phrasing than in design
- a worked example pair in `assets/`, one README and one description for a small real
  project, showing the separation holding rather than describing it
- a short variant outline for projects too small to justify the full section list, which
  would need to be a documented outline rather than an invitation to merge the files
