# Prompt Engineering Basics

A short, practical primer on writing prompts that produce reliable output. Read
this first, then look at the prompts in this repo as worked examples of the ideas
below.

---

## The four parts of a high-quality prompt

Most ambiguity and most hallucination come from a prompt that is missing one of
these four parts. Include all four and the model has far less room to guess.

1. **Instruction** — the specific task you want performed.
   *Summarise, translate, debug, design, rewrite.*

2. **Context** — background that constrains the problem space.
   *Target audience, technical constraints, the surrounding system, what already
   exists.*

3. **Input data** — the specific content to process.
   *A block of code, an email draft, a spec document, a requirement.*

4. **Output indicator** — the exact format, length, style, or tone of the result.
   *"A Markdown table under 150 words." "Numbered console steps." "One paragraph,
   no bullet points."*

A prompt missing the **output indicator** gets inconsistent formatting. A prompt
missing **context** gets confident but wrong answers. Name all four.

---

## From a one-off prompt to a reusable skill

A one-off prompt lives in a single chat. A **skill** (or system prompt, or
instruction file) is a reusable behaviour contract you load once and invoke many
times. The prompts in this repo are skills — they are designed to behave the same
way every session.

The jump from prompt to skill is where most of the quality comes from. A good
skill adds, on top of the four parts above:

- A **persona** — who the AI is and how it thinks. Specific beats vague:
  "a senior AWS architect with 15+ years' experience" constrains far more than
  "an expert."
- **Hard rules** — what it must never do. One line each, ordered by severity.
- A **confidence threshold** — the single most effective anti-hallucination line
  is *"Only say things you are more than 90% sure about. If you are not sure, say
  so and stop."*
- **Session-start questions** — ask what you can't infer, all in one message,
  before producing anything.
- A **deterministic output structure** — fixed sections in a fixed order, so the
  user can predict exactly what they'll get.

The full method is in [meta/how-to-write-a-skill.md](meta/how-to-write-a-skill.md).

---

## Patterns worth stealing

These show up across the prompts in this repo because they work:

- **One primary path, not a menu.** Multiple options transfer the decision back to
  the user. Give the single best recommendation plus, at most, one labelled
  "Optional" variation.
- **Clarify before acting.** When the request is ambiguous, ask one focused
  question rather than producing a confident wrong answer.
- **Flag but don't fix.** When reviewing user-provided content, point out a
  problem you weren't asked about in one line — don't silently rewrite it.
- **Banned phrases.** Listing specific phrases to avoid ("I hope this email finds
  you well") is far more enforceable than "be concise."
- **Internal reference tables.** Decision tables the AI reasons from but doesn't
  print, so it chooses consistently without asking every time.

---

## A minimal checklist

Before you send a non-trivial prompt, confirm:

- [ ] The instruction is a specific action, not a vague ask.
- [ ] The context the model needs is actually in the prompt.
- [ ] The input data is clearly delimited from the instructions.
- [ ] The output format, length, and tone are stated.
- [ ] For anything reusable: persona, rules, confidence threshold, and a fixed
      output structure are all present.
