# How to Write an Effective SKILL.md

### A practical guide based on real skill patterns that work

---

## What is a SKILL.md?

A SKILL.md is a structured instruction file that tells an AI exactly how to
behave for a specific domain or task. It is not a prompt. It is not a
system message. It is a repeatable, invokable behaviour contract.

When written well, it:
- Eliminates hallucination by constraining what the AI can and cannot say
- Produces consistent output every single time
- Knows when to ask and when to act
- Adapts to the user without losing its core behaviour

When written badly, it:
- Gives the AI too much freedom → hallucination and inconsistency
- Over-constrains → rigid, unhelpful output
- Asks too many questions → friction before value
- Asks too few → wrong output on first attempt

---

## The Anatomy of a Good Skill

Every effective skill has these parts in this order:

```
1. Frontmatter        — name, description, triggers
2. Persona            — who the AI is, how it thinks
3. Rules              — what it must never do
4. Session Start      — what it asks before acting
5. Flows              — how it handles each scenario
6. Output Structure   — exactly what it produces and in what order
7. Reference Tables   — internal decision guides
8. Tone & Format      — how it writes
```

Never skip any of these. Each one closes a different door to bad output.

---

## Part 1 — Frontmatter

```yaml
---
name: skill-name
description: >
  One paragraph. Covers:
  - What this skill does
  - When to use it (trigger scenarios)
  - Explicit trigger words or phrases
  - What it does NOT cover (if boundary is important)
---
```

### Rules for good frontmatter:
- The `description` is used by the AI to decide whether to invoke this skill.
  If it is vague, the skill will not trigger reliably.
- List explicit trigger words: service names, action verbs, file types,
  error patterns, or the slash command name itself.
- If two skills could overlap, describe the boundary clearly in both.

### Example — too vague (bad):
```yaml
description: Helps with AWS stuff.
```

### Example — specific (good):
```yaml
description: >
  Act as a senior AWS cloud architect for any AWS question or issue.
  Use when the user pastes anything AWS-related, asks about an AWS service,
  mentions an AWS error, or says "aws-guide". Triggers on: EC2, S3, IAM,
  Lambda, RDS, VPC, CloudFormation, ECS, EKS, Route53, CloudFront, or any
  AWS service name.
```

---

## Part 2 — Persona

The persona anchors the AI's identity for the entire session. Without it,
the AI drifts toward generic assistant behaviour.

### What to include:
- Role and seniority level (be specific — "15+ years" is more constraining
  than "experienced")
- Domain of expertise
- How they think (communication style)
- Confidence standard (the 90% rule)

### Template:
```markdown
You are a [role] with [X]+ years of hands-on experience in [domain].
You think like [reference point] — [communication style in 3–5 words].
You only say things you are more than 90% sure about.
If you are not sure, you say so and stop there.
```

### The 90% confidence rule — why it matters:
This single line is the most effective anti-hallucination constraint in any
skill. It forces the AI to:
- Stop before speculating
- Say "I'm not sure" instead of inventing an answer
- Ask a question instead of guessing

Always include it. Never weaken it.

---

## Part 3 — Rules

Rules are hard constraints. The AI must never break them.
This section is what separates a skill from a suggestion.

### How to write rules:

- Start every rule with "Never" or an active constraint verb
- Be specific — vague rules are ignored
- Order by severity — most critical first
- Keep each rule to one line
- Never write more than 8–10 rules — too many and none of them stick

### The universal rules that belong in almost every skill:

```markdown
## Rules (never break these)

- Never give multiple options — give the ONE primary path that best fits.
- Never suggest changes the user didn't ask about.
- Never give false positives. If something is unclear, ask.
- Keep every answer simple and straight to the point.
- Your confidence must be above 90% before stating anything as fact.
- Always offer ONE optional step at the end, labelled "Optional".
```

### Domain-specific rules examples:

For technical skills:
```markdown
- Primary fix path is always [AWS Console / UI]. Only give CLI or code
  if the user explicitly asks.
- Never recommend [EKS / EC2 / complex patterns] unless the user asks.
```

For writing skills:
```markdown
- Never use filler openers like "I hope this email finds you well".
- Never give two draft options — give the ONE best version.
```

### Why the "ONE primary path" rule is critical:
Multiple options feel helpful. They are not. They transfer the decision
back to the user, who came to the skill because they did not want to decide.
One clear recommendation with one optional variation is always better.

---

## Part 4 — Session Start

This is what the skill asks before it does anything. Get this wrong and
every output will be calibrated to the wrong context.

### Rules for Session Start:

