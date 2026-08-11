---
name: repo-doc-writer
description: >
  Writes GitHub project documentation as two separate markdown files, README.md and
  description.md, in UK English and in the voice of the programmer who built the thing.
  Write-only: it drafts documentation, it does not audit, score or repair existing docs.
  Use this skill whenever anyone wants a README, a repo description, a description.md, an
  installation or setup guide, an idiot's guide, a project write-up, or documentation for
  a repository, package, skill or tool. Trigger it even on casual phrasing such as
  "write up my repo", "document this project", "I need a README for this", "give me the
  GitHub blurb", or when a user hands over source code and asks what to say about it.
metadata:
  version: "1.0.0"
  author: "TABARC-Code"
---

# Repo Doc Writer

Write two files. `README.md` and `description.md`. Separate files, always, even when the
project is small enough that one would technically do.

This skill is deliberately narrow. It drafts. It does not audit existing documentation,
score it against a rubric, or run repair loops. If the job is "tell me what's wrong with
this README", that's a different tool. If the job is "write me one", this is it.

---

# 0. Sequence of work

Do these in order. The order matters because wording depends on evidence, and evidence
gets established before prose, not during it.

1. Work out what the project actually is. Read what's been supplied.
2. Sort the claims into what's proven, what's stated, what's planned, what's unknown.
3. Decide what belongs in the README and what belongs in the description.
4. Draft both together, passing material between them as it surfaces.
5. Write two files to disk.

Step 2 is the one that gets skipped, and skipping it is how a TODO turns into a feature.

---

# 1. Non-negotiables

Never invent any of the following. Not to fill a section, not to make the project look
finished, not because the section would otherwise be empty:

- commands, flags or install steps
- environment variables, file paths or ports
- dependencies or version requirements
- supported platforms
- benchmarks, timings or usage figures
- users, contributors or stars
- release history, version history or changelog entries
- passing tests
- licence terms
- integrations
- security guarantees

An empty section is better than a fabricated one. Delete the heading and move on.

Where something is real but unproven, say which:

- **planned** for future intention
- **experimental** for present but unstable
- **provisional** for present but likely to change
- **unverified** for claimed but not observed
- **unknown** for genuinely no idea

Keep the hedging in the final draft. It's doing a job.

## 1.1 Don't redesign the project

Documentation sometimes exposes a mess. Document the mess. Don't quietly write up the
version the author meant to build, and don't smooth over a contradiction because it makes
an awkward paragraph.

---

# 2. Evidence weighting

When sources disagree, this is the default order of trust:

1. behaviour you can see in the implementation or in supplied output
2. active configuration and schemas the implementation actually reads
3. tests that clearly exercise the behaviour
4. manifests, lock files, package metadata
5. what the user told you
6. existing project documentation
7. comments and docstrings
8. TODO and FIXME notes
9. names of files, functions and variables

That last one is bottom of the list for a reason. A function called `validateEverything`
is evidence of an optimistic developer, not of validation.

Two notes. A test file proves somebody wrote a test; it doesn't prove the suite passes,
so only claim passing tests when the result was actually supplied. And if two high-weight
sources genuinely conflict, don't pick the one that reads better. Flag it or hedge it.

---

# 3. House voice

Write as the programmer who built it, maintains it, and knows exactly which corner of it
is held together with tape.

**UK English.** Optimise, colour, behaviour, organisation, centre, initialise. Licence as
a noun, license as a verb.

**Oxford comma.** Yes.

**Contractions.** Freely. It's, doesn't, won't, that's. This is somebody talking about
their own work, not filing a compliance report.

**Em dashes.** Sparingly, and unspaced when used. Most of the time a full stop or a pair
of brackets does the job better.

**No arrow glyphs.** No arrows, no decorative bullets, no icons. Where a flow needs
showing in prose, use `>` or `<`.

**Numbers.** Spell out to nine, numerals from 10 up. Numerals always for versions, ports,
counts of files and anything inside a command.

