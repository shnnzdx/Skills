# Agent Skills — Evidence-First Workflows

A collection of reusable agent instructions for **GitHub engineering, Grok multi-agent delivery, resumes, and interviews**.

这不是只用于写简历的仓库。这里的 Skills 按任务拆分，原则是：**证据优先、适度自主、独立 QA、关键决策升级**。Skill 是工作指引，**不是 GitHub/AWS 执行权限**。

## Skills / 使用入口

| Skill | Canonical entry | Use it for |
| --- | --- | --- |
| **GitHub Evidence Harness** | [`skills/github-evidence-harness/SKILL.md`](skills/github-evidence-harness/SKILL.md) | Exact SHA/diff/CI checks, bounded QA, L0–L3 risk review and G0–G5 permissions |
| **Grok Multi-Agent Orchestration** | [`skills/grok-multi-agent-orchestration/SKILL.md`](skills/grok-multi-agent-orchestration/SKILL.md) | DD coordinates FE/BE/AI/Cloud/QA/Docs and Codex/Cursor; fewer repetitive ChatGPT reviews |
| **Evidence-First Resume / 结果导向简历** | [`skills/evidence-first-resume/SKILL.md`](skills/evidence-first-resume/SKILL.md) | Role-agnostic, bilingual, evidence-backed, concise resume editing |
| **Evidence-First Resume — English** | [`skills/evidence-first-resume/SKILL.en.md`](skills/evidence-first-resume/SKILL.en.md) | English-only resume guidance |
| **S&B Interview / 情景与行为面试** | [`skills/evidence-first-sb-interview/SKILL.md`](skills/evidence-first-sb-interview/SKILL.md) | Behavioral STAR, situational decisions, independent evidence and bilingual interview practice |

Additional interview assets: [English-only guide](skills/evidence-first-sb-interview/SKILL.en.md), [story-bank template](skills/evidence-first-sb-interview/STORY_BANK_TEMPLATE.md), and [interview README](skills/evidence-first-sb-interview/README.md).

## How to use / 如何启动

**Engineering:** Load GitHub Evidence Harness + Grok Multi-Agent Orchestration. Give Grok DD a repository, task goal, explicit limits, and any **already granted** operations. Grok verifies the exact GitHub state, delegates work and QA, then closes L0–L2 tasks itself within that authorization. ChatGPT reviews material L3 architecture/security issues, rather than every PR.

**Resume:** Load the standalone Evidence-First Resume skill. Supply your source resume/projects, desired language, and constraints. It preserves ownership, metrics and implementation status; it does not fabricate impact or force a particular job family.

**Interview:** Load the S&B Interview skill. Supply verified examples and the target question/role; do not treat a polished resume bullet as proof of a longer story.

## Review tiers — 简化审查

| Tier | Typical work | Review ownership |
| --- | --- | --- |
| L0 | Docs/index/read-only | Grok DD |
| L1 | Isolated low-risk code/tests | Grok DD + QA/CI |
| L2 | Cross-component feature/integration | Grok DD + independent QA |
| L3 | Authentication, privacy, cross-tenant access, data integrity, serious production risk | Human Owner + specialist QA; ChatGPT for material architecture/security review |

These tiers determine **who reviews**, not **who can click Merge**. G0–G5 gate external effects; no permission is created by this README. A repo-specific standing authorization can delegate routine actions after careful path/workflow review. A PR merge that triggers ECR publication requires authorization for the combined impact; ECS production deployment remains a separate owner decision.

## Existing resume entry points / 旧链接兼容

- [`RESUME_SIMPLE.md`](RESUME_SIMPLE.md): retained bilingual legacy snapshot. Canonical updates now belong in `skills/evidence-first-resume/SKILL.md`.
- [`RESUME_SIMPLEen.md`](RESUME_SIMPLEen.md): retained English legacy snapshot. Canonical updates now belong in `skills/evidence-first-resume/SKILL.en.md`.

These root files are intentionally kept so older links continue to work. Avoid editing both copies independently; keep the canonical `skills/` versions as the maintained source. The previous README linked root `SKILL.md` / `SKILL.en.md` that did not exist; those links have been corrected.

## Safety and scope

- Read repository facts from GitHub at pinned SHAs; distinguish `STATIC-VERIFIED`, `EXECUTED-PASS`, `REPORTED`, `NOT RUN`, `UNKNOWN` and `OPEN`.
- The engineering skills are reusable across repositories. Cadensy / DESIGN-04 is an example, **not** a global automatic merge policy or a retroactive change to existing G3/G4/G5 restrictions.
- This public repo contains general instructions, not secrets, tokens, private resumes, personal story banks or production credentials.

**Default:** the shortest honest evidence-backed report wins over long speculative architecture reviews.
