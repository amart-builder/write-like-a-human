# Write Like a Human

Make your AI write like you. Not "professional and polished." You.

## How to use it

Send your AI tool this repo link and say "set me up." If your tool likes a fuller
prompt, paste this:

> Clone https://github.com/amart-builder/write-like-a-human (start with AGENTS.md
> and SETUP.md, and fetch the whole repo, not just this README) and set me up:
> collect my writing samples, build my voice skill, and prove it on three real
> drafts. Follow the repo's playbook exactly.

That's the whole interface. Your AI reads this repo and walks you through the rest.
Works best with Claude Code; any agent that can read a repo, write local files, and
spawn a clean-context second agent can run the full version. Agents with fewer
abilities (no files, no subagents) get a degraded version per SETUP.md that is
built to say what it can't verify instead of faking it: same craft, weaker
verification, labeled as such.

## What it actually does

First it collects your real writing. Paste 10 or so samples, or let it mine emails
you've sent (it shows you what it keeps, you approve). No samples and don't care
whose voice it is? There's an opt-in fallback that builds a corpus from Naval
Ravikant's public writing. Fair warning: that one makes everything sound like
Naval, runs without numeric score claims, and everything it drafts has to be
presented as openly style-borrowed.

Then it measures how you actually write. Not "casual and friendly." Measured: how
long your emails run, how you open, how you close, what punctuation you abuse, the
things you never do. Your voice is mostly habits, and habits can be measured and
imitated.

Then it installs a skill with a hostile judge built in. Every time your AI drafts
something in your voice, a second AI that never saw your conversation gets your
real writing and a police-style lineup: the draft shuffled in with real pieces of
yours it has never seen (when the corpus is big enough to hold pieces back; a
small corpus gets the draft scored on its own). It tries to pick the fake and
scores 0 to 100 how likely the same person wrote each piece. A draft is delivered
as a pass only when the judge doesn't confidently pick it out AND the score clears
a calibrated gate. Fail either, and the writer gets told exactly which lines gave
it away and tries again. Three strikes and it stops and shows you the best attempt
with the judge's objections. Expect that to happen sometimes: a gate honest
enough to catch AI writing also catches drafts, and "you decide" is a normal
ending, not a malfunction. Every draft you see carries a status stamp; it quotes
a number only when calibration has earned one.

It also wires itself in: with your OK, setup adds one routing line to your
agent's standing instructions so external-facing writing gets offered this skill
instead of depending on you to remember it exists.

Your AI drafts. You send. Nothing goes out on its own.

## Why the judge matters

Everyone has a prompt that says "write casually, don't sound like AI." It doesn't
work well, because the model grading its own writing grades gently. A fresh
judge that never saw the conversation just reads your writing, reads the draft,
and says whether it would be fooled. If it can pick your draft out of the lineup,
or the score misses the bar, the draft doesn't leave the building. And where a
second AI vendor is available, the final check runs on that other vendor's model
too, as a decorrelated second opinion with a veto.

Because a judge's number is only as good as the judge, setup calibrates it first,
with repeated draws: your own held-out writing has to pass the gate, a generic AI
draft has to fail it, and the setup agent's own best imitation of you gets scored
and recorded, so you know whether the gate can tell a good imitation from the
real thing or only slop from the real thing. If calibration fails or gets
skipped, the skill says so and runs without score claims until the corpus earns
them. No fake certainty: what the gate provably catches is generic AI writing,
and the calibration record tells you honestly how much more than that it catches
on your corpus.

The writer is also held to [blader/humanizer](https://github.com/blader/humanizer),
a community catalog of 29 AI-writing tells (vendored in this repo, MIT), before
the judge ever sees a draft. The catalog catches the obvious slop; the judge is
there for what a word list can't see.

## What's in the repo

- `AGENTS.md`, `SETUP.md`: the playbook your AI follows.
- `templates/`: the skill it generates, the judge protocol, the corpus guide.
- `reference/humanizer-rules.md`: the AI-tells rulebook.
- `reference/naval/`: the fallback-corpus instructions. His actual writing is
  fetched to your machine at setup time, not stored in this repo (his words, his
  copyright).

## The fine print

Your corpus is stored on your machine, and drafting and judging send your samples
and drafts to the AI providers involved: the model that writes and judges, plus
the second vendor when the cross-family check runs (and, if you choose email
mining, the mining step itself sends candidate emails to the model provider and
whatever connector serves your mail). No other service receives them, and
sending, posting, and publishing are always yours; the skill only drafts. This is
for writing as yourself (or openly borrowing a public author's style, with that
openness required). Using it to pass off writing as someone else's is
impersonation; the setup instructions and the generated skill itself both refuse
that.

MIT licensed. Humanizer rules by [Siqi Chen](https://github.com/blader/humanizer),
distributed by its upstream as MIT; the catalog itself credits Wikipedia's
"Signs of AI writing" guide (CC BY-SA) as its source, and the vendored copy
keeps both notices.