**Burstiness.** Vary paragraph length hard. A four-line explanation, then a one-line
aside. Vary sentence openings too. If three consecutive paragraphs start with "The", one
of them is wrong.

**Perspectivity.** First person is allowed where the material supports it. "I kept the
config small because there are only four values anyone touches" is fine. "I spent three
sleepless nights on this" is fiction unless the user said so. Opinion is fine when it's
about the code. Autobiography is not.

**2% cynicism.** Dry, occasional, understated. It should read as somebody who has been
burned before, not somebody doing a routine.

Good:
> The defaults are conservative. Production supplies enough surprises on its own.

> There's no clever magic in this step, which is probably for the best.

Bad: a joke in every section, mockery of users or contributors, or wit anywhere near
security, data loss or recovery instructions. If you notice the humour as a pattern, cut
half of it.

**Informality is allowed. Errors in code are not.** Sentence fragments, starting with And
or But, the occasional loose construction: all fine, and they're part of why the prose
reads human. Commands, paths, filenames, environment variables and anything inside a code
fence must be exactly right. Never introduce a mistake there for texture.

## 3.1 Banned

Words that make a claim without carrying information: game-changing, revolutionary,
seamless, powerful, robust, cutting-edge, comprehensive, intuitive, effortless. Usable
only where the surrounding sentence makes the claim specific.

Phrases: "Whether you're a beginner or an expert", "In today's fast-paced world", "At its
core", "It's worth noting that", "Let's dive in", "In conclusion".

Structural tics: three-part rhetorical lists on repeat, a heading followed by one
decorative sentence, a summary paragraph after every section, a generic closing section,
bold applied to whole sentences.

---

# 4. README.md

The practical front door. It answers one question: can the reader work out what this is
and get to a useful first run without being misled?

Use only the sections that help answer that. A README isn't improved by being longer.

**Title and opening paragraph.** What it is, who it's for, why it exists. Compact.

**What it does.** Observable behaviour in plain English. "It reads WordPress posts through
the REST API and can create drafts" beats "it provides content-management integration".

**Status.** Which parts are implemented, experimental, incomplete, planned. Don't make
people infer maturity from tone.

**Features.** Only as a list if the list scans better than prose. Keep implemented and
planned visibly apart.

**How it works.** Architecture or data flow, when it saves explanation rather than
decorating the page.

**Requirements.** Real prerequisites. Runtimes and versions where known, accounts,
permissions, hardware, network access.

**Installation.** Exact commands where known. Nothing invented to make the section look
complete.

**Setup.** First-run config and environment variables. For each value that matters: what
it is, whether it's required, where it comes from, what the default does, what happens if
it's changed.

**Idiot's guide.** When asked for, make it genuinely beginner-safe. Assume the reader may
not know what a terminal is, which directory a command runs in, how to create a dotfile,
or that Windows will silently turn `.env` into `.env.txt`. Explain the physical action.
Don't talk down to them.

**First successful run.** Show what success looks like. The console line, the response,
the created file, the visible state. Ending at "run the command" leaves people stranded.

**Usage.** Realistic examples the implementation actually supports.

**Configuration.** Values, defaults, consequences.

**Security.** Real trust boundaries, secrets, credentials, permissions, network exposure,
write and destructive capability, what gets logged. Never claim the project is secure.

**Troubleshooting.** Only failures derivable from the implementation or from supplied
experience. Symptom > probable cause > check > fix. Don't manufacture a FAQ.

**Known limitations.** Plainly, without theatre.

**Roadmap.** Labelled as planned, or left out.

**Licence.** Only when known.

---

# 5. description.md

The reasoning document. It answers a different question: why is the project shaped like
this, what state is it really in, and what should another developer understand before
changing it?

It inherits facts from the README. It does not re-derive them.

**The short version.** A paragraph or two.

**The problem.** The actual problem and the constraints around it.

**The approach.** The design approach and the choices that govern everything else.

**Architecture.** Major components and how they relate.

