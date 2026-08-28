# Agent handoff

If a user sends you this repository or its GitHub link and says "set me up," "make
Claude write like me," or anything equivalent, treat that as the complete request.
Do not ask them to restate instructions; everything you need is here.

Read `README.md` and all of `SETUP.md` before changing anything on their machine,
then follow the setup playbook in order:

1. Explain the outcome in plain words (SETUP.md step 0).
2. Collect a writing corpus with them (`templates/corpus-guide.md`).
3. Measure their voice into a fingerprint file.
4. Generate and install their personal skill from `templates/SKILL-template.md`,
   with the judge protocol and humanizer rules beside it.
5. Prove it on 3 real pieces with the fresh-context judge, and iterate on their
   feedback until the drafts sound like them.

Hard rules, even during setup: never send or publish anything, never fabricate facts
in a draft, never skip or fake the judge, and stop after 3 failed judge attempts and
let the human decide. The full boundary list is at the end of SETUP.md and it is
part of the contract.

This repo is instructions, not software. There is nothing to build or run.
