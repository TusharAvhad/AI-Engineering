# AI Engineering — Prompt Library

A curated collection of production-grade prompts (AI "skills") for software
engineering, cloud architecture, and technical communication. Each prompt is a
reusable **behaviour contract**: load it once, invoke it many times, and get
consistent, high-quality output every session.

This repo doubles as a worked demonstration of how to write prompts that are
specific, deterministic, and resistant to hallucination. Start with
[prompt-engineering-basics.md](prompt-engineering-basics.md), then browse the
catalog below.

> **Author:** Tushar Avhad · **GitHub:** [TusharAvhad](https://github.com/TusharAvhad?tab=repositories) · **LinkedIn:** [tushar-b-avhad](https://www.linkedin.com/in/tushar-b-avhad/)

---

## Why these prompts work

Every prompt here follows the same discipline:

- **A specific persona** — a senior practitioner with a defined style, not a
  generic "expert."
- **A 90% confidence rule** — the AI says what it's sure of and stops where it
  isn't, instead of guessing.
- **Hard rules** — short, enforceable constraints ordered by severity.
- **One primary path** — a single clear recommendation plus, at most, one
  labelled "Optional" variation. No menus.
- **Deterministic output** — fixed sections in a fixed order, so you can predict
  exactly what you'll get.

The full method is written up in
[meta/how-to-write-a-skill.md](meta/how-to-write-a-skill.md).

---

## Catalog

### Prompt engineering (start here)
| Prompt | What it does |
|---|---|
| [prompt-engineering-basics.md](prompt-engineering-basics.md) | A short primer: the four parts of a good prompt, and the jump from one-off prompt to reusable skill. |
| [meta/how-to-write-a-skill.md](meta/how-to-write-a-skill.md) | The complete method for writing an effective skill, with templates and an anti-hallucination checklist. |
| [meta/response-efficiency.md](meta/response-efficiency.md) | A global ruleset that makes any AI answer accurately, concisely, and without false positives. |

### Codebase design
| Prompt | What it does |
|---|---|
| [codebase-design/codebase-design.md](codebase-design/codebase-design.md) | Shared vocabulary for designing **deep modules** — a lot of behaviour behind a small interface. |
| [codebase-design/deepening.md](codebase-design/deepening.md) | How to safely deepen shallow modules based on their dependency category. |
| [codebase-design/design-it-twice.md](codebase-design/design-it-twice.md) | A parallel sub-agent pattern for exploring several radically different interfaces, then comparing them. |
| [codebase-design/code-review.md](codebase-design/code-review.md) | Review a diff or PR for correctness, security, design, and maintainability with severity-ranked findings. |
| [codebase-design/debug-root-cause.md](codebase-design/debug-root-cause.md) | Systematically reproduce, isolate, root-cause, and minimally fix a bug — no guessing. |
| [codebase-design/write-tests.md](codebase-design/write-tests.md) | Write or improve tests that cover behaviour through the interface, survive refactors, and catch real bugs. |

### Planning & workflow
| Prompt | What it does |
|---|---|
| [planning-and-workflow/grill-me.md](planning-and-workflow/grill-me.md) | Stress-test a plan or design by getting interviewed relentlessly until every decision is resolved. |
| [planning-and-workflow/conversation-to-prd.md](planning-and-workflow/conversation-to-prd.md) | Turn an existing conversation into a full PRD — synthesis, no interview. |
| [planning-and-workflow/create-readme.md](planning-and-workflow/create-readme.md) | Generate a comprehensive, scannable project README from the codebase. |
| [planning-and-workflow/commit-and-pr.md](planning-and-workflow/commit-and-pr.md) | Write clear Conventional Commit messages and structured PR descriptions from a diff. |

### Architecture & cloud
| Prompt | What it does |
|---|---|
| [architecture-and-cloud/aws-architecture.md](architecture-and-cloud/aws-architecture.md) | Design a cost-effective, secure, scalable AWS architecture with C4 diagrams and draw.io XML. |
| [architecture-and-cloud/aws-guide.md](architecture-and-cloud/aws-guide.md) | A senior AWS architect for any service question or troubleshooting, console-first. |
| [architecture-and-cloud/terraform-aws.md](architecture-and-cloud/terraform-aws.md) | A senior Terraform + AWS expert for concepts, code generation, and debugging. |
| [architecture-and-cloud/terraform-tests.md](architecture-and-cloud/terraform-tests.md) | Write comprehensive Terraform module tests — variable validation, plan assertions, security constraints, per-module `tests/` folders. |
| [architecture-and-cloud/github-actions.md](architecture-and-cloud/github-actions.md) | A senior GitHub Actions + CI/CD expert, with OIDC-first AWS deployment patterns. |

### Writing & communication
| Prompt | What it does |
|---|---|
| [writing-and-communication/technical-email.md](writing-and-communication/technical-email.md) | Draft or rewrite professional technical emails — no filler, one best version. |
| [writing-and-communication/it-resume-writer.md](writing-and-communication/it-resume-writer.md) | Transform experience into a polished, ATS-friendly IT resume using the X-Y-Z formula. |

---

## How to install and use these prompts

Each file is plain Markdown. The content is portable across tools — only the
*installation location* differs. Below are the steps for the four most common
AI coding assistants.

### GitHub Copilot (VS Code)

Copilot reads repository and personal custom instructions.

1. **Repo-wide instructions:** create `.github/copilot-instructions.md` and paste
   the prompt body you want always active (e.g. the contents of
   `meta/response-efficiency.md`).
2. **Reusable prompt files:** create a `.github/prompts/` folder, add a file like
   `aws-guide.prompt.md`, and paste a prompt into it. Invoke it in Copilot Chat
   with `/aws-guide`.
3. Enable prompt files via the setting `chat.promptFiles` if your version
   requires it.

### Claude (claude.ai, Claude Code, or Projects)

1. **One-off:** paste the full prompt file as the first message in a new chat,
   then send your actual request as the next message.
2. **Projects (claude.ai):** create a Project and paste the prompt into the
   **Custom Instructions** box so it applies to every chat in that Project.
3. **Claude Code:** drop the prompt into a `CLAUDE.md` at your repo root (always
   loaded), or into `.claude/commands/<name>.md` to invoke it as `/<name>`.

### Amazon Kiro

Kiro uses **steering files** and **skills** inside a `.kiro/` folder.

1. **Steering (always or conditionally applied):** copy a prompt into
   `.kiro/steering/<name>.md`. Front-matter controls when it loads
   (`inclusion: always`, `fileMatch`, `manual`, or `auto`) — see
   `meta/response-efficiency.md`, which already uses `inclusion: always`.
2. **Skills (invoked on demand):** copy a prompt into
   `.kiro/skills/<name>/SKILL.md`. The front-matter `name` and `description`
   drive when Kiro activates it — the prompts here already include these.
3. Reload the window (or start a new session) so Kiro picks up the new files.

### Google Gemini

1. **Gemini app / AI Studio:** paste the prompt as a **System Instruction** (AI
   Studio has a dedicated field), then send your request as the user turn.
2. **Gemini Code Assist (VS Code / JetBrains):** add a `.gemini/styleguide.md` or
   paste the prompt into your chat context at the start of a session.
3. **One-off:** paste the full prompt, then your request, as two consecutive
   messages in any Gemini chat.

> **General rule for any tool:** if there's a "system instruction" or "custom
> instruction" field, put the prompt there. If there isn't, paste the prompt as
> the first message, then your request as the second.

---

## Clone

```bash
git clone https://github.com/TusharAvhad/AI-Engineering.git
cd AI-Engineering
```

Browse the folders, copy the prompt you need, and drop it into your tool using
the steps above.

---

## License

Released under the [MIT License](LICENSE) — free to use, copy, modify, and share.