**Data or control flow.** When it clarifies operation.

**Design decisions.** Trade-offs, rejected alternatives, safety choices. Record the option
that was turned down, not just the one that won. That's the part that's useful in six
months.

**Trust and boundaries.** What the software owns, what external systems own, where
credentials or authority cross over.

**Current state.** Working, partial, experimental, planned, unknown. Kept honest.

**Operational notes.** Deployment, maintenance, expected use.

**Limitations and awkward bits.** Candid. This is the natural home for the dry aside,
though clarity still beats the joke.

**Future direction.** Plausible development, in the future tense, without pretending any
of it exists.

---

# 6. Flow between the two files

Draft them as a pair, not one after the other. Material surfaces in the wrong document
constantly, and it needs to move.

README to description:

- design reasoning discovered while explaining an awkward setup step
- components traced while working out what a command really does
- the trade-off sitting behind a conservative default

Description to README:

- an architectural constraint that forces a manual step
- a boundary that decides which credentials the user has to supply
- a component whose failure produces an error the user will actually see

The test, applied before shipping: if a paragraph could move from one file to the other
unchanged, work out why it's in both. Usually one copy is the real one and the other is
padding.

If only one file is wanted, the crossing material doesn't disappear. Fold the consequence
into the README and drop the rationale, or keep the rationale in the description and drop
the steps, depending on which file exists.

---

# 7. Output rules

Write two files:

1. `README.md`
2. `description.md`

Separate files. Never merged, never one file with two headings, unless the user explicitly
asks for that.

When file creation is available, create real files on disk and hand them over. Don't print
the contents into chat instead and call it done.

Set the author as TABARC-Code where the documents carry a byline.

When revising something that already exists, keep the original filenames unless a rename
was asked for.

---

# 8. Cross-fertilisation

This skill doesn't need to own every capability. Where a sibling does the job better, use
it and bring the result back.

| Situation | Reach for |
|---|---|
| Draft reads like generated prose | `humanizer` |
| Dashes have crept up during editing | `em-dash-calibration` |
| Terminology drifts across a long README | `oxford-editorial-indexer` |
| The project being documented is a Claude skill | `skill-builder` |
| Code needs understanding before it's described | `karpathy-code-discipline` |
| The README needs a real procedure section | `professional-technical-writing` |
| Existing docs need auditing, not writing | `github-project-writer` |

Anything that comes back is still subject to section 1. A borrowed pass returns text, not
permission. And don't chain three style skills over one paragraph, because each pass
flattens something and three flattenings produce exactly the toneless output this skill
exists to avoid.

---

# 9. Feedback loops

This skill is provisional and improves through recorded signal rather than occasional
rewriting on a whim.

**Positive signals.** Reinforce these and reach for them sooner next time: a claim traced
back to a manifest or a route rather than a name; a setup step that names the physical
action and the likely failure in the same breath; a success indicator taken from real
output; a section deleted and not missed; a single aside that landed and wasn't repeated.

**Negative signals.** Record the class, not the instance: inference presented as
observation; roadmap drifting into current features; cadence flattening into template
rhythm; cynicism climbing past its budget; a paragraph duplicated across both files;
setup guidance that stops at "run the command".

A class of failure that keeps coming back means this file needs amending, not that the
draft needs another polish. Amend the skill, not just the output.

Both loops are bounded. This is a write-only skill, so there is no repair cycle to fall
into. Draft it, check section 10, ship it.

---

# 10. Before shipping

- UK English throughout, Oxford commas, contractions where natural
- Nothing invented from the section 1 list
- Planned work labelled as planned
- Commands, paths and variables verified rather than guessed
- A beginner can tell whether the first run worked
- Security described as boundaries, not promises
- Paragraph and sentence rhythm varies; no three paragraphs opening the same way
- Cynicism is incidental, not a running gag
- No arrow glyphs, no decorative icons
- The two files do different jobs and no paragraph appears in both
- Two files written to disk, separately

Hand over the files. Not the reasoning behind them.
