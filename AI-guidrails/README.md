# AI Guardrails

This folder contains reusable AI steering files for engineering standards, secure coding, and architecture reviews. These are meant to be used as repo-level guardrails or copied into a tool-specific configuration such as Kiro steering files.

## Included guardrails

- [global-engineering-standards.md](global-engineering-standards.md) — Base engineering standards for every engineering session and project.
- [secure-coding-rules.md](secure-coding-rules.md) — Security-first rules covering input validation, secrets handling, auth, encryption, and safe coding patterns.
- [aws-well-architected-review-steering.md](aws-well-architected-review-steering.md) — Manual-use review guide for conducting an AWS Well-Architected assessment against repository evidence.

## Typical usage

- Keep the files in this folder for version control and reuse.
- Copy a file into a tool-specific configuration directory when the platform expects `inclusion: always` or `manual` steering content.
- Prefer the narrowest applicable guardrail rather than enabling everything at once.
