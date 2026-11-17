---
name: github-evidence-harness
description: "Evidence-first GitHub execution and review harness for multi-agent engineering. Verify exact SHAs, file diffs, CI jobs and logs; separate reported from executed evidence; grade risk; delegate routine work to Grok/QA and escalate only critical architecture or security changes to ChatGPT and the human owner."
---

# GitHub Evidence Harness / GitHub 证据与分级审查

## Purpose / 目标

Use this skill whenever an agent inspects, implements, reviews, merges, or reports on GitHub repository work. This is an **evidence and decision protocol**, not permission to execute. It cannot guarantee zero hallucinations; it makes unsupported assertions visible and prevents them from being treated as verified.

核心目标：**少开会、少重复审查；保留不可妥协的证据、权限与安全边界。** 普通工程任务由 Grok/DD + 独立 QA 完成；ChatGPT 只介入最高风险、实质性争议或 Owner 明确指定的独立架构审查。

## 1. Non-negotiable invariants / 不可妥协

1. **Pin first.** Before reviewing or acting, read repository, base branch SHA, PR head SHA and worktree HEAD (where applicable). A branch name is not a version. Report exact SHA.
2. **Read real artifacts.** Use file contents at the pinned SHA, full diff/file list, actual workflow definitions, job metadata, and job logs when the claim needs logs. A PR description, agent report, or green badge alone is not executable evidence.
3. **Label every claim:** `STATIC-VERIFIED` (inspected source or Git metadata), `EXECUTED-PASS` / `EXECUTED-FAIL` (observed run and scope), `REPORTED` (someone else's unverified claim), `NOT RUN`, `UNKNOWN`, `OPEN`. Never upgrade a label by paraphrasing.
4. **Separate types of acceptance:** design accepted ≠ code implemented ≠ tests executed ≠ PR merged ≠ artifact published ≠ production deployed. Separate `CURRENT`, `PROPOSED`, `OWNER-APPROVED`, `DEFERRED`, `OPEN`.
5. **Negative evidence has a boundary.** "No defect found in scanned files" is not "no defect exists." A static test is not React/browser E2E. A local unit test is not the new HTTP/DB contract test.
6. **Scope and permissions are distinct.** Read-only inspection does not authorize implementation; writing a branch does not authorize push, merge, publication or production changes. This document itself grants no authorization.
7. **Never report a successful operation before its tool returns success.** Check actual post-action state when possible. Never invent URLs, commit IDs, job names, logs, exact counts, test results, or approvals.
8. **Keep secrets out.** Do not copy credentials, raw user tokens, private data, proprietary incident logs, or sensitive identifiers into PRs, public repositories or agent handoffs.

## 2. Minimal evidence path / 快速证据链

For a normal PR, use the *shortest sufficient* sequence:

1. **Identity:** repo/full name; current base SHA and head SHA; PR state; whether head contains base (merge-base / ahead-behind). Re-check after head or base changes.
2. **Scope:** list every changed path; examine real diff against target base. Compare to allowlist and known behavior. "Docs-only" requires path/content and workflow-trigger verification, not filename guesswork.
3. **Safety:** inspect relevant source and existing tests at exact head. Search for auth, state, boundary, rollback or observable behavior affected by changed lines; do not extrapolate to unrelated modules.
4. **Tests:** read the relevant workflow definition, run/check metadata, correct event (push vs pull_request), steps and logs. Check named tests, counts, failures, skips and whether new tests are actually collected by CI.
5. **Independent QA:** QA inspects pinned head without editing implementation; maintain an explicit list of NOT RUN cases and bounded claims.
6. **Decision:** `PASS`, `CONDITIONAL PASS` or `NO-GO` with **one actionable reason** per blocker; assign a risk level and who may approve. No automatic serial referrals to ChatGPT.

If a connector returns 403 or missing log content, label `CI transcript UNKNOWN`. A green job can be verified at metadata level, **not** as proof of test case names. Another authorized reviewer can fetch the missing log and record that reader as the source. Do not invent a workaround or request secrets.

A new head after edits, rebase or merge is **a new review target**. Reuse prior QA only if changed code/test blobs are verified identical and new checks pass; explicitly state inherited evidence and its boundaries.

## 3. Review risk tiers / 分级审查，而非每次找 ChatGPT

Risk is based on **effect and uncertainty**, not changed-line count.

| Tier | Typical examples | Default reviewer | ChatGPT? |
| --- | --- | --- | --- |
| **L0 — Trivial** | Safe docs/index wording, links, comments, non-executing formatting | Grok/DD alone, optional Docs check | No |
| **L1 — Routine** | Isolated tests, bounded UI copy, local helper/refactor with unchanged contract | Grok/DD + quick independent QA or CI | No |
| **L2 — Significant** | Multi-file feature, cross-component state, non-sensitive API behavior, integration regression | Grok/DD + independent QA, targeted evidence | Usually no; only on dispute/escalation |
| **L3 — Critical** | Auth/session/tenant or trip isolation, permissions, privacy/secrets, persistent-data integrity/migration, irreversible deletion, production deployment or high-impact rollback, uncontained release, unresolved material security finding | Human Owner + independent QA; ChatGPT for architecture/security challenge | Yes for material architecture/security decision; Owner always owns execution approval |

**Escalate one tier** for a serious evidence gap affecting correctness, divergent reviewer conclusions, missing rollback, unknown downstream effects, scope creep across trust boundaries, or unreviewed CI/workflow changes. A tiny one-line authorization bug can be L3; a lengthy documentation refactor can be L0.

**ChatGPT trigger (critical only):** L3 design/authorization boundary, cross-tenant information exposure risk, irreversible production decision, or QA/engineering unable to resolve a material contradiction. If a critical decision has already been explicitly accepted and the new change is byte-identical docs-only, do not reflexively demand another ChatGPT review. Grok must record the reason when escalating.

**Human Owner remains final authority.** ChatGPT provides independent analysis, not credentials, a deployment token, or a substitute for owner consent.

## 4. Action gates / 风险等级不等于操作授权

Use the smallest practical permissions for the target repository. Maintain a repo-specific, owner-approved standing delegation policy if the Owner wants Grok to handle routine work without repeated prompts.

- **G0 — Read:** inspect repository, issues, PRs and public/authorized logs. Default autonomous.
- **G1 — Local change:** edit/build/test in a bounded, approved worktree. No cloud/production implied.
- **G2 — Publish branch/PR:** push commits, create/update PR and request QA. Not a merge.
- **G3 — Merge to main:** GitHub merge; requires an explicit per-PR owner decision **or** an existing, clearly scoped standing delegation (L0–L2 only).
- **G4 — Downstream artifact publication:** any automatic ECR/package/image release triggered by G3. Analyze actual workflows *before* G3. If G3 automatically causes G4, treat them as **one combined impact for approval**, not two repetitive conversations; never grant G4 implicitly.
- **G5 — Production operation:** ECS deploy, environment/flag changes, data migration, secrets, irreversible production action. Owner approval required; do not infer from G3/G4.

Historical project-specific gates remain effective until the Human Owner explicitly replaces them. **Publishing this skill does not retroactively authorize any blocked PR, G3/G4/G5, database run, cloud action, or previously OPEN product decision.**

For routine L0–L2 work, the Owner may define a standing policy such as: "Grok can G1/G2, independently QA and merge approved path classes once all named checks pass, provided the workflow has no production side effects." Without that policy, stop at the last explicitly authorized action. Do not claim an unstated standing grant.

## 5. Cheap review workflow / 轻量闭环

**Intake (DD):** one task, clear goal, target base/head, allowed paths, expected behavior, risk tier, authorized gates. Skip lengthy architecture packets for L0/L1.

**Implement (Worker/Codex):** smallest change; preserve unrelated code; tests appropriate to risk. Own work is `IMPLEMENTER-REPORTED` until independently checked.

**Verify (QA):** test exact head and critical negative cases; confirm changed-file allowlist and CI collection; summarize PASS/FAIL, executed vs NOT RUN, and blockers. QA must not quietly patch the same implementation it independently reviews.

**Decide (DD):** if L0–L2 and evidence complete, close using delegated permissions; if a clear defect exists, request **one targeted correction** and one recheck. No endless "Rev5.1/5.2/5.3" cycle unless a real new finding requires it. After two failed targeted iterations, disagreement, or scope/risk escalation, ask the Owner to route to ChatGPT.

**Finish:** exact head/base, code/test evidence, PR state, artifact state, remaining known risk, next authorized action. Ordinary result should be 5–10 lines, not a 20-page report.

## 6. Critical STOP rules / 必须中止的条件

STOP before writing or merging when: permissions are missing; head drift is unexamined; protected-path scope expands; CI relevant checks fail; a required new test is absent from actual CI collection; copied secrets/PII appear; authorization/tenant checks can be bypassed; a new DB migration or cloud side effect appears; rollback is not credible; a material question is still OPEN; branch protection/review rules would be bypassed; or the supposed independent QA is only the implementer's self-report.

A skip is not a pass. A PR push-run that skips code jobs can still be green; identify the actual pull_request run with code checks. A docs-only main push can still trigger builds if filters are broad: inspect filters and called workflows before claiming "no ECR."

## 7. Case study: Cadensy DESIGN-04 / 真实经验提炼

Historical pattern, **not current authorization**:

- Account missing `X-Trip-Id` and Header/Path conflict needed explicit semantics; documentation acceptance did not prove Phase 3 runtime behavior.
- Invite creation had `Route Trip A` versus `Session/Header Trip B`; green CI initially omitted the new failing test. The fix required fresh organizer membership evidence, correctly matched Path/Header, and independent negative tests.
- A later UI-only issue displayed cached Trip B while the POST correctly targeted A: code safety and UX consistency were separate criteria.
- A PR head synced `main` with docs-only changes: check exact parents, file blobs, merge-base and new CI. When code is byte-identical, preserve old code QA but validate the new SHA.
- Historical G3 (merge), G4 (automatic frontend ECR publication), G5 (ECS deploy) were distinct. Never treat a technical PASS as action approval.

**Generalizable lesson:** check the right artifact, not the biggest report. Grok handles routine evidence closure. Reserve ChatGPT for genuinely high-impact design/security decisions, not every docs sync or PR revision.

## 8. Compact handoff template / 一屏交付

```text
TASK: <what changed>
RISK: L0/L1/L2/L3 — one-line reason
PIN: base=<SHA> head=<SHA>
DIFF: <paths/critical contract>
EVIDENCE: STATIC-VERIFIED <...>; EXECUTED <...>; UNKNOWN/NOT RUN <...>
QA: PASS / CONDITIONAL PASS / NO-GO; exact head
GATES: authorized <...>; blocked <...>; downstream side effects <...>
DECISION: Grok autonomous / Human Owner / ChatGPT L3
NEXT: one concrete step
```

Never fabricate the placeholders. A six-line truthful audit is better than a long invented assurance.
