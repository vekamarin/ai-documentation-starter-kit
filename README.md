# Context Starter Kit: The Documentation Your AI Actually Needs

*Handout from **Claude Community Wrocław #5** — Veronica Marin Kramer ([linkedin.com/in/vekamarin](https://www.linkedin.com/in/vekamarin))*

> **Slide Deck:** [`presentation.html`](./presentation.html) *(also viewable in the browser via [GitHub Pages](https://vekamarin.github.io/ai-documentation-starter-kit/))*

You don't need to write 11,000 lines of specifications by hand before your next feature. The AI can draft every one of these documents—your job is to review, correct, and approve them so the decisions are yours.

---

## 1. Pick Where to Start

If you have an existing project, start with **Lessons Learned** and **ADRs**. If you are starting a new repository, start with **`CLAUDE.md`** and **Architecture / Data Model**.

| Document | Location Example | What It Answers | How AI Drafts the First Version |
|---|---|---|---|
| **Instructions (`CLAUDE.md`)** | `CLAUDE.md` | Build commands, coding conventions, boundaries, and pointers to your specs | Run `/init` in Claude Code, then trim it down to principles and links |
| **Lessons Learned (Forensics)** | `docs/context/forensics.md` | What already went wrong in the repo, measured, and the rule that prevents it | Ask AI to audit the repo for duplication, monoliths, and mock data; you confirm the rules |
| **Architecture Decisions (ADRs)** | `docs/decisions/adr-001.md` | Key technical choices, alternatives considered, and *why* you chose this path | Talk through a decision out loud or paste a meeting transcript; AI formats the ADR |
| **Data Model & Boundaries** | `docs/context/data-model.md` | What core entities mean, how they relate, and how tenant/user data is isolated | Ask AI to document your existing database schema and flag ambiguities |
| **Capability Specs** | `docs/specs/capabilities/*.md` | What a capability does, accepts, returns, and how it fails | Give a 5-bullet outline; ask AI to expand it using a standard template and ask clarifying questions |
| **Skills & Path Rules** | `.claude/skills/*/SKILL.md` `.claude/rules/*.md` | *How* you work (migrations, seed data, UI components, testing, git workflow) | When you catch yourself repeating an instruction in chat, ask AI to turn it into a short skill |

---

## 2. How to Organize Context (Don't Dump Everything Into One File)

Keep your root `CLAUDE.md` short so every session starts fast and focused:

1. **Always loaded (`CLAUDE.md` + `@imports`):** High-level principles, commands, and links to where specs live.
2. **Loaded by file path (`.claude/rules/*.md` with `paths:`):** Rules that only matter when touching specific files (for example, SQL migrations or API routes).
3. **Loaded by folder (`src/<module>/CLAUDE.md`):** Module-specific context loaded only when working in that directory.
4. **Loaded on match (`.claude/skills/<name>/SKILL.md`):** Task workflows (testing, seeding, deployment). Only the skill's `name` and `description` stay in context until a task matches it.

> **Rule of thumb:** Specs say **WHAT** to build and **WHY**. Skills say **HOW** to work. A skill should *link* to the spec or ADR (`See docs/decisions/adr-020.md`), never copy-paste it—copies drift apart.

---

## 3. Copy-Paste Starter Prompts

### Audit an existing repo (Forensics → Guardrails)
```text
Scan this repository and write a concise forensic report in docs/context/forensics.md.
Measure:
1. The 10 largest source files by line count and where component/service responsibilities are mixed.
2. Any hardcoded mock/fallback data in frontend or API code.
3. Duplicated helpers or inconsistent patterns across modules.
For each finding, propose one concrete guardrail rule we can enforce going forward.
```

### Turn a discussion into an ADR
```text
Here are our notes from today's architecture discussion:
[paste notes or voice transcript]

Draft an Architecture Decision Record in docs/decisions/adr-XXX-<slug>.md with:
- Status: Draft (awaiting human review)
- Context & problem statement
- Options considered and trade-offs
- Decision and consequences (what becomes easier, what becomes harder, and what rule the AI must follow)
Ask me clarifying questions before finalizing if any trade-off is ambiguous.
```

### Draft a Capability Spec before coding
```text
We are planning a new capability: [brief description].
Before writing any code, read docs/context/ and our existing ADRs.
1. List the hidden decisions or ambiguities you would otherwise have to guess (data source, access control, error handling, edge cases).
2. Ask me those questions first.
3. Once I answer, draft the specification in docs/specs/capabilities/<name>.md.
```
