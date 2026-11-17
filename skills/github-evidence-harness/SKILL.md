---
name: github-evidence-harness
description: "Lightweight, evidence-first GitHub workflow for a personal Grok-led engineering team. Reduce approval and review overhead: self-check routine changes, use independent QA for meaningful integration risks, and escalate only high-impact security, data or production decisions."
---

# GitHub Evidence Harness — Personal Team Lite / 个人团队轻量版

**Use for:** GitHub development, PR review, CI evidence and release decisions in a small owner-led AI team.

**Goal:** Move quickly without inventing evidence. **Do not copy big-company approval procedures into everyday personal-project work.** A short, verified result is better than repeated 20-page reviews.

This skill is **guidance, not GitHub/AWS authorization**. It does not grant a bot any action permission or change pre-existing project restrictions.

## 1. Default: choose the lightest review / 默认用最轻的流程

| Level | Typical changes | Review / completion | ChatGPT |
| --- | --- | --- | --- |
| **Quick / 日常** | README, links, docs, small CSS/copy changes, isolated low-risk unit tests, simple refactors without contract changes | Implementer checks diff and relevant test; Grok DD accepts. **No mandatory separate QA or formal design document.** | No |
| **Standard / 重要** | Cross-file UI/state changes, bounded new features, API behavior not affecting authentication, multi-module integrations | Worker implements → independent QA verifies **affected behavior and key regressions once** → DD decides. | No by default |
| **Critical / 高风险** | Authentication/permissions, cross-trip/tenant isolation, privacy/secrets, destructive migrations, irreversible data changes, serious security findings, production deployment or expensive cloud changes | Owner decides risk and release; targeted independent QA and rollback evidence. ChatGPT can challenge **material new** architecture/security decisions. | Only for material critical issues or owner request |

**Risk is about consequences**, not number of lines or number of PR commits. Grok DD can reclassify work when new evidence changes the risk. A trivial docs-only sync does not become Critical just because its branch has an old Critical review history.

### Do not confuse three separate questions

1. **Is the change correct?** Appropriate review/tests for its risk level.
2. **Is the change authorized?** Use the permission scope the Owner already granted.
3. **What happens after merge?** A merge may automatically publish ECR/package artifacts even if no ECS deployment occurs.

Don't turn these three questions into three repeated conversations. Put any needed approval in **one concise Owner request**, including automatic downstream effects.

## 2. Evidence without paperwork / 不制造形式主义

For Quick tasks, the minimum is:

- Name the changed files and describe what changed.
- Actually inspect the diff and run the relevant check **if one exists**. State `NOT RUN` when no test was run; don't invent coverage.
- Make a short final report. No mandatory QA bot, multi-agent meeting, SHA matrix, acceptance spreadsheet, archival bundle, or new design revision.

For Standard tasks:

- At PR/release review, check the **actual PR head** and changed files; run targeted tests and required CI checks.
- Use an **independent QA worker once** to check the risky behavior and main negative case. Small unrelated edits do not restart full QA.
- If a new test was meant to run in CI, confirm that the CI command collects it; a green badge alone isn't proof.
- If tests fail, fix the specific defect, rerun the relevant tests and close. Don't create cascading "rev5.1 / rev5.2 / rev5.3" approval cycles.

For Critical tasks:

- Pin exact base/head SHAs; inspect auth/data boundary and real code, test the material negative cases, assess rollback, state missing E2E or production evidence.
- Involve ChatGPT **once per genuinely new disputed architectural/security decision** when needed; don't force it to reapprove code that was proven unchanged.
- A new head from docs-only main sync does **not** require a new critical architecture review if executable blobs are unchanged; record that fact and relevant CI.

### Four honest labels suffice

**VERIFIED:** personally read source/diff/log or ran the check.  
**REPORTED:** another agent's claim not independently reproduced.  
**NOT RUN:** test or operation was not executed.  
**UNKNOWN:** missing/inaccessible evidence.

Use exact commit SHAs on a PR or release summary; there's no need to print SHAs in every design chat or small status ping. For published test counts, use a run log or reproducible local result, not guesswork. If CI logs are 403, write `UNKNOWN` for the transcript; don't repeatedly block routine work when verified local checks and the Owner's policy allow proceeding. If exact CI test-name proof is an acceptance criterion for that task, obtain it or explicitly note that criterion remains unmet.

