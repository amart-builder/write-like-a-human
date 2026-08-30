# Write Like a Human

Make your AI write like you. Not "professional and polished." You.

## How to use it

Send your AI tool this repo link and say "set me up." If your tool likes a fuller
prompt, paste this:

> Read https://github.com/amart-builder/write-like-a-human (start with AGENTS.md and
> SETUP.md) and set me up: collect my writing samples, build my voice skill, and
> prove it on three real drafts. Follow the repo's playbook exactly.

That's the whole interface. Your AI reads this repo and walks you through the rest.
Works best with Claude Code; any agent that can read a repo, write local files, and
spawn a clean-context second agent can run the full version, and SETUP.md tells
agents with fewer abilities how to degrade honestly instead of faking it.

## What it actually does

First it collects your real writing. Paste 10 or so samples, or let it mine emails
you've sent (it shows you what it keeps, you approve). Don't have samples or don't
care? There's a default that builds a corpus from Naval Ravikant's public writing
instead. Fair warning: that one makes everything sound like Naval.

Then it measures how you actually write. Not "casual and friendly." Measured: how
long your emails run, how you open, how you close, what punctuation you abuse, the
typos you leave in, the things you never do. Your voice is mostly habits, and habits
can be copied precisely.

Then it installs a skill with a hostile judge built in. Every time your AI drafts
something in your voice, a second AI with zero context gets your real writing and
a police-style lineup: the draft shuffled in with real pieces of yours it has never
seen (when the corpus is big enough to hold pieces back; a small corpus gets the
draft scored on its own). It has to try to pick the fake, and it scores 0 to 100 how likely the same
person wrote each piece. A draft ships only when the judge can't confidently pick
it out AND the score clears the gate. Fail either, and the writer gets told exactly
which lines gave it away and tries again. Three strikes and it stops and shows you
the best attempt with the judge's objections. Every draft you see carries its score.

Your AI drafts. You send. Nothing goes out on its own.

## Why the judge matters

Everyone has a prompt that says "write casually, don't sound like AI." It doesn't
work, because the model grading its own writing believes its own excuses. A fresh
judge that never saw the conversation has no excuses to believe. It just reads your
writing, reads the draft, and says whether it would be fooled. If it can pick your
draft out of the lineup, or the score misses the bar, the draft doesn't leave the
building. And where a second AI vendor is available, the final check runs on that
other vendor's model too, because judges go easy on writing from their own family.

And because a judge's number is only as good as the judge, setup calibrates it
first: your own held-out writing has to pass the gate and a generic AI draft has to
fail it. If either check fails, the skill says so and runs without score claims
until the corpus, or the judge's consistency on it, is good enough to earn them.
No fake certainty.

The writer also has to clear [blader/humanizer](https://github.com/blader/humanizer),
a catalog of 29 known AI-writing tells (vendored in this repo, MIT), before the judge
ever sees it. The word list catches the obvious slop; the judge catches everything
the word list can't.

## What's in the repo

- `AGENTS.md`, `SETUP.md`: the playbook your AI follows.
- `templates/`: the skill it generates, the judge protocol, the corpus guide.
- `reference/humanizer-rules.md`: the AI-tells rulebook.
- `reference/naval/`: the default corpus instructions. His actual writing is fetched
  to your machine at setup time, not stored in this repo (his words, his copyright).

## The fine print

Your writing stays on your machine. The skill drafts and the judge scores; sending,
posting, and publishing are always yours. And this is for writing as yourself (or
openly borrowing a public author's style). Using it to pass off writing as someone
else's is impersonation, and the setup instructions tell the agent to refuse.

MIT licensed. Humanizer rules by [Siqi Chen](https://github.com/blader/humanizer),
also MIT.
