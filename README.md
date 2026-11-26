# Agent Skills — Personal Team Workflows

Reusable skills for a small AI-assisted engineering team, plus evidence-first resumes and interviews.

**默认：少流程、重结果、证据真实。** Grok DD 负责日常研发与决策协调；不是每个 PR 都需要独立 QA、ChatGPT 复审或多级审批。只有有实际风险的工作才加审查强度。

## Skills

| Skill | Entry | Purpose |
| --- | --- | --- |
| **GitHub Evidence Harness — Lite** | [`skills/github-evidence-harness/SKILL.md`](skills/github-evidence-harness/SKILL.md) | Quick / Standard / Critical review, real GitHub evidence, simple action boundaries |
| **Grok Multi-Agent Orchestration — Lite** | [`skills/grok-multi-agent-orchestration/SKILL.md`](skills/grok-multi-agent-orchestration/SKILL.md) | Grok DD, FE/BE/AI/Cloud, QA, Docs, Codex/Cursor with minimal ceremony |
| **Evidence-First Resume / 结果导向简历** | [`skills/evidence-first-resume/SKILL.md`](skills/evidence-first-resume/SKILL.md) | Bilingual, role-agnostic, evidence-backed resume writing |
| **Resume — English** | [`skills/evidence-first-resume/SKILL.en.md`](skills/evidence-first-resume/SKILL.en.md) | English guide |
| **S&B Interview / 情景与行为面试** | [`skills/evidence-first-sb-interview/SKILL.md`](skills/evidence-first-sb-interview/SKILL.md) | Verified behavioral and situational interview preparation |
| **Algorithm Interview Coach / 算法刷题教练** | [`skills/algorithm-interview-coach/SKILL.md`](skills/algorithm-interview-coach/SKILL.md) | NeetCode/LeetCode: staged explanation, Python basics, comprehension checks, independent coding, hint-first reviews, optional Notion archive |

[Interview English guide](skills/evidence-first-sb-interview/SKILL.en.md) · [Story bank template](skills/evidence-first-sb-interview/STORY_BANK_TEMPLATE.md) · [Interview README](skills/evidence-first-sb-interview/README.md)

## Engineering — three levels / 个人项目的审查强度

| Level | Typical work | Default workflow |
| --- | --- | --- |
| **Quick** | Docs, small CSS/copy, isolated safe tests | Worker checks diff and relevant test → Grok DD closes; **no mandatory QA** |
| **Standard** | Feature, cross-file state/API integration | Worker → **one targeted independent QA** → Grok DD closes |
| **Critical** | Auth, privacy, cross-tenant isolation, destructive data or risky production changes | Human Owner + targeted QA; **ChatGPT only for material critical architecture/security questions** |

Only use the checks needed for this change. Report real evidence, not a stack of artifacts. Don't request repeated reviews for unchanged executable code after a docs-only branch sync.

**Permissions are separate from review.** The Owner can grant a scoped standing delegation for routine GitHub changes, which removes repeated asks; this README does not itself grant it. A merge might auto-publish an image, so request approval for the combined effect **once** if not already delegated. Production/deletion/secrets require explicit Owner permission. Existing Cadensy restrictions remain until explicitly changed.

## How to use / 使用方法

**Grok:** Load the GitHub Harness and Multi-Agent Skill. Provide repo, goal and any previously approved action scope. DD assigns only needed workers, uses the lowest appropriate review level, and gives a **four-to-five-line status**.

**Resume:** Load the standalone [bilingual Resume Skill](skills/evidence-first-resume/SKILL.md) or [English guide](skills/evidence-first-resume/SKILL.en.md) with the candidate's evidence and requested language/length. The former root-level duplicates were removed; these `skills/evidence-first-resume/` files are the canonical sources.

**Interviews:** Load the S&B Skill; keep fact, inference and hypothetical answers distinct.

**Algorithm practice:** Load the Algorithm Interview Coach Skill; teach one method at a time and review independent code with hints before showing a full correction.

Algorithm teaching examples: [Valid Sudoku complete walkthrough](skills/algorithm-interview-coach/examples/VALID_SUDOKU_FULL_WALKTHROUGH.md) · [Top K Frequent Elements complete walkthrough](skills/algorithm-interview-coach/examples/TOP_K_FREQUENT_ELEMENTS_FULL_WALKTHROUGH.md) · [Reusable session template](skills/algorithm-interview-coach/templates/PROBLEM_SESSION_TEMPLATE.md).

## Boundaries

This public repo contains no secrets, tokens or personal resumes. These skills do not alter any GitHub protection rule, AWS setting, existing authorization, or historical acceptance result.

**Default operating principle:** Quick work moves quickly. Important work gets relevant QA. Critical work gets deliberate judgment—not a bigger bureaucracy.
