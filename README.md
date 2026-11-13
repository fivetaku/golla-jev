English | [한국어](README.ko.md) | [中文](README.zh.md) | [日本語](README.ja.md) | [Español](README.es.md)

# golla-jev (골라제브)

<p align="center">
  <img src="assets/golla-jev-hero.png" alt="golla-jev" width="320">
</p>

> "Golla golla~" ("pick one, pick one!") is what Korean market vendors shout. Office work is full of picking too: which expense category, does this mail need a reply, is this clause risky. golla-jev finds those **pick-one moments** in your work and designs a workflow that hands them to Jev.

Jev (TypeSafe System One) does not write text. It picks from fixed options and answers with probabilities in about 0.2 s per call. golla-jev takes a description of your work and returns a design that gives the picking to Jev, arithmetic, dates and lookups to code, sentences to templates, and photo or audio reading to a multimodal model.

## Quick start

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
/plugin install golla-jev
```

Claude Code 2.1.275+: `/plugin install golla-jev --marketplace fivetaku/gptaku_plugins`

Restart Claude Code, then try:

- `/golla-jev I want to automate our team's receipt reimbursements`
- "golla-jev, where could Jev help in my company?" — broad scope lists 3–5 candidates first (idea mode)
- "Design a Jev workflow for this task" — designs one task end to end (design mode)

## What you get

- A one-line flow: input → transcription → N Jev questions → script → output
- A step table assigning each part to Jev, code, an LLM or a person
- Draft Jev question JSON with an example state
- A preview of the output document, each field tagged [J] Jev, [C] code, [원] source text, [사람] human
- Similar public cases with numbers, failure points and how to guard them
- A test plan: sample size, locked answers, dev/holdout threshold procedure

## Example designs

Three designs golla-jev actually produced, included unedited apart from the review notes. Read one first to see what a one-paragraph task description turns into.

- [Receipts → expense report](examples/01-expense-report.md) — design mode
- [Shared inbox → kanban board and weekly report](examples/02-mail-board.md) — design mode
- [Influencer shortlist](examples/03-influencer-screening.md) — idea mode, then design mode

## Bundled benchmark template

`skills/golla-jev/bench/` holds a 108-item Korean office benchmark over 5 workflows (expense category, mail board, meeting notes, survey, contract clauses). Items are hand-written, not customer data. The author's run on 2026-11-13 with jev-1.14.0 scored 96.3% (208/216) with Korean questions and 93.5% with English ones. A 0.70 threshold picked on the dev split auto-handled 97% (57/59) of holdout items with zero errors among them. A month earlier, jev-1.13.0 gave 0.85 under the same procedure, so pick the threshold again whenever the model changes (`bench/REPORT.md`).

```
cp -r <plugin path>/skills/golla-jev/bench ./my-bench && cd my-bench
python3 build_items.py          # after editing the WF dict for your task
python3 run.py                  # needs TYPESAFE_API_KEY (do not type the value on the command line)
python3 analyze.py && python3 sweep.py
```

## Requirements

- Claude Code
- Running the drafted questions needs a TypeSafe API key (paid, https://typesafe.ai). Design output alone needs no key.
- Recommended: the official `typesafe:typesafe-ai` plugin. golla-jev checks model IDs, prices and limits there and on docs.typesafe.ai instead of from memory.
- Benchmark: Python 3.9+, optional `certifi`

## What it won't do

- It doesn't stop at "Jev fits / doesn't fit". Parts that don't fit go to another owner so the flow is complete.
- It doesn't recommend someone else's threshold. You pick the threshold from your own sample.
- It doesn't ask Jev to write text, do arithmetic or count dates.

## License

MIT. An independent project, not affiliated with TypeSafe. See [DISCLAIMER](DISCLAIMER.md).
