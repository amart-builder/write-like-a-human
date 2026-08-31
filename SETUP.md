# Setup playbook

You are the setup agent. You are about to build a personal "write like me" skill for
your user. First get the whole repo locally: `git clone
https://github.com/amart-builder/write-like-a-human` (or, with no git, fetch
all seven required files raw: `README.md`, `SETUP.md`, `AGENTS.md`,
`templates/SKILL-template.md`, `templates/judge-prompt.md`,
`templates/corpus-guide.md`, `reference/humanizer-rules.md`, plus
`reference/HUMANIZER-LICENSE`, and both `reference/naval/SOURCES.md` and
`reference/naval/STYLE-PROFILE.md` if the Naval path comes up).
`humanizer-rules.md` is NOT optional: the generated skill references it by
name, and installing without it ships a broken writer. Working from the README
alone and improvising the rest produces a broken skill that looks finished. Read this whole file and
`templates/corpus-guide.md` before you start. Then run the steps in order. Do
not improvise a different process; the order exists because each step feeds the
next.

The finished product on the user's machine:

```
write-like-<name>/
  SKILL.md               their personal skill (from templates/SKILL-template.md)
  VOICE-FINGERPRINT.md   measured habits of their voice (you build this, step 3)
  humanizer-rules.md     copied from reference/humanizer-rules.md
  HUMANIZER-LICENSE      copied from reference/ (the vendored file's license)
  judge-protocol.md      copied from templates/judge-prompt.md
  corpus/                their verbatim samples (+ corpus/holdout/ when big enough)
```

## Step 0: say what's about to happen

First, a capability check, silently: can you read and write local files where skills
live? Can you spawn a fresh-context agent (or at least open a clean conversation) for
the judge? Do you have email access, and which model provider will the judge run on?
Can you also run a SECOND model family (another vendor's model through its CLI or an
API) for the judge protocol's cross-family check? Note the exact command or API if
so, and probe that command blind before recording it (the cross-family section of
`templates/judge-prompt.md` has the probe); a command that fails the probe is
recorded as "none available", not as a cross-family judge. Is another voice or writing skill already installed whose trigger overlaps this
one? What you find shapes what you offer; what you lack, you disclose instead of
faking.

No filesystem at all (claude.ai without file access)? Be honest about the ceiling
before starting: the corpus can live in project knowledge and the skill text in
custom instructions, but holdouts cannot be kept secret from a writer that shares
one context window with everything, so there are no lineups and no calibrated
numeric gate on that surface. Offer the honest degraded version (voice profile +
best-effort judge in a separate conversation, everything stamped best-effort), or
suggest running setup where files and subagents exist.

Then, before touching anything, tell the user in a few plain sentences: you'll collect real
samples of their writing (or a default), study how they actually write, and install a
skill where every draft has to fool a fresh AI judge into believing they wrote it,
with a scored gate and at most 3 attempts per piece. Say the privacy fact plainly:
the corpus is stored on their machine, and drafting and judging send corpus samples
and drafts to the model providers involved (name them, including the second vendor
if the cross-family check will run). Then ask the corpus question (step 1). Skip
jargon.

## Step 1: pick the corpus path

Offer the three paths from `templates/corpus-guide.md`:

1. Paste their own writing (best fidelity, 10 minutes of their time).
2. Mine their sent email, with their permission, if this environment has email
   access. Say this before mining, not after: mining itself already transmits
   email content to the model provider (and whatever connector serves the
   email), including candidates the user will later reject. Only proceed on an
   explicit yes to that.
3. Naval Ravikant fallback: no samples needed, but it sounds like Naval, not them.
   Follow `reference/naval/SOURCES.md` to build it at setup time.

