# Release-readiness checklist

Run this immediately before shipping a fix, including a fix written in response to CI breakage. Tick an item only from evidence gathered in this change window.

This checklist is an inference from GitHub's public November 2026 status incidents. It is not a GitHub document, and it is not written for or on behalf of any employer. The supporting timeline and citations are in [POSTMORTEM.md](POSTMORTEM.md).

Times in the notes below use IST (Asia/Calcutta, UTC+5:30). Rechecked against public GitHub Status at about 3:05 PM IST on 14 November 2026.

## Before you ship

- [ ] **1. Vendor status.** Read <https://www.githubstatus.com/> and any open incident pages. Record the live component state for each of the following. If Actions, Git Operations, or Pull Requests are not operational, default to no-ship unless a human overrides in item 14.
  - [ ] Actions
  - [ ] Git Operations
  - [ ] Pull Requests
  - [ ] Webhooks
  - [ ] Pages
  - [ ] API Requests
  - [ ] Packages
  - [ ] Codespaces
  - [ ] Status snapshot saved (URL, time, IST)
- [ ] **2. Unresolved RCA debt.** If a recent critical or major Actions-involved or merge-path incident still has no published root cause, say so explicitly in the go/no-go note. Do not invent a cause. At the 14 November 2026 recheck this was still true of the [11 November critical multi-product incident](https://www.githubstatus.com/incidents/dvy0t3mvh1lg) and the resolved [13 November Pull Requests incident](https://www.githubstatus.com/incidents/1xzmt89vv61l).
- [ ] **3. CI is green on the fix SHA.** Required checks have passed on the exact commit you intend to ship, not on an earlier commit of the same branch. Re-check that the pull-request head SHA matches that commit. Stale PR metadata was a published symptom of the [10 November incident](https://www.githubstatus.com/incidents/hnf21m9jkl3c).
- [ ] **4. Missing runs, not just failed runs.** Every expected workflow actually started. If a GitHub incident involved dropped events or a backlog, retrigger any missing workflows before declaring green. GitHub said Actions skips events older than 30 minutes ([9 November, Webhooks and Actions](https://www.githubstatus.com/incidents/xxb3l2bm76dr)) and that about 0.09% of pull-request events never started a workflow on 10 November.
- [ ] **5. Rerun flakes and leftovers.** Rerun failed or flaky jobs once on the fix SHA. After a vendor incident, also inspect queued, waiting, or cancelled jobs that may be holding concurrency. The [9 November Actions and Checks API incident](https://www.githubstatus.com/incidents/tp8cghg7tlc4) left jobs in a state that limited concurrency until they were released.
- [ ] **6. Authenticated, retry-safe path.** Checkout, API calls, and release-asset downloads in the shipping workflows use authenticated requests. Retries are idempotent. SSH or a documented fallback exists for Git if you rely on HTTPS. On 9 November GitHub warned that anonymous CI jobs and release downloads may fail with a 504 ([incident](https://www.githubstatus.com/incidents/c9px7cw2ghrs)).
- [ ] **7. Rollback plan.** Written and tested enough to execute without the current branch: previous artifact or image, revert commit or previous release, and (if used) a flag-off path. The rollback must revert the actual failing change, not only a config wrapper. GitHub's [4 November reintroduction](https://www.githubstatus.com/incidents/xr3myzj7ms51) is the cautionary case: configuration was rolled back, the code change was not, and a later deploy brought the fault back.
- [ ] **8. Feature flag or equivalent gating.** The fix can be disabled without a full revert, or you have explicitly accepted that it cannot and have documented why.
- [ ] **9. Canary or staged rollout.** Ship to a small slice or a non-critical environment first. Watch a pre-agreed set of signals for a pre-agreed bake time before widening.
- [ ] **10. Monitoring and alerts.** Dashboards and alerts for the changed path are live (error rate, latency, and, for CI-platform changes, workflow start time and missing-check rate). On-call knows what will fire.
- [ ] **11. Merge and release path health.** A test merge or dry-run release against current GitHub pull-request and merge APIs succeeds. If merges are timing out, stop, even if Actions is listed as operational.
- [ ] **12. Communications draft.** Internal note and, if needed, a customer note are drafted but **not sent**. They cite only published GitHub wording and do not assert a cause GitHub has not published.
- [ ] **13. Blast radius and owners.** Service owners, on-call, and a rollback owner are named and reachable. The change window does not collide with a known GitHub migration or an open major or critical incident unless item 14 explicitly accepts that risk.
- [ ] **14. Change approval / go-no-go.** A human records ship, wait, or abort, with the evidence from items 1-13. This step is not automated.
- [ ] **15. Post-ship watch.** A human, or a tightly scoped automated watch with a human rollback owner, stays on the canary signals through the bake window, with authority to roll back.

## Human sign-off

These steps can be prepared with automation. The decision itself needs a named person.

- [ ] **Go / no-go to ship.** Risk acceptance, not a threshold. Green CI on a fix SHA can coexist with an unpublished critical RCA, a merge-path incident, or a backlog GitHub has not fully described.
  - Decision: ship / wait / abort
  - Signed:
  - Time (IST):
- [ ] **Merge-freeze and freeze-lift.** Freezing and unfreezing change delivery commitments. Lifting after "Actions is operating normally" would have been premature on 10 November, when pull-request processing was still failing and some PRs showed a stale head commit.
  - Decision:
  - Signed:
  - Time (IST):
- [ ] **Rollback decision.** Automated rollback may propose or trip a pre-agreed safety gate. Taking production backwards, and confirming the rollback actually removes the failing change, needs a person who can see side effects.
  - Decision:
  - Signed:
  - Time (IST):
- [ ] **Customer-impact statements.** Impact numbers in the November public record ranged from 0.09% non-starts to a 65% peak of affected workflow runs, and several incidents have no published cause. Auto-posted blame or percentages will be wrong or unfair.
  - Decision:
  - Signed:
  - Time (IST):
- [ ] **External communications.** Status posts, customer emails, and support macros are attributable speech. Drafts may be prepared automatically. Sending may not.
  - Decision (send / hold):
  - Signed:
  - Time (IST):
- [ ] **Root-cause acceptance of our fix.** A human accepts that the fix addresses the failure mode you actually saw, and that you are not shipping a coincidental change during a vendor incident. Vendor RCA, when unpublished, cannot be filled in by a model.
  - Decision:
  - Signed:
  - Time (IST):
- [ ] **Change approval for production.** Authorization and accountability stay with a named owner, especially when the checklist has exceptions (shipping during an open GitHub incident, skipping a canary, no feature flag).
  - Decision:
  - Signed:
  - Time (IST):
- [ ] **Accepting residual risk when GitHub's RCA is still unpublished.** At recheck, examples include the 11 November critical incident and the resolved 13 November merge-timeout incident. Only a person can decide whether "backlog cleared, cause unknown" is an acceptable environment in which to ship.
  - Decision:
  - Signed:
  - Time (IST):

## Evidence pack

Record links and identifiers here, or attach them next to the ticks.

- Status snapshot:
- Open incident pages:
- Ship SHA:
- Pull-request head SHA:
- Check URLs:
- Retriggered workflow runs:
- Rollback plan / previous artifact:
- Flag or gate:
- Canary signals and bake window:
- Communications drafts (unsent):
- Owners and on-call:

## Safe to automate

These are appropriate to automate with a human-visible audit trail. They do not replace the signatures above.

- Polling GitHub Status and opening an incident ticket or chat thread when Actions, Git, pull requests, Webhooks, or Pages change state. This must not be the only signal.
- Confirming required checks are green on the exact ship SHA, and flagging missing expected workflows.
- Retriggering failed, dropped, or stale jobs under a documented policy after vendor recovery. A human should still see the list.
- Enforcing authenticated checkout and downloads in workflow templates, and failing the workflow if a job is anonymous.
- Canary metric gates that pause a rollout when pre-agreed error or latency thresholds trip, if the alternative is a human override rather than an automatic widen.
- Drafting (not sending) internal and external communications from a template that only includes cited status-page text.
- Collecting this evidence pack.