**Never equate:** code written with merged; CI green with all tests executed; ECR built with ECS deployed; static grep with browser E2E; proposal with implemented production behavior.

## 3. Permissions: simple in daily conversation / 日常不逐项报 G0–G5

The **Owner may grant standing delegation** for a named repository and path/workflow class. Under such a grant, Grok DD can handle routine planning, local edits, tests, branches, PRs and permitted merges **without asking again for every file, SHA or small PR**. Verify the scope before acting.

Without a grant, do not assume permission to push, merge or change infrastructure merely because this skill says "Quick". A skill is never its own authorization.

Use three practical boundaries in everyday reports:

- **Work:** read, edit, run local checks and prepare a branch, within the established task/agent authorization.
- **Publish:** push/PR/merge and **any automatically triggered image or package publication**. Ask once for the combined effect if not covered by standing policy.
- **Production:** deployment, production flag/secret changes, database migrations, destructive operations, or real cloud expenditure. Explicit Owner authorization, regardless of test results.

**Compatibility with prior Cadensy gates:** G0=read; G1=local implementation; G2=push/PR; G3=merge; G4=artifact publication; G5=production. They remain enforceable for work already restricted. **This update neither retroactively grants G3/G4/G5 nor reopens blocked PRs.** A future Owner decision can deliberately replace the old per-task policy with a precise standing delegation; record that scope once.

### Optional standing-delegation template (NOT automatically approved)

```text
Repo: <owner/repo>
Grok may autonomously: <local changes, push/PR, and/or merge>
Allowed scope: <safe paths/branches/change classes>
Required checks: <relevant tests / named CI>
Downstream effects explicitly covered: <e.g. preview/ECR build only, or none>
Excluded: <auth, privacy, secrets, DB migration, production changes>
Stop and ask the Owner when: <...>
Expires/review date (optional): <...>
```

Do not fill this with imagined approval. A standing grant covers only what the Owner actually chose.

## 4. Team loop / 不必每轮都拉齐所有机器人

**Quick:** DD assigns one worker → worker checks its change → DD closes.  
**Standard:** DD assigns worker → QA checks affected behavior → DD closes.  
**Critical:** DD gathers focused evidence → Owner/optional ChatGPT resolves the critical decision → scoped implementation and QA.

Do not summon Frontend, Backend, Cloud, QA, Docs and ChatGPT for a tiny change. Documentation is optional unless it answers a future operational question.

When an issue is found, return **one targeted fix request**. Prefer preserving prior validated results for unchanged areas. After two unsuccessful focused attempts or a genuinely ambiguous safety question, DD escalates; don't produce endless packets.

## 5. Real stop signs / 真正要停的情况

Stop and ask before crossing unauthorized boundaries, exposing credentials or PII, bypassing auth/tenant separation, running destructive migrations, changing a production secret/flag/deployment, ignoring a material failing regression, violating branch protection, or discovering an unreviewed side effect.

Do **not** block a routine PR solely because a browser E2E wasn't run, a docs-only SHA changed, QA couldn't fetch every log, or a cosmetic issue remains—unless that specific proof is essential to the change's safety or an explicitly accepted requirement.

## 6. Cadensy lessons / DESIGN-04 的教训

- Invite Path Trip A vs Header Trip B was a **real critical isolation issue**, worth targeted independent review.
- The UI later showed Trip B while the POST targeted A: fix the **specific display bug**, not the entire navigation architecture.
- New tests initially weren't collected by CI: check the actual test entry when adding tests.
- A docs-only main sync changed SHA but not executable code: revalidate relevant metadata, **reuse prior code QA**, don't ask ChatGPT to repeat the same architectural review.
- A frontend merge may automatically publish ECR while ECS production deployment remains separate: mention both effects in one approval request.

## 7. Final output / 最终汇报保持四行左右

```text
Changed: <one-sentence behavior + files or PR>
Checked: <actual test/CI result, or NOT RUN>
Risk: Quick / Standard / Critical; any genuine remaining blocker
Status: <PR/merge/deploy state> | Next: <one action or "done">
```

Add SHA and deeper evidence links **when a PR, release, or risk actually needs them**. Default to a small honest update, not an audit dossier.
