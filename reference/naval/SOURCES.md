# Building the Naval corpus at setup time

The default corpus is the public writing of Naval Ravikant. This repo does not ship his
writing. His words are his copyright, and this is a public repo. Instead, you (the setup
agent) build the corpus on the user's machine, at setup time, for the user's personal use.

Save what you collect to the new skill's `corpus/` directory as plain text or markdown
files, one source per file. Keep every piece verbatim. Do not paraphrase, do not clean up.

## Sources, in order of preference

1. **nav.al** (his blog). Fetch pages from https://nav.al. Prefer the short essays and
   transcribed talks. Good starting points: the "How to Get Rich" series pages and the
   happiness essays. Save 10 to 20 pieces.
2. **The "How to Get Rich (without getting lucky)" tweetstorm** (May 2018, @naval on X).
   Widely mirrored; nav.al hosts expanded versions of each tweet as posts. If you can
   search X, pull his most-liked original posts (not replies, not retweets).
3. **The Navalmanack** (navalmanack.com). Eric Jorgenson's collection of Naval's
   words, free to read online and as a PDF. Fallback only: its own disclaimer says it
   stitches together edited transcripts, tweets, and talks, so it is Naval's ideas in
   a partly editorial voice. Prefer his first-hand writing above; if you must use it,
   take passages where he is clearly speaking and label the files `channel: transcript`.

Target: 3,000 to 8,000 words of his actual writing across at least 10 distinct pieces.

## If every fetch fails

Use `STYLE-PROFILE.md` in this directory as the voice source instead, and tell the user
plainly: "I couldn't fetch Naval's writing, so I'm working from a style description.
Matching will be looser." A style description cannot support an authorship comparison,
so with zero real samples the skill runs PROVISIONAL: no judge scores, no pass claims,
just the description-guided draft. Say that out loud, and offer the paste-your-own
path as the better option.

## One warning to give the user

The Naval default makes writing sound like Naval: compressed, aphoristic, philosophical.
That is a strong flavor. It works for essays and posts. It is a strange voice for, say,
a scheduling email. If the user mainly wants emails in their own voice, steer them to
the paste or email-mining paths.
