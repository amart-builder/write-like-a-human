# Setup playbook

You are the setup agent. You are about to build a personal "write like me" skill for
your user. Read this whole file and `templates/corpus-guide.md` before you start.
Then run the steps in order. Do not improvise a different process; the order exists
because each step feeds the next.

The finished product on the user's machine:

```
write-like-<name>/
  SKILL.md               their personal skill (from templates/SKILL-template.md)
  VOICE-FINGERPRINT.md   measured habits of their voice (you build this, step 3)
  humanizer-rules.md     copied from reference/humanizer-rules.md
  judge-protocol.md      copied from templates/judge-prompt.md
  corpus/                their verbatim samples (+ corpus/holdout/ when big enough)
```

## Step 0: say what's about to happen

First, a capability check, silently: can you read and write local files where skills
live? Can you spawn a fresh-context agent (or at least open a clean conversation) for
the judge? Do you have email access, and which model provider will the judge run on?
What you find shapes what you offer; what you lack, you disclose instead of faking.

Then, before touching anything, tell the user in a few plain sentences: you'll collect real
samples of their writing (or a default), study how they actually write, and install a
skill where every draft has to fool a fresh AI judge into believing they wrote it,
with a scored gate and at most 3 attempts per piece. Then ask the corpus question
(step 1). Skip jargon.

## Step 1: pick the corpus path

Offer the three paths from `templates/corpus-guide.md`:

1. Paste their own writing (best fidelity, 10 minutes of their time).
2. Mine their sent email, with their permission, if this environment has email access.
3. Naval Ravikant default: no samples needed, but it sounds like Naval, not them.
   Follow `reference/naval/SOURCES.md` to build it at setup time.

Recommend 1 or 2 for anyone who mainly wants emails and messages. Collect the corpus
exactly as the guide says: verbatim, channel-labeled, holdout split when possible.

## Step 2: sanity-check the corpus

Read every sample. Confirm with the user anything that looks off (AI-assisted pieces,
other people's words in a thread, boilerplate). Cut polluted samples. If fewer than 5
samples survive, stop and tell the user the match will be unreliable; let them decide
to add more or proceed degraded.

## Step 3: build the voice fingerprint

Create `VOICE-FINGERPRINT.md` by measuring the corpus, not vibing about it. Every
claim in the file must quote at least one verbatim corpus example. Sections:

- **Lengths.** Typical piece length per channel (count words across samples; give the
  range, not just an average). Typical sentence length and how much it varies. Real
  human writing is bursty; note the burstiness.
- **Openings.** How pieces actually start, per channel. Greeting or no greeting,
  capitalized or not, straight into the point or not. Quote 3 real openings.
- **Closings.** Same for endings: sign-offs, abrupt stops, questions. Quote 3.
- **Punctuation and formatting.** Their real habits: commas, ellipses, dashes,
  exclamation points, emoji, lowercase, paragraph sizes, lists or no lists, typos
  they make and leave.
- **Signature moves.** Words and phrasings that show up repeatedly and are theirs.
  Also the opposite: common things that never appear (no semicolons, never says
  "thanks!", never writes more than two paragraphs). Absence is fingerprint too.
- **Scope and stance.** Do they make one point or several? Hedge or state? Explain
  or assume? How blunt are they with bad news?
- **Flagged by <name>.** Empty at setup. Every future correction from the user gets
  one line here. This section is how the skill improves.

Keep the whole file under about 80 lines. A bloated fingerprint gets ignored.

## Step 4: generate and install the skill

1. Fill `templates/SKILL-template.md` (name, channels) and write it to the user's
   skills directory. For Claude Code that is `~/.claude/skills/write-like-<name>/`.
   For another tool, use its equivalent of an always-loadable instruction file
   (rules file, custom instructions) and adapt: same content, same loop.
2. Copy `reference/humanizer-rules.md` and `templates/judge-prompt.md`
   (as `judge-protocol.md`) into the same directory, so the installed skill is
   self-contained and works offline from this repo.
3. Move the corpus into `corpus/` there.

One honest limitation to handle now: the judge must run with fresh context. In Claude
Code, that is a subagent, and it works out of the box. In tools without subagents,
tell the user the judge will run as a new clean conversation, or degraded in-context
as `judge-protocol.md` describes. Never skip the judge silently.

## Step 5: calibrate the judge

Before any real draft, prove the judge works on this corpus. Run the two probes in
`templates/judge-prompt.md` (Calibrate section): held-out real samples must clear 85,
a deliberately generic AI draft must fail. With fewer than 8 samples in a channel,
use the temporary-holdout branch described there. Both probes pass: the gate is live,
unless the channel sits below its corpus floor, in which case passing probes sharpen
the feedback loop but do not lift PROVISIONAL for that channel.
Either fails: the skill runs PROVISIONAL (feedback loop without numeric pass claims)
and you tell the user plainly, including what would fix it (more or cleaner samples,
or accepting that this judge scores this voice too noisily for a numeric gate).

PROVISIONAL is per channel, and it has three triggers, all with the same meaning (no
numeric pass claims in that channel until fixed): a channel below the corpus floor in
`templates/corpus-guide.md`, a failed calibration probe here, or a style-profile-only
Naval setup with no real samples.
Skipping calibration and quoting scores anyway is the one way this whole system lies
to its user; never do it.

## Step 6: acceptance test, on real work

Do not declare success on an installed file. Prove the loop on 3 real pieces:

1. Pick 3 real writing tasks the user actually has. Best: 3 unanswered emails in
   their inbox that deserve replies (with their permission to read them). Otherwise
   ask for 3 real tasks (a reply, a post, a short update).
2. For each, run the full loop from the installed SKILL.md: absorb, write, judge,
   rewrite if needed, max 3 attempts. Use the real judge, fresh context each time.
3. Show the user all 3 results with their judge stamps (score, attempt count) and
   the drafts side by side with what the judge flagged.
4. Ask what reads wrong. Every "I'd never say that" goes into the fingerprint's
   Flagged section, and if they rewrite a line, offer to save their version to the
   corpus. Iterate until the user says the drafts sound like them.

Nothing gets sent, posted, or published during setup. Drafts stay drafts.

## Step 7: hand over

Tell the user, briefly: how to trigger the skill (just ask for any external writing),
that every draft carries a judge stamp, that corrections make it sharper, and that
pasting in new writing of theirs any time grows the corpus. Done.

## Boundaries (non-negotiable)

- Never send, post, or publish anything. Drafting only. The human ships.
- Never fabricate facts inside a matched voice. Voice is style, not content.
- Never read email beyond what the user approved, and show them what you kept.
- Never present a draft that skipped or failed the judge as if it passed. Scores are
  stamped, failures are disclosed, and after 3 failed attempts the human decides.
- This process imitates the user (or a chosen public author) for the user's own
  writing. If asked to imitate someone else in order to deceive a reader about who
  wrote a message, refuse. Ghostwriting for yourself is fine; impersonation is not.