- Ask every new session — do not assume context carries over
- Ask ALL questions in ONE message — never spread across multiple turns
- Ask only what you cannot infer — do not ask what the user already told you
- Maximum 4–5 questions — more than this is friction, not clarity
- Always wait for answers before proceeding — never assume and proceed

### Standard intake questions that apply to most skills:

```markdown
1. Level / experience of the user
   (Beginner / Intermediate / Experienced)

2. Intent / mode
   (Info/Knowledge vs Issue/Troubleshooting vs Generate/Create)

3. Any domain-specific context
   (e.g. serverless vs server, recipient type, deployment target)
```

### When to ask mid-conversation vs upfront:

Ask upfront: level, intent, broad context
Ask mid-conversation: specifics that only matter for a particular request

### If a document or existing content is provided:

```markdown
1. Extract requirements silently — do not repeat everything back
2. Identify only what is missing or unclear
3. Ask gap questions one at a time
4. Confirm your understanding in 3–5 lines before proceeding
```

---

## Part 5 — Flows

Flows define exactly what the AI does for each mode of use.
Every skill should have at least two flows. Most need three.

### Standard flow types:

| Flow | When to use |
|---|---|
| Info / Knowledge | User wants to understand something |
| Generate / Create | User wants something produced (code, email, diagram) |
| Issue / Troubleshoot | Something is broken or not working |

### How to write a flow:

Each flow must define:
1. Trigger — when does this flow activate?
2. Clarify step — what does it ask if unclear?
3. Output steps — exactly what it produces, in what order
4. Optional — one extra thing, clearly labelled

### The clarify-before-act pattern (critical for quality):

```markdown
### Step 1 — Clarify if needed
If [condition is ambiguous], ask one focused question at a time until
you are 90%+ confident. Do not [guess / assume / proceed without clarity].
```

This pattern prevents the most common failure mode: the AI produces a
confident but wrong output because it assumed instead of asked.

### Flow output must be deterministic:

The user should be able to predict exactly what they will get.
Bad: "I'll help you with that."
Good: numbered steps, labelled sections, fixed output order every time.

---

## Part 6 — Output Structure

Define exactly what the skill produces and in what order.
This is the contract between the skill and the user.

### Rules for output structure:

- Name every section with a header
- Specify length constraints ("one or two lines max", "3–5 bullet points")
- Specify format ("numbered steps", "code block", "one paragraph")
- Specify what adapts to context (e.g. depth based on level)
- Specify what never changes (e.g. always include Optional)

### Example output contract:

```markdown
Respond in this exact order:

1. Summary          — 1–2 lines, adjusted to user level
2. Purpose          — 1 line
3. When to use      — 2–3 bullet points, real scenarios only
4. How to create    — numbered console steps, format: Service → Section → Action
5. Optional         — 1 extra thing, labelled "Optional"
```

### Length calibration by level:

Always include this if the skill adapts to user level:

```markdown
Adjust depth to level:
- Beginner: plain English, no jargon, explain every step
- Intermediate: include key concepts, assume basic familiarity
- Experienced: concise, assume strong baseline, skip basics
```

---

## Part 7 — Reference Tables

Internal decision guides that tell the AI how to choose without asking
the user every time. These reduce questions and improve output quality.

### When to use a reference table:

- Service selection (e.g. which AWS service for which need)
- Email type → what to lead with / end with
- Tone → recipient type → language register
- Error type → most likely cause

### Format:

```markdown
## [Topic] Reference (internal — use to make decisions)

| Condition | Primary choice | Notes |
|---|---|---|
| Need X    | Use Y         | Only use Z if... |
```

### Why "internal" matters:

Labelling a table as "internal — use to make decisions" tells the AI this
is for reasoning, not for showing to the user. Without this label, the AI
may print the table in its response when it shouldn't.

---

## Part 8 — Tone & Format

This section governs how the skill writes, not what it writes.
Without it, the AI defaults to chatbot style — verbose, padded, generic.

### Always include:

```markdown
## Tone & Format

- Write like a [role] explaining to a [audience] — not a textbook, not a chatbot.
- Short sentences. Scannable structure.
- No bold walls of text. Use headers and numbered lists for steps only.
- Never pad answers. If it fits in two lines, use two lines.
- Never say "Great question!" or similar filler.
```

### Banned phrases — include a list for any writing skill:

A list of specific phrases the skill must never use is one of the most
effective formatting constraints available. It is more reliable than
saying "be concise" or "avoid filler".

```markdown
## Banned Phrases (never use)
- "I hope this email finds you well"
- "Great question!"
- "As an AI language model..."
- "Certainly!"
- "Absolutely!"
```

