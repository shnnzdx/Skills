---
name: grok-multi-agent-orchestration
description: "Lean Grok-led personal engineering team. DD coordinates only the specialists needed; routine work is self-checked, important work gets targeted independent QA, and ChatGPT is reserved for consequential security or architecture questions."
---

# Grok Multi-Agent Orchestration — Personal Team Lite / 个人团队协作

Start with `../github-evidence-harness/SKILL.md`. This is an assignment and quality guide, **not a grant of GitHub or AWS permissions**.

**Team principle:** We are a small owner-led engineering team, not a large enterprise. **Ship a correct increment with minimal ceremony.** Don't create approval chains, review packets, or giant docs merely to look rigorous.

## 1. Who does what / 责任明确但不官僚

- **Human Owner:** product direction, meaningful trade-offs, standing delegation scope, expensive/production/destructive actions. Should not be interrupted for routine internal bookkeeping.
- **Grok DD (orchestrator):** understand the goal, classify risk (Quick/Standard/Critical), choose workers, track actual authorization, make routine decisions, and give one short status update.
- **Frontend / Backend / AI Agent / Cloud:** implement or research **only when relevant**. A UI issue doesn't need the Cloud worker unless a release effect matters.
- **QA:** independently challenge risky integration and core regressions. **Not mandatory for every README edit or simple helper.**
- **Docs:** update useful source-of-truth pages when needed, not after every code change.
- **Codex / Cursor:** bounded implementation and tests; never invent QA results or approve their own risky release.
- **ChatGPT:** independent second opinion only for **material new security/architecture decisions, contested evidence, or explicit Owner request**. Not every PR's gatekeeper.

Grok DD owns coordination. Avoid parallel writes to the same file; one implementer plus a reviewer when it matters.

## 2. Three-track delivery / 三条路径，不逐轮拉评审

| Work type | Actual loop | Who decides completion |
| --- | --- | --- |
| **Quick / 日常** | DD → relevant worker → inspect diff + small check → done | Grok DD, within previously approved action scope |
| **Standard / 重要** | DD → worker → focused independent QA + relevant CI → done | Grok DD, within authorized scope |
| **Critical / 高风险** | DD → evidence → Human Owner / optional ChatGPT critical challenge → focused implementation and QA | Owner controls consequential external actions |

Examples: change README links = Quick; implement a cross-page UI flow = Standard; introduce a new auth/tenant permission model or destructive production migration = Critical.

Risk is based on the change, not old PR labels or quantity of documents. An unchanged critical decision does not need reapproval simply because a new commit updated documentation.

## 3. Default autonomy / 默认减少打断

Grok should automatically handle investigation, task breakdown, worker assignment, local checks and concise evidence reporting **within its existing permissions**. The Owner can explicitly create a **standing GitHub delegation** covering routine branch work or safe merges; once granted, no per-file/per-PR permission ping is needed inside that scope.

Do not silently assume the standing policy already exists. Current project-specific gates remain effective until the Owner changes them.

For approvals not already covered, present **one question** stating task + effect + expected automatic side effects. Do not ask separate ceremonial approvals for G1, G2, G3, G4 when the Owner can give a single clearly scoped approval. Production actions stay separately Owner-controlled.

## 4. Practical anti-hallucination checks / 防幻觉但不繁琐

- **When reviewing a PR:** read its real changed files and current head; quote code or logs when material. Do not use an agent-written summary as proof.
- **When reporting tests:** say what was actually run, and whether new tests are collected by CI. A green PR badge without logs is metadata evidence, not proof of individual test names.
- **When uncertain:** write VERIFIED / REPORTED / NOT RUN / UNKNOWN. Don't fabricate missing counts or use invented URLs.
- **For Quick work:** agent's own diff check and appropriate test may be enough. No forced separate QA worker.
- **For Standard work:** one independent QA pass focusing on the affected behavior, not an exhaustive repository audit.
- **For Critical work:** independently inspect trust boundaries, failure modes, real deployment effects and rollback; get Owner input where needed.

A QA issue is a **small patch request**, not permission to redesign the project. Keep a bounded PASS with explicit untested cases if the remaining gap is non-critical.

## 5. Stop and ask only for real impact / 只为重要风险升级

Escalate to Owner when a change is outside approved permissions, has real production/data/secret effects, or the required test fails. Involve ChatGPT when **the critical architecture/security question itself is unresolved**, not for every subsequent SHA, rerun, docs-only main sync, or minor UI follow-up.

Avoid: repetitive design reviews, mandatory six-bot ceremonies, "QA reviews QA" cycles, formal risk matrices for CSS, serial G3/G4 conversations, and giant handoff documents.

Do not waive: truthful evidence, secret safety, server-side authorization, meaningful negative tests for security behavior, accurate production status, and actual Owner permission for consequential operations.

## 6. A single short DD update / 每次完成最多几行

```text
Completed: <what changed>
Proof: <real test or relevant CI, or NOT RUN>
Concern: <only material risk, otherwise "none">
GitHub/production: <PR or merge state and any automatic effects>
Next: <done / one concise approval question / targeted fix>
```

For Critical work only, attach a deeper technical decision memo if it materially helps. No independent ChatGPT review needed merely to close Standard tasks.