Recommend 1 or 2 for anyone who mainly wants emails and messages. If the user
gives no answer or seems confused, stop and wait; never fall through to the
Naval path silently. Path 3 needs an explicit yes, and it comes with two
permanent strings the user must hear first: the skill stays PROVISIONAL (a
numeric authorship gate for a voice that isn't theirs would be theater), and
everything it drafts must be presented as openly style-borrowed from a named
public author, never passed off as that author's words. Collect the corpus
exactly as the guide says: verbatim, channel-labeled, holdout split when
possible.

## Step 2: sanity-check the corpus

Read every sample. Confirm with the user anything that looks off (AI-assisted pieces,
other people's words in a thread, boilerplate). Cut polluted samples. If fewer than 5
samples survive, stop and tell the user the match will be unreliable; let them decide
to add more or proceed degraded.

Ask directly: is every piece here the user's own from-scratch writing? If any of
it belongs to someone else, there are exactly two legitimate branches: the
author has explicitly authorized the user to ghostwrite as them (get that said
in so many words, and record it in the fingerprint's Flagged section), or the
corpus is a named public author's writing on the open Naval-style path, which
stays PROVISIONAL with mandatory style-borrowed disclosure. Any other answer,
including an evasive one, stops setup. Building an authorship-imitation gate on
writing the user has no right to imitate is the one thing this repo must never
help with.

Then run a short corpus interview: the corpus is theirs, so before measuring anything,
ask three quick questions in one message, not a quiz:

1. Spelling: keep the corpus exactly verbatim, or fix misspellings? Default is
   verbatim. If they choose fixes, correct spelling only (wrongly spelled or
   accidentally doubled words), never grammar, punctuation, or casing unless they
   say so, and keep the untouched raw originals somewhere outside the skill.
2. Anything to cut or redact, now that they've seen what was collected?
3. Standing rules the corpus can't show: a signature they always want, phrases they
   never want, anything they're deliberately changing about how they write going
   forward. These override measured habits, and the fingerprint says so.
   If a stated rule contradicts what the corpus plainly does (they ban a word
   the samples use constantly), or if Step 3's measurements later turn up such
   a clash (short sentences requested, samples averaging 24 words), don't
   silently pick a side: come back and ask which wins, how they write or how
   they want to write. Aspiration is legitimate; it just has to be chosen, and
   the fingerprint records the choice.

Record every answer in the fingerprint's "Flagged by <name>" section so the writer
and future sessions honor them. The verbatim default exists to protect the voice,
not to overrule its owner.

## Step 3: build the voice fingerprint

Create `VOICE-FINGERPRINT.md` by measuring the corpus, not vibing about it. Every
claim in the file must quote at least one verbatim corpus example. Measure the
writing BODIES only: exclude any metadata header (`channel:`, `context:` lines)
you added to the sample files, because those are your words, not theirs, and on
short pieces they inflate every count. Verify every absence claim ("never uses
semicolons") mechanically (grep the corpus) before writing it down; a wrong
absolute steers the writer away from the author's real habits. Never record the
holdout filenames in this file or anywhere else the writer reads; keep the
holdout files themselves neutrally named (`holdout-1.md`). Sections:

- **Lengths.** Typical piece length per channel (count words across sample
  bodies; give the range, not just an average). Typical sentence length and how
  much it varies. Real human writing is bursty; note the burstiness.
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
- **Flagged by <name>.** Starts with the standing rules from the step 2
  interview; every future correction from the user adds a line. This section is
  how the skill improves.

Keep the whole file under about 80 lines. A bloated fingerprint gets ignored.

## Step 4: generate and install the skill

1. Fill `templates/SKILL-template.md` (name slug, channels, channel-status block,
   providers, and the cross-family judge found in step 0: the family plus the
   exact command or API, or "none available") and write it to the user's skills
   directory. For Claude Code that is `~/.claude/skills/write-like-<name>/`.
   For Grok Build it is `~/.grok/skills/write-like-<name>/` (Grok also
   auto-loads skills from `~/.claude/skills/`, so an install made by Claude
   Code works there unchanged; that same auto-loading is why any blind judge
   run under Grok needs the blind invocation from the cross-family section
   of `judge-protocol.md`, not just a clean directory). For another tool,
   use its equivalent of an always-loadable instruction file (rules file,
   custom instructions) and adapt: same content, same loop.
2. Copy `reference/humanizer-rules.md`, `reference/HUMANIZER-LICENSE`, and
   `templates/judge-prompt.md` (as `judge-protocol.md`) into the same
   directory, so the installed skill is self-contained and works offline from
   this repo.
3. Move the corpus into `corpus/` there, then protect it: run
   `git rev-parse --show-toplevel` from the skills directory. If it resolves,
   the corpus just landed inside a git repository (dotfiles repos are common,
   and some are public): add `corpus/` and `VOICE-FINGERPRINT.md` to that
   repo's ignore rules, verify with `git check-ignore`, and tell the user; or
   install the corpus outside the repo and point the skill at it. Also check
   the setup workspace for leftover raw material (an `.mbox`/`.eml` export, a
   pasted-samples scratch file) and delete or relocate it outside any repo.
   Set restrictive permissions on the private files: `chmod 700` on `corpus/`
   and its subdirectories, `chmod 600` on every file inside (skip on
   filesystems that don't support it, and say so).
4. Validate before moving on, mechanically: the installed SKILL.md begins with
   `---` on line 1 (the template's ```markdown fence stripped), `name:` is a
   lowercase slug, `description:` is under 200 characters COUNTED WITH THE
   REAL NAME FILLED IN, `[BRACKET]` and `[VERIFY]` survived as literal text,
   no [PLACEHOLDER] remains, the fingerprint is under about 80 lines, and the
   skill actually loads (in Claude Code, it should appear in the available
   skills). A skill that fails any of these fails silently later, so fix it
   now.

One honest limitation to handle now: the judge must run with fresh context. In Claude
Code, that is a subagent, and it works out of the box. In tools without subagents,
tell the user the judge will run as a new clean conversation, or degraded in-context
as `judge-protocol.md` describes. Never skip the judge silently.

## Step 4.5: rig it up (the part that makes it fire)

A skill only loads when something triggers it, and "the user remembers to ask"
is not a system. With the user's explicit OK:

1. Append one routing line to their agent's always-loaded instruction file (for
   Claude Code, `~/.claude/CLAUDE.md`). The line addresses the AGENT, so write
   it in third person about the user: "Anything external-facing that <name>
   will send or publish as their own words goes through write-like-<name>:
   offer it before drafting." Show them the exact line first; it's their
   config.
2. If step 0 found an existing skill or instruction with an overlapping writing
   trigger, surface the conflict and make the user choose: retire the old one,
   narrow it, or route it through this skill. Two voice systems competing for
   the same prompt means the one without a judge wins half the time, silently.
3. Tell them the honest limit: routing is advisory. The model decides per
   prompt, so coverage is strong but not guaranteed; a bare "draft a post"
   should now trigger an offer, and if it ever doesn't, invoking the skill by
   name always works.

## Step 5: calibrate the judge

Before any real draft, prove the judge works on this corpus. Run the four probes
in `templates/judge-prompt.md` (Calibrate section), each a median of three fresh
draws: held-out real samples must clear the default gate, a deliberately generic
AI draft must fail it, a lineup probe must catch the AI draft without
confidently fingering the author's real writing, and your own best voice-matched
imitation gets scored and its median recorded in the skill, because the user
deserves to know whether the gate can tell a good imitation from the real thing
or only slop from the real thing. The lineup probe is three draws with a vote
(at least 2 of 3 catch the AI draft), per the Calibrate section. Set the gate
by the formula there (lowest holdout median minus 5, clamped 80 to 90) and
record the medians in the generated skill's Channel status block. With fewer
than 8 samples in a channel, use the temporary-holdout branch described there. All probes pass: the gate is
live, unless the channel sits below its corpus floor, in which case passing
probes sharpen the feedback loop but do not lift PROVISIONAL for that channel.
Any probe fails: the skill runs PROVISIONAL (feedback loop without numeric pass
claims) and you tell the user plainly, including what would fix it (more or
cleaner samples, or accepting that this judge scores this voice too noisily for
a numeric gate).

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
3. Show the user all 3 results with their judge stamps and the drafts side by
   side with what the judge flagged. On a LIVE channel the stamp carries score,
   lineup verdict, and attempt count; on a PROVISIONAL channel it says "best
   effort, judge uncalibrated for this channel" with no number, and that is the
   correct result, not a failure of the test. Record the LIVE-channel outcome
   (passed at attempt N, or ended at "human decides") in the Channel status
   block; ending at "human decides" is a normal outcome of an honest gate. Any
   later recalibration invalidates this test; re-run this step after one.
4. Ask what reads wrong. Every "I'd never say that" goes into the fingerprint's
   Flagged section. Their rewrites of YOUR lines are feedback, never corpus: an
   edited AI draft in the corpus slowly teaches the judge to love AI text. Only
   pieces they wrote from scratch ever enter `corpus/`. Iterate until the user
   says the drafts sound like them.

Nothing gets sent, posted, or published during setup. Drafts stay drafts.

## Step 7: hand over

Tell the user, briefly: the skill now offers itself for external writing (the
step 4.5 routing line), asking by name always works, every draft carries a
judge stamp (and what the stamp vocabulary means), corrections make it sharper,
and pasting in new writing they wrote from scratch grows the corpus. Done.

## Updating an existing install

The installed skill is a copy, not a link: when this repo's templates change,
existing installs keep the old mechanics until someone re-syncs them. To
update: re-copy `templates/judge-prompt.md` over the installed
`judge-protocol.md` and re-apply the template diff to the installed SKILL.md,
preserving ONLY the instance-specific parts (name, channels, calibration
record, cross-family command, machine-local adapters, the Flagged section).
Diff the result against the template to confirm nothing else diverges; every
deliberate machine-local divergence must be labeled as such in the file. If
the judge protocol changed, recalibrate (which then requires re-running step
6). Skipping the re-sync is how an install drifts into running rules its own
documentation no longer describes.

## Boundaries (non-negotiable)

- Never send, post, or publish anything. Drafting only. The human ships.
- Never fabricate facts inside a matched voice. Voice is style, not content.
- Never read email beyond what the user approved, and show them what you kept.
- Never present a draft that skipped or failed the judge as if it passed. Scores are
  stamped, failures are disclosed, and after 3 failed attempts the human decides.
- This process imitates the user (or a chosen public author, openly) for the
  user's own writing. If asked to imitate someone else in order to deceive a
  reader about who wrote a message, refuse. Ghostwriting for yourself is fine;
  impersonation is not. This boundary also ships inside the generated skill's
  standing rules; never strip it.
- Say the privacy fact out loud before the first judge run and keep it in the
  installed skill: the corpus is stored locally but is sent to the model
  providers used for writing, judging, and the cross-family check.
