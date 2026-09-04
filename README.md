# AI spec kit

A toolkit for building and shipping software with AI agents. It is a set of
files you copy into your project, a filled in example of each one, and the rules
that make an agent use them.

Everything here came out of building two mobile apps this way, with fast
iterations and an agent doing most of the work. An agent does most of the work
in this repository too, under occasional human supervision, so expect some AI
slop.

## Set it up

Copy five things into your project, in this order. It takes about an hour, and
most of that hour is writing your own answers rather than copying.

1. **Copy [the approach](./01-approach.md) into your project**, e.g. as
   `docs/approach.md`. It holds how an agent should manage the work, and it says
   nothing about how to build software, because an agent already knows that.
   Copy it rather than linking to this repository, so that your project does not
   depend on the kit sitting beside it.
2. **Copy [examples/CLAUDE.example.md](./examples/CLAUDE.example.md) to
   `CLAUDE.md` or `AGENTS.md` at your root**, and replace the invented project's
   answers with your own. Your agent loads this file without being told, and it
   is the only file that does, so everything else in the kit reaches your agent
   through it.
3. **Write `SPECS.md`**, using [examples/SPECS.md](./examples/SPECS.md). Your
   project in four lines, every feature on one line each, and the feature being
   built now written out in full.
4. **Create `TO-BE-CONTINUED.md`**, using
   [examples/TO-BE-CONTINUED.md](./examples/TO-BE-CONTINUED.md). Start it empty
   and let the work fill it.
5. **Add the rest when you feel the lack of one**, and not before.
   [Documents worth having](./02-documents.md) describes all ten and says what
   belongs in each.

## What changes once it is there

An agent working in a project set up this way does four things it does not do
otherwise.

- **It opens a session by saying where it stands.** Which part of the project it
  is about to touch, how much initiative it has there, how well it understands
  that part, and which documents govern it. You get one cheap place to correct
  it, before the work rather than after.
- **It writes the feature down and gets agreement before writing code.** The
  spec says what somebody can do when the feature is finished and how anybody
  will know it works, so finishing stops meaning that the code compiles.
- **It records what it left half built, in the same commit.** A paywall with
  real prices that charges nobody looks finished in a screenshot and in the
  code, and the unfinished list is the only place that difference is written
  down.
- **It closes by saying what it verified, what it assumed, and what is waiting
  on you.** Agents answer that accurately when asked and rarely volunteer it.

## What is in it

Two documents apply to any project.

| File | What it is for |
| --- | --- |
| [01-approach.md](./01-approach.md) | How an agent manages the work, in a page. Copy it into your project. |
| [02-documents.md](./02-documents.md) | The ten files a project keeps, and what belongs in each. |

Three are specific, and each says at the top what it is specific to. Skip
whichever does not describe what you are building.

| File | Specific to |
| --- | --- |
| [03-mobile-launch.md](./03-mobile-launch.md) | Shipping to the App Store and Google Play. |
| [04-graphics-and-visuals.md](./04-graphics-and-visuals.md) | Anything with a designed surface, and not having a designer. |
| [lessons.md](./lessons.md) | Findings from real projects, grouped by subject. Nobody reads it from the top. |

[examples](./examples/) holds a filled in version of every document, all
describing the same invented project, so that you can copy a shape rather than
work from a description.

The repository also carries a writing style at
`.claude/skills/plain-writing/SKILL.md`, which an agent working here loads on
its own. It is one house style, so swap in your own if you have one.

## The idea underneath all of it

An agent is fast, tireless, and confident whether or not it is right. You cannot
tell a correct answer from an invented one by reading it, because an agent
writes both the same way, so the proof has to come from somewhere else.

Every file here follows from that. Write things down so that the agent cannot
forget them. Make it prove its claims rather than state them. Keep the checks
cheap enough to run on every change. None of it is about better prompting.

## The rules the kit runs on

**Parsimony.** A rule earns its keep by changing what an agent does. Most of the
rules here started as something that went wrong once, and anything that has
never changed an outcome gets cut. A short list gets followed and a long one
gets skimmed.

**Speed of iteration.** Short rules kept next to the work beat a specification
written before the work started. The point of working this way is how quickly
you can try something, see it fail, and change it, and heavy process takes that
away.

**Sparse but significant quality checks.** Put tests, code reading and human
sign off where being wrong is permanent or expensive, e.g. data with no second
copy, money, and advice about health or safety. Skip them where a mistake is
obvious and cheap to fix. Checking everything to the same depth costs more than
it saves, and it teaches everybody to ignore the checks.

**Rules meant to be bent.** Each rule says when it applies, so that an agent can
tell when it does not apply. An agent that skips a rule should say so in the
chat and say why. A rule that gets bent often is either wrong or badly written,
so fix it or remove it.

**A check beats an instruction.** Where a rule can be a command that fails,
write the command. Twenty em dashes reached these documents in one week while
the style file banning them sat in this repository, pointed at from the
approach.

## How to commit here

For an agent adding something to this repository.

- **Branch from the latest main, and fetch first.** Another agent may have
  landed something while you were reading. A change written against an old copy
  of a document deletes whatever arrived in between, and the deletion is easy to
  miss in a large diff.
- **Check whether the rule is already here.** Extend the rule that exists rather
  than adding a second one beside it that says nearly the same thing. Eight
  rules in the approach once said the same thing, because each session that hit
  the fault wrote a new rule instead of adding its evidence to the old one.
- **Ask whether it belongs here at all.** A finding about one app belongs in
  that app, or in [lessons](./lessons.md) if the next project would hit it too.
  The approach is only for rules about managing the work.
- **Write it in the format the documents use.** A heading that gives an
  instruction, a body saying when the rule applies and what to do, a line
  starting with Why, and one italic line of evidence. Leave the evidence line
  out when no real incident sits behind the rule, rather than inventing one.
- **Say when the rule can be skipped.** A rule that fires every time gets read
  as background and stops changing what anybody does.
- **Follow the writing style** in `.claude/skills/plain-writing/SKILL.md`, in
  the commit message as much as in the document.
- **Write the commit message about the change, rather than about the work.** One
  subject line in sentence case with no full stop, then plain sentences saying
  what problem the change fixes. Do not list the files, because the diff lists
  them already.
- **One rule per commit**, so that somebody can revert a rule on its own.
- **Open a pull request rather than pushing to main.** Say in the description
  what you verified and what you assumed.

## How the kit is kept

It is a working toolkit rather than a finished one. It grows from real projects,
at the moment somebody finds something missing. When a piece of work turns out
to need something that is not written here, it goes in here as well as getting
done. Tests and analytics both arrived that way, and the next project would need
both too.

It also shrinks, and shrinking is the half that gets skipped. The approach
reached 66 rules over 1579 lines before somebody cut it back to a page, and most
of what went was one app's engineering findings written as though they were
general rules.

Keep it mostly general. An entry belongs here when it would be true of the next
app somebody builds this way. Anything specific to one project stays in that
project, and the specific documents here say what they are specific to.

Two apps feed it at the moment.

## Credits

The idea for the repository came from
[github/spec-kit](https://github.com/github/spec-kit), which seemed to me
somewhat over prescriptive and to need too much maintenance, and that works
against some of the benefits of fast agentic iterations. I might change my mind
with more experience.

The spec here is the shortest version of their idea that still changes what an
agent builds. They write three documents per feature, a directory of templates,
numbered requirements, ranked user stories, and acceptance criteria in a formal
notation. This is one page with four headings, collapsed to a line when the
feature ships.
