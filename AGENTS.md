# Agent handoff

If a user sends you this repository or its GitHub link and says "set me up," "make
Claude write like me," or anything equivalent, treat that as the complete request.
Do not ask them to restate instructions; everything you need is here.

First `git clone` the repo (or fetch every file SETUP.md lists, raw). The
playbook spans six files; improvising from the README alone produces a
broken skill. Read `README.md` and all of `SETUP.md` before changing anything
on their machine, then follow the setup playbook in order:

1. Explain the outcome in plain words (SETUP.md step 0), including where their
   writing gets sent when the judge runs.
2. Collect a writing corpus with them (`templates/corpus-guide.md`).
3. Measure their voice into a fingerprint file (bodies only, verified claims).
4. Generate and install their personal skill from `templates/SKILL-template.md`,
   with the judge protocol and humanizer rules beside it, and validate it loads.
5. Rig it up (SETUP.md step 4.5): with their OK, add the routing line so the
   skill gets offered for external writing, and resolve any conflicting skill.
6. Calibrate the judge (four probes, median of three draws each), then prove it
   on 3 real pieces with the fresh-context judge, and iterate on their feedback
   until the drafts sound like them.

Hard rules, even during setup: never send or publish anything, never fabricate facts
in a draft, never skip or fake the judge, and stop after 3 failed judge attempts and
let the human decide. The full boundary list is at the end of SETUP.md and it is
part of the contract.

This repo is instructions plus a few commands to run at setup time (the corpus
fetch for the fallback path, judge subagents, the optional cross-family CLI
check). There is nothing to build.