### The "flag but don't fix" rule:

Include this in any skill where the user provides existing content
(code, email, architecture, config):

```markdown
If the user's [content] has a [problem type] you weren't asked about
but it matters, flag it in one line only — don't fix it unless asked.
```

This keeps the skill focused and prevents scope creep.

---

## Anti-Hallucination Checklist

Use this when writing or reviewing any skill. Every item closed = one less
way the AI can produce bad output.

```
[ ] Persona is specific — role, seniority, domain, communication style
[ ] 90% confidence rule is included
[ ] Rules section exists with "never break these" label
[ ] "Never give multiple options" rule is included
[ ] Session start asks level and intent before acting
[ ] All questions asked in ONE message, not spread across turns
[ ] Each flow has a clarify-before-act step
[ ] Output structure is deterministic — fixed order, fixed sections
[ ] Length constraints are specified per section
[ ] Reference tables are labelled "internal"
[ ] Tone & format section exists
[ ] "Flag but don't fix" rule included if user provides existing content
[ ] Banned phrases listed if skill involves writing
[ ] Optional step is always exactly one, always labelled "Optional"
[ ] Skill does not ask for information it can infer
[ ] Skill does not produce output before it has enough context
```

---

## Common Mistakes and How to Fix Them

| Mistake | Why it fails | Fix |
|---|---|---|
| Vague persona ("you are an expert") | AI drifts to generic | Add role, years, domain, style |
| No confidence rule | AI speculates freely | Add "90% rule" explicitly |
| Multiple options given | Decision transferred back to user | "ONE primary path" rule |
| Questions spread across turns | Friction, context lost | All questions in ONE message |
| No output structure | Output varies every time | Define fixed section order |
| "Be concise" as a rule | Too vague to enforce | Add specific length per section |
| No "flag but don't fix" rule | AI rewrites things not asked | Add it explicitly |
| Missing flow for troubleshooting | AI handles issues like knowledge | Separate flow with clarify step |
| No banned phrases list | Writing sounds like a chatbot | List specific phrases to avoid |
| Too many rules (10+) | None of them stick | Pick the 6–8 most critical |

---

## Skill File Template (copy and fill)

```markdown
---
name: your-skill-name
description: >
  [What this skill does. When to use it. Explicit trigger words.]
---

# [Skill Title]

You are a [role] with [X]+ years of experience in [domain].
You think like [reference] — [style in 3–5 words].
You only say things you are more than 90% sure about.
If you are not sure, you say so and stop there.

---

## Rules (never break these)

- Never give multiple options — give the ONE primary path.
- Never suggest changes the user didn't ask about.
- Never give false positives. If unclear, ask.
- Keep answers simple and straight to the point.
- Confidence must be above 90% before stating anything as fact.
- Always offer ONE optional step at the end, labelled "Optional".
- [Domain-specific rule 1]
- [Domain-specific rule 2]

---

## Session Start — Ask Every New Session

[Ask all intake questions in ONE message before doing anything.]

---

## Flow A — [Mode 1: e.g. Info / Knowledge]

### Step 1 — Clarify if needed
[When to ask, what to ask, how many questions]

### Step 2 — [Output section name]
[What to produce, format, length constraint]

### Step 3 — [Output section name]
[What to produce, format, length constraint]

### Optional
[One extra thing. Label it "Optional".]

---

## Flow B — [Mode 2: e.g. Generate / Create]

[Same structure as Flow A]

---

## Flow C — [Mode 3: e.g. Issue / Troubleshoot]

[Same structure as Flow A]

---

## [Domain] Reference (internal — use to make decisions)

| Condition | Primary choice | Notes |
|---|---|---|
| [condition] | [choice] | [when to deviate] |

---

## Tone & Format

- Write like a [role] explaining to a [audience].
- Short sentences. Scannable structure.
- No bold walls of text.
- Never pad answers.
- Never say "Great question!" or filler.
- If the user's content has a problem you weren't asked about,
  flag it in one line only — don't fix it unless asked.
```

---

## Summary — The 8 Things That Make a Skill Work

```
1. Specific persona        — who the AI is, how it thinks
2. 90% confidence rule     — anti-hallucination anchor
3. Hard rules              — what it must never do
4. Single-message intake   — all questions upfront, in one go
5. Clarify-before-act      — never guess, always confirm
6. Deterministic output    — fixed sections, fixed order, every time
7. Internal reference      — decision tables the AI uses, not shows
8. Tone constraints        — banned phrases, length limits, format rules
```

These eight things, applied consistently, produce skills that behave the
same way every session, give confident and accurate output, and know
exactly when to ask and when to act.
