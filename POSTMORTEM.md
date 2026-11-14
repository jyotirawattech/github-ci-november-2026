# CI outage postmortem: public GitHub status incidents, November 2026

**Audience:** a team that depends on GitHub Actions (and related GitHub CI surfaces) and needs a factual first-pass timeline, plus a pre-ship readiness checklist.

**Scope window:** 1 November 2026 00:00 IST through the original data snapshot at **22:22:15 IST / 16:52:15 UTC on 13 November 2026**, with a public-status recheck at **about 3:05 PM IST / 09:35 UTC on 14 November 2026**. Times below are shown in IST (Asia/Calcutta, UTC+5:30) with the original UTC value from GitHub.

**Status of this document:** unofficial, independent analysis compiled only from public GitHub sources. It is not a postmortem of an internal product incident, it was not written or reviewed by GitHub, and it is not written for or on behalf of any employer. Recheck updates are labelled as such.

**Suggested repository name:** `github-ci-november-2026`

---

## How to read this document

Four kinds of statement are kept apart:

| Label | Meaning |
| --- | --- |
| **Fact** | A timestamp, component state, impact figure, or other value taken from GitHub's status API or incident page. Derived arithmetic from those timestamps is marked as derived. |
| **GitHub's statement** | Wording from a status update or resolved post, paraphrased or quoted. |
| **Inference** | A conclusion drawn from those facts or statements. Not published by GitHub. |
| **Unknown / Not published** | GitHub has not published the value or cause. Hypotheses are labelled **speculation**. |

Nothing in this document invents times, durations, causes, impact numbers, or quotes. Where GitHub's resolved post gives an impact window that differs from the status-page `started_at` / `resolved_at` timestamps, both are recorded. Status-page open time is not treated as the customer-impact window.

---

## Sources and snapshot

Primary sources, retrieved 13 November 2026:

- GitHub Status incident API: <https://www.githubstatus.com/api/v2/incidents.json> (page `updated_at` 16:52:15 UTC / 22:22:15 IST)
- Unresolved incidents: <https://www.githubstatus.com/api/v2/incidents/unresolved.json> (rechecked 22:41 IST; no newer updates)
- Individual incident pages under <https://www.githubstatus.com/incidents/{id}> (HTTP 200 confirmed for the CI-primary incidents)
- History index: <https://www.githubstatus.com/history>
- Availability-report tag: <https://github.blog/tag/github-availability-report/>
- Latest published monthly report: [GitHub availability report: October 2026](https://github.blog/news-insights/company-news/github-availability-report-october-2026/) (published 12 November 2026). A November 2026 report was not up at snapshot time (`https://github.blog/news-insights/company-news/github-availability-report-november-2026/` returned HTTP 404). It was still 404 at the 14 November recheck.

The incidents API returned the 50 most recent incidents, spanning 17 September 2026 through 13 November 2026, including every November 2026 incident in that set (20 incidents). No November incident is listed with a 1 November UTC start. Packages and Codespaces were not named as affected components in any November incident in this dataset.

## Recheck, 14 November 2026

Primary sources were retrieved again at about **3:05 PM IST / 09:35 UTC on 14 November 2026**:

- GitHub Status incident API: <https://www.githubstatus.com/api/v2/incidents.json> (page `updated_at` 05:54:27 UTC / 11:24:27 IST on 14 November 2026)
- Unresolved incidents: <https://www.githubstatus.com/api/v2/incidents/unresolved.json> (empty; no open incidents)
- Incident page: <https://www.githubstatus.com/incidents/1xzmt89vv61l>
- Availability-report URL: <https://github.blog/news-insights/company-news/github-availability-report-november-2026/> still returned HTTP 404. The latest published monthly report remains [GitHub availability report: October 2026](https://github.blog/news-insights/company-news/github-availability-report-october-2026/).

What changed after the 13 November 22:22 IST / 16:52 UTC snapshot:

1. **[Incident with Pull Requests](https://www.githubstatus.com/incidents/1xzmt89vv61l)** is no longer open. After the snapshot's last captured update (16:52:15 UTC, still `investigating`, ~1% of merge requests timing out), GitHub posted:
   - 22:58:01 IST / 17:28:01 UTC: `monitoring`. "The degradation affecting Pull Requests has been mitigated. We are monitoring to ensure stability."
   - 23:16:11 IST / 17:46:11 UTC: `monitoring`. "We continue to monitor the stability of merge requests."
   - 23:59:31 IST / 18:29:31 UTC: `resolved`. "This incident has been resolved... A detailed root cause analysis will be shared as soon as it is available."
   Status-page open time was 15:47:12 UTC to 18:29:31 UTC on 13 November (21:17:12 IST to 23:59:31 IST). Derived duration: **2 hours 42 minutes**. Impact figures in the updates remain the ones already captured: initially 3% of merge requests timing out, then less than 1%, then about 1%. The resolved post did not publish a final percentage or a root cause. Actions was still not listed as an affected component. No later update was present at recheck.
2. **One new November incident** was posted after the snapshot: [Copilot experiencing degraded model availability for some GPT models](https://www.githubstatus.com/incidents/51drrqvsv66g) (minor, resolved). Status-page start 01:43:51 IST on 14 November / 20:13:51 UTC on 13 November; resolved 02:57:59 IST / 21:27:59 UTC. The resolved post gives an impact window of 19:05 to 21:27 UTC (00:35 to 02:57 IST on 14 November) and attributes the degradation to an upstream model provider. It is out of CI scope and is listed under "November incidents not used as CI evidence."
3. **No older November incident** gained a new root-cause post, a new numeric impact figure, or any other update after the snapshot. Unpublished-RCA items from the original draft remain unpublished, including the 11 November critical multi-product incident.
4. **November incident count** in the API is now **21** (the original 20, plus the Copilot incident). The CI-primary count is unchanged: nine Actions-listed incidents, eight distinct events.
5. There were **no unresolved incidents** at recheck. All listed GitHub components, including Actions and Pull Requests, were `operational`.

---

---

## Summary

Through the 13 November snapshot, GitHub had published **20** public status incidents. The 14 November recheck adds one Copilot-only incident, for **21** in November to date. The CI counts below are unchanged. **Nine** of them listed **Actions** as an affected component. Two of those nine are the same event told twice: the 4 November Actions incident at <https://www.githubstatus.com/incidents/h02n9572mx9y> tells readers to use the combined summary at <https://www.githubstatus.com/incidents/xr3myzj7ms51>. So there are **eight distinct Actions-impacting events**, three of them labelled **critical**, three **major**, and two **minor**.

Published customer impact on Actions was not uniform:

- On 9 November, during a planned Actions database migration and traffic shift, GitHub said an average of **28%** of workflow runs failed to start, update, or report status correctly, peaking at **65%**, and that some jobs were then left in a state that limited concurrency until the runs were released. ([Incident with Actions and Checks API](https://www.githubstatus.com/incidents/tp8cghg7tlc4))
- Earlier the same day, a degraded Kafka broker delayed webhooks and Actions. GitHub said **Actions skips events older than 30 minutes, so some workflow runs did not start**. Some webhook deliveries were delayed by up to **3.5 hours**. ([Incident with Webhooks and Actions](https://www.githubstatus.com/incidents/xxb3l2bm76dr))
- On 10 November, GitHub said Actions workflows were delayed for about **1.8%** of events, peaking at about **7%**, with typical delays of **1.5 to 3.5 minutes** (up to **10 minutes**), and that about **0.09%** of pull-request events never started their workflows and needed a retrigger. ([Incident with Git Operations and Pull Requests](https://www.githubstatus.com/incidents/hnf21m9jkl3c))
- On 11 November a **critical** multi-product incident marked Actions as `major_outage` and left a job backlog that GitHub later said was fully processed. **Root cause: Not published.** GitHub said a detailed RCA would be shared when available. ([Incident](https://www.githubstatus.com/incidents/dvy0t3mvh1lg))

CI was also hit on paths that are not the Actions component itself. On 3 November, Git HTTPS clones, fetches, and pushes were slow or failed for hours (SSH was not affected). On 9 November GitHub warned that **CI jobs and release downloads may fail with a 504** if they make anonymous calls. A **major** Pull Requests incident on 13 November had merge attempts timing out (initially 3%, then about 1%). It was still open at the original snapshot and **resolved** at 23:59:31 IST / 18:29:31 UTC that night. Actions was not listed on that incident. Root cause: **Not published** (GitHub promised a detailed RCA and had not posted one at recheck).

**Inference:** a team that treats "Actions is green on the status page" as sufficient CI health will miss delayed starts, silent non-starts that need a retrigger, stuck runs that consume concurrency after recovery, anonymous checkout/release-download 504s, and merge-gate failures. Those failure modes are all in GitHub's published November updates.


## Affected services and published impact

### CI-primary incidents (Actions and/or Pages listed)

| Incident | Status-page impact | Status open (IST / UTC) | Published impact window (if given) | Components GitHub marked | Published Actions / Pages impact |
| --- | --- | --- | --- | --- | --- |
| [Incident with Pull Requests, Git Operations and Issues](https://www.githubstatus.com/incidents/smtdswd1h2w4) | major | 15:19:55-17:20:06 IST on 4 Nov / 09:49:55-11:50:06 UTC | Beginning ~09:35 UTC (15:05 IST); recovery began ~11:24 UTC (16:54 IST); remaining Actions impact mitigated 11:43 UTC (17:13 IST); resolved 11:50 UTC (17:20 IST) | Pull Requests, Issues, APIs, Git Operations, Actions | Intermittent errors and delays, including Actions. No Actions percentage published. |
| [Incident with Actions and Pages](https://www.githubstatus.com/incidents/xr3myzj7ms51) | major | 21:35:31-22:31:11 IST on 4 Nov / 16:05:31-17:01:11 UTC | 15:49-16:48 UTC (21:19-22:18 IST), and again 17:20-17:55 UTC (22:50-23:25 IST) - the second window is the related incident below | Actions, Pages, Webhooks (Code Scanning and Copilot coding agent named in the resolved post) | Workflow runs and job status updates delayed by up to 25 minutes. Pages deployments dropped to about 4% of normal volume. Webhook deliveries dropped to about 58% of normal volume. Delayed events were processed after mitigation. |
| [Incident with Actions](https://www.githubstatus.com/incidents/h02n9572mx9y) | major | 23:10:33-23:46:08 IST on 4 Nov / 17:40:33-18:16:08 UTC | Combined into the incident above (second window 17:20-17:55 UTC) | Actions | See combined summary. Status updates said customers may observe delayed job status updates. |
| [Incident with Actions](https://www.githubstatus.com/incidents/t89g2q1800fr) | minor | 22:20:11-23:12:35 IST on 5 Nov / 16:50:11-17:42:35 UTC | 16:39-16:57 UTC (22:09-22:27 IST) | Actions (Checks named in the resolved post) | At peak, about 3% of workflow runs failed to queue; roughly 0.21% of in-progress runs failed. |
| [Degraded performance for multiple GitHub services](https://www.githubstatus.com/incidents/pfbmpxxfq1cy) | minor | 05:06:06-05:50:49 IST on 6 Nov / 23:36:06 UTC 5 Nov-00:20:49 UTC 6 Nov | 23:21 to approximately 23:26 UTC on 5 Nov (04:51-04:56 IST on 6 Nov) | Actions, Pages, Issues, Git Operations (API and authentication named in the resolved post) | Failed requests and Git pushes; some Actions workflow runs were delayed. No percentage published. |
| [Incident with Webhooks and Actions](https://www.githubstatus.com/incidents/xxb3l2bm76dr) | major | 15:30:35-17:20:35 IST on 9 Nov / 10:00:35-11:50:35 UTC | 09:23-11:40 UTC (14:53-17:10 IST). Delays for a small share began as early as 01:00 UTC (06:30 IST). Mitigation at 10:37 UTC (16:07 IST); backlog cleared 11:40 UTC (17:10 IST) | Actions, Webhooks (Dependabot and dependency graph named in the resolved post) | Delayed Actions workflow runs. Actions skips events older than 30 minutes, so some runs did not start. Until mitigation, an average of 26% of webhook deliveries were delayed, peaking at 30%. Some deliveries were delayed by up to 3.5 hours. |
| [Incident with Actions and Checks API](https://www.githubstatus.com/incidents/tp8cghg7tlc4) | critical | 23:42:37 IST 9 Nov-00:29:32 IST 10 Nov / 18:12:37-18:59:32 UTC 9 Nov | 18:07-18:59 UTC (23:37 IST 9 Nov-00:29 IST 10 Nov) | Actions, Webhooks, Pages (Checks API and Copilot code review named in updates / resolved post) | Average of 28% of workflow runs affected; peak 65%. Some jobs left in a state that limited available concurrency until the runs were released. |
| [Incident with Git Operations and Pull Requests](https://www.githubstatus.com/incidents/hnf21m9jkl3c) | critical | 21:40:52-23:25:03 IST on 10 Nov / 16:10:52-17:55:03 UTC | Approximately 15:00-17:55 UTC (20:30-23:25 IST) | Actions, Pull Requests (repository pages and Git operations named in the resolved post) | Workflows delayed for approximately 1.8% of events, peaking at about 7% between 16:30 and 16:50 UTC (22:00-22:20 IST). Delays typically 1.5 to 3.5 minutes, up to 10 minutes. Approximately 0.09% of pull-request events did not start their workflows and needed to be re-triggered. Some PRs continued to display an earlier source-branch commit after the broader degradation ended. |
| [Impact across multiple GitHub products including Actions, Pages, and Copilot products.](https://www.githubstatus.com/incidents/dvy0t3mvh1lg) | critical | 02:18:12-02:51:39 IST on 12 Nov / 20:48:12-21:21:39 UTC on 11 Nov | Not published beyond the status-page timeline | Actions (marked `major_outage`), Pages (partial then major outage), Webhooks, Copilot | GitHub said a backlog of Actions jobs was later fully processed. No percentage, delay, or root cause published. |

### CI-adjacent incidents (checkout, anonymous CI calls, merge gate, or webhook triggers)

| Incident | Status-page impact | Status open (IST / UTC) | Why it is in scope | Published CI-relevant impact | Root cause |
| --- | --- | --- | --- | --- | --- |
| [Incident with Git Operations](https://www.githubstatus.com/incidents/13b6lyl8d7bz) | minor | 02:27:14-04:11:03 IST on 4 Nov / 20:57:14-22:41:03 UTC on 3 Nov | HTTPS clone/fetch/push is the default checkout path for many workflows | GitHub said Git operations over HTTPS were degraded from 15:00 UTC to 22:37 UTC on 3 Nov (20:30 IST 3 Nov-04:07 IST 4 Nov). For about half of Git HTTPS traffic, median response time rose from under 100 ms to as high as 5 seconds. Average error rate 0.65%, peak 1.29%. SSH was not affected. A load-balancer block then prevented some users of one third-party integration from performing Git HTTPS operations until it was removed at 13:26 UTC on 4 Nov (18:56 IST). | Published: buildup of excess connections on Git HTTPS hosts. |
| [Increase in timeouts for some unauthenticated users](https://www.githubstatus.com/incidents/pyynvrm1vskw) | minor | 16:06:05-18:02:35 IST on 8 Nov / 10:36:05-12:32:35 UTC | Same class of unauthenticated traffic that GitHub explicitly tied to CI the next day | Degraded performance primarily for unauthenticated users; API Requests marked degraded, then mitigated. No CI-specific figure published. | Not published. GitHub said a detailed RCA would be shared when available. A mid-incident update said they had "identified a problematic component and [were] working to redeploy it." |
| [Increased timeouts due to surge in anonymous users](https://www.githubstatus.com/incidents/c9px7cw2ghrs) | minor | 11:29:07-14:39:56 IST on 9 Nov / 05:59:07-09:09:56 UTC | GitHub explicitly warned about CI jobs | **GitHub's statement:** "CI jobs and release downloads may fail with a 504 Gateway Timeout status if they make anonymous calls." Logged-in users and authenticated requests were later described as unaffected. Impact recurred after an initial mitigation. No CI failure percentage published. | Not published. Title and updates attribute it to a surge in anonymous traffic. Detailed RCA not published. |
| [Incident with Webhooks](https://www.githubstatus.com/incidents/1h2wbly1y72t) | minor | 01:52:20-01:56:23 IST on 12 Nov / 20:22:20-20:26:23 UTC on 11 Nov | Webhook delivery can start workflows | Degraded performance reported, then "Webhooks is operating normally" about four minutes later (derived from status timestamps). No impact figures. | Not published. |
| [Incident with Webhooks API](https://www.githubstatus.com/incidents/gl7yhd7jx00v) | minor | 00:13:13-01:03:54 IST on 13 Nov / 18:43:13-19:33:54 UTC on 12 Nov | Same | Timeouts and errors impacting about 1% of customer traffic, then recovered. Actions was not marked. | Not published. |
| [Incident with Pull Requests](https://www.githubstatus.com/incidents/1xzmt89vv61l) | major, **still investigating** at snapshot | Started 21:17:12 IST / 15:47:12 UTC on 13 Nov; last update 22:22:15 IST / 16:52:15 UTC | Merge is a common ship gate even when Actions itself is marked operational | Some merge attempts timing out. GitHub said this initially impacted 3% of requests and was later down to less than 1%, then "continuing to investigate and monitor ~1% of merge requests timing out." Actions was not listed. | Not published. Still investigating. |

**Fact:** no November incident in this dataset listed Packages or Codespaces as a component.


## Timeline per incident

Status-page updates are listed in the order GitHub displayed them. Impact windows quoted from resolved posts are called out when they start earlier than `started_at`.

### 1. Incident with Git Operations - 3 November (CI-adjacent)

- **Page:** <https://www.githubstatus.com/incidents/13b6lyl8d7bz>
- **Shortlink:** <https://stspg.io/n478rfsspcmr>
- **Status-page impact / final status:** minor / resolved

**GitHub's published impact window:** 15:00-22:37 UTC on 3 November (20:30 IST on 3 Nov-04:07 IST on 4 Nov). That is earlier than the first status update.

| Time (IST / UTC) | Status | GitHub's statement (abridged only where marked) |
| --- | --- | --- |
| 20:30 IST 3 Nov / 15:00 UTC | - | Resolved post: Git HTTPS operations on GitHub.com were degraded from this time. |
| 02:27:14 IST 4 Nov / 20:57:14 UTC | investigating | Investigating reports of degraded performance for Git Operations. |
| 03:07:34 IST / 21:37:34 UTC | investigating | Identified the issue and beginning to roll out a mitigation. |
| 03:35:09 IST / 22:05:09 UTC | investigating | Mitigation rolled out; customers should start seeing improvements. |
| 04:07:27-04:07:36 IST / 22:37:27-22:37:36 UTC | investigating, then monitoring | Significant improvement; some Git Operations may still be slow for a small set of customers. Marked mitigated. |
| 04:11:03 IST / 22:41:03 UTC | resolved | Full resolved post, including the HTTPS vs SSH split, latency and error rates, and the connection buildup cause, as quoted in the impact table. |
| 18:56 IST 4 Nov / 13:26 UTC 4 Nov | - | Resolved post: the load-balancer block that was part of mitigation was removed. Until then it prevented some users of one third-party integration from performing Git HTTPS operations. |

**Root cause (published):** buildup of excess connections on the hosts that serve Git HTTPS traffic, saturating network capacity. **Mitigation (published):** blocking a client traffic pattern at the load balancers and reloading them. **Follow-up GitHub said it is working on:** alerting on sustained Git HTTPS latency sooner, and making load-balancer mitigations take effect on existing connections immediately.

### 2. Incident with Pull Requests, Git Operations and Issues - 4 November (Actions also marked)

- **Page:** <https://www.githubstatus.com/incidents/smtdswd1h2w4>
- **Shortlink:** <https://stspg.io/18blsgwnbyy6>
- **Status-page impact / final status:** major / resolved

**GitHub's published start:** approximately 09:35 UTC (15:05 IST), about 15 minutes before the first status update (derived).

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| ~15:05 IST / ~09:35 UTC | - | Resolved post: customers began experiencing intermittent errors and delays. |
| 15:19:55 IST / 09:49:55 UTC | investigating | Investigating degraded performance for Git Operations, Issues and Pull Requests. |
| 15:20:47 IST / 09:50:47 UTC | investigating | Actions marked degraded. |
| 15:23:38 IST / 09:53:38 UTC | investigating | Pull Requests marked partial outage. |
| 16:08:49 IST / 10:38:49 UTC | investigating | Elevated errors and intermittent failures affecting Pull Requests, Git Operations, and Issues. |
| 16:41:30 IST / 11:11:30 UTC | investigating | API Requests marked degraded. |
| 16:47:52 IST / 11:17:52 UTC | investigating | Working on mitigating; some requests may fail or be delayed. |
| ~16:54 IST / ~11:24 UTC | - | Resolved post: recovery began. |
| 17:00:42 IST / 11:30:42 UTC | investigating | Degradation affecting API Requests, Git Operations, Issues and Pull Requests mitigated. Actions still marked degraded. |
| 17:13:52 IST / 11:43:52 UTC | monitoring | Actions mitigated (matches the resolved post's 11:43 UTC figure). |
| 17:20:06 IST / 11:50:06 UTC | resolved | Cause: a shared database reaching capacity. Service restored by switching to another database host. GitHub said it has since reduced database load, increased capacity, added earlier monitoring, and is improving recovery procedures. |

### 3-4. Incident with Actions and Pages, then a second Actions wave - 4 November

These are one event in GitHub's own accounting. The second incident's resolved text is: "Please refer to the combined summary in this related incident: https://www.githubstatus.com/incidents/xr3myzj7ms51."

- **Pages:** <https://www.githubstatus.com/incidents/xr3myzj7ms51>, <https://www.githubstatus.com/incidents/h02n9572mx9y>
- **Shortlinks:** <https://stspg.io/pjgptg3cfc6n>, <https://stspg.io/7pwv28hvkmz8>
- **Status-page impact / final status:** both major / resolved

| Time (IST / UTC) | Incident | Status | GitHub's statement |
| --- | --- | --- | --- |
| 21:19 IST / 15:49 UTC | combined RCA | - | First impact window starts (resolved post). |
| 21:35:31 IST / 16:05:31 UTC | xr3myzj7ms51 | investigating | Investigating degraded performance for Actions and Pages. |
| 21:41:38-21:47:29 IST / 16:11:38-16:17:29 UTC | xr3myzj7ms51 | investigating | Issues with Actions and Pages; Webhooks marked degraded; Actions then marked partial outage. |
| 21:44 IST / 16:14 UTC | combined RCA | - | First mitigation: configuration change rolled back. |
| 22:04:45 IST / 16:34:45 UTC | xr3myzj7ms51 | investigating | Identified an issue with events that might trigger Actions; mitigations applied; customers may observe delayed job status updates. |
| 22:15:48-22:19:27 IST / 16:45:48-16:49:27 UTC | xr3myzj7ms51 | investigating then monitoring | Webhooks, then Actions and Pages, marked mitigated. |
| 22:18 IST / 16:48 UTC | combined RCA | - | First published impact window ends. |
| 22:31:11 IST / 17:01:11 UTC | xr3myzj7ms51 | resolved | Combined resolved post published on this page (covers both windows). |
| 22:50 IST / 17:20 UTC | combined RCA | - | Second impact window starts. GitHub's statement: the code change itself was not reverted by the rollback, so a subsequent deployment reintroduced it. |
| 23:10 IST / 17:40 UTC | combined RCA | - | Second mitigation: rolled back again, then processed queued events and cleared backlogs. |
| 23:10:33 IST / 17:40:33 UTC | h02n9572mx9y | investigating | Investigating degraded availability for Actions. |
| 23:23:11 IST / 17:53:11 UTC | h02n9572mx9y | investigating | Identified an issue with events that might trigger Actions; mitigations applied; delayed job status updates possible. |
| 23:25 IST / 17:55 UTC | combined RCA | - | Second published impact window ends. |
| 23:37:51-23:46:08 IST / 18:07:51-18:16:08 UTC | h02n9572mx9y | monitoring, then resolved | Actions mitigated; pointer to the combined summary. |

**Root cause (published):** a configuration change to an internal event-routing service that unexpectedly paused delivery of several event streams. The change depended on feature flags that were not set up as intended, which the service did not handle gracefully.

**Published follow-ups:** the routing service will keep its existing behavior when a feature toggle is missing; a pre-deploy check that each routing flag exists and is registered under the correct owning service; making flag ownership less error-prone; improved alerting for paused event delivery.

**Inference:** a rollback that reverts configuration but not the accompanying code is an incomplete rollback. That is exactly what GitHub said happened between the two windows.

### 5. Incident with Actions (Checks database) - 5 November

- **Page:** <https://www.githubstatus.com/incidents/t89g2q1800fr>
- **Shortlink:** <https://stspg.io/lv4q3xm9r0rk>
- **Status-page impact / final status:** minor / resolved

**GitHub's published impact window:** 16:39-16:57 UTC (22:09-22:27 IST), which starts before and ends during the status-page incident.

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 22:09 IST / 16:39 UTC | - | Resolved post: impact begins. |
| 22:20:11 IST / 16:50:11 UTC | investigating | Investigating degraded performance for Actions. |
| 22:22:52 IST / 16:52:52 UTC | investigating | Some workflow runs may experience delays or errors when starting. Investigating and preparing a mitigation. |
| 22:27 IST / 16:57 UTC | - | Resolved post: impact window ends. |
| 22:34:46 IST / 17:04:46 UTC | monitoring | Degradation affecting Actions mitigated. |
| 23:12:35 IST / 17:42:35 UTC | resolved | At peak, about 3% of workflow runs failed to queue; roughly 0.21% of in-progress runs failed. Cause: resource exhaustion in a database service that stores Actions Checks data. Bursts of highly concurrent, large read requests put the service under memory pressure, which caused it to stall and reject requests. Mitigated by failing over the affected database host. GitHub said it has increased memory capacity across the affected fleet and will limit concurrency of large queries. |

### 6. Degraded performance for multiple GitHub services - 5-6 November

- **Page:** <https://www.githubstatus.com/incidents/pfbmpxxfq1cy>
- **Shortlink:** <https://stspg.io/s5bs6jn41fwh>
- **Status-page impact / final status:** minor / resolved

**Note on windows:** the resolved post's impact window is 23:21 to approximately 23:26 UTC on 5 November (04:51-04:56 IST on 6 November) - about five minutes. The status incident was opened at 23:36:06 UTC (05:06:06 IST), after that window, and was resolved at 00:20:49 UTC (05:50:49 IST). Both are recorded; they are not the same interval.

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 04:51-~04:56 IST 6 Nov / 23:21-~23:26 UTC 5 Nov | - | Resolved post: elevated errors on Git operations, Issues, Actions, authentication, and API requests. Customers may have encountered failed requests and Git pushes; some Actions workflow runs were delayed. |
| 05:06:06-05:06:51 IST / 23:36:06-23:36:51 UTC | investigating | Investigating degraded performance for Actions and Pages, then several services including Git operations, Issues, Actions, and API access. |
| 05:28:28 IST / 23:58:28 UTC | investigating | Issues marked degraded. |
| 05:36:11-05:36:28 IST / 00:06:11-00:06:28 UTC 6 Nov | monitoring | Mitigated; seeing recovery. |
| 05:49:21-05:50:49 IST / 00:19:21-00:20:49 UTC | investigating, monitoring, resolved | Git Operations operating normally, then resolved. GitHub also wrote, during monitoring, that more details would be shared in its availability report. |

**Root cause (published):** a bug in database failover automation during planned maintenance. The automation stopped before completing the switch to a replacement database primary, leaving the existing primary unable to accept writes. Mitigated by restoring writes on the original primary; maintenance was then completed. GitHub said it is fixing the automation bug and strengthening pre-maintenance checks and recovery procedures.


### 7. Increase in timeouts for some unauthenticated users - 8 November (CI-adjacent)

- **Page:** <https://www.githubstatus.com/incidents/pyynvrm1vskw>
- **Shortlink:** <https://stspg.io/5yt31mfmyc6p>
- **Status-page impact / final status:** minor / resolved

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 16:06:05 IST / 10:36:05 UTC | investigating | Investigating reports of impacted performance for some GitHub services. |
| 16:46:19 IST / 11:16:19 UTC | investigating | Continuing to investigate degraded performance primarily for unauthenticated users. |
| 16:50:57 IST / 11:20:57 UTC | investigating | API Requests experiencing degraded performance. |
| 17:20:49 IST / 11:50:49 UTC | investigating | Identified a problematic component and working to redeploy it. |
| 17:26:20 IST / 11:56:20 UTC | investigating | API Requests mitigated. |
| 17:59:30-18:02:35 IST / 12:29:30-12:32:35 UTC | monitoring, resolved | Mitigated, then resolved. RCA not published. |

### 8. Increased timeouts due to surge in anonymous users - 9 November (CI-adjacent, CI jobs named)

- **Page:** <https://www.githubstatus.com/incidents/c9px7cw2ghrs>
- **Shortlink:** <https://stspg.io/jztczfplj6yb>
- **Status-page impact / final status:** minor / resolved

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 11:29:07 IST / 05:59:07 UTC | investigating | Investigating reports of impacted performance for some GitHub services. |
| 12:00:44 IST / 06:30:44 UTC | investigating | Debugging a spike in anonymous traffic. Some users who are not logged in may see errors on the site. **CI jobs and release downloads may fail with a 504 Gateway Timeout status if they make anonymous calls.** |
| 12:21:26-12:33:38 IST / 06:51:26-07:03:38 UTC | monitoring | Mitigated; 504s for anonymous traffic mitigated; continuing to observe. |
| 13:13:15 IST / 07:43:15 UTC | investigating | Repeated increase in 504 errors for anonymous and unauthenticated traffic, including users who are not logged in and release downloads. Continuing to investigate and apply mitigations. |
| 13:46:25 IST / 08:16:25 UTC | investigating | Continuing to observe high rates of 504 errors for anonymous requests. Logged-in users and authenticated requests are unaffected. |
| 14:17:55 IST / 08:47:55 UTC | investigating | Error rates have declined again; preparing targeted traffic controls. Logged-in and authenticated requests have not been affected. |
| 14:35:46-14:39:56 IST / 09:05:46-09:09:56 UTC | monitoring, resolved | Mitigated, then resolved as error rates remained low and additional protections were prepared. Detailed RCA not published. |

### 9. Incident with Webhooks and Actions - 9 November

- **Page:** <https://www.githubstatus.com/incidents/xxb3l2bm76dr>
- **Shortlink:** <https://stspg.io/4vdghm7tycm4>
- **Status-page impact / final status:** major / resolved

**GitHub's published windows:** delays for a small share from as early as 01:00 UTC (06:30 IST); main window 09:23-11:40 UTC (14:53-17:10 IST); mitigation 10:37 UTC (16:07 IST).

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| From 06:30 IST / 01:00 UTC | - | Resolved post: delays for a small share began as early as this time. The degraded Kafka broker had been slow since 7 November (see root cause). |
| 14:53 IST / 09:23 UTC | - | Resolved post: job-queue cluster degradation window starts. |
| 15:30:35 IST / 10:00:35 UTC | investigating | Investigating degraded performance for Actions and Webhooks. |
| 15:58:19 IST / 10:28:19 UTC | investigating | Investigating delays in Actions run start times and webhook deliveries. |
| 16:07 IST / 10:37 UTC | - | Resolved post: mitigated by removing the broker from service. |
| 16:17:25 IST / 10:47:25 UTC | investigating | Applied a mitigation and monitoring for recovery. |
| 16:36:35 IST / 11:06:35 UTC | investigating | Actions mitigated. Webhooks still degraded. |
| 16:47:28-17:12:04 IST / 11:17:28-11:42:04 UTC | investigating | Webhooks marked partial outage, then degraded performance. |
| 17:10 IST / 11:40 UTC | - | Resolved post: backlog cleared; published window ends. |
| 17:15:07-17:20:35 IST / 11:45:07-11:50:35 UTC | monitoring, resolved | Webhooks mitigated, then resolved post. |

**Root cause (published):** a degraded host had slowed one of the cluster's Kafka brokers since 7 November. The broker stopped reporting the metrics GitHub's alerts rely on, so GitHub did not identify it until it stalled the cluster on 9 November.

**Published follow-up:** an alert that pages GitHub within 30 minutes when a host slows a broker this way, so the broker can be removed before it affects customers.

**Published Actions behavior that matters operationally:** "Actions skips events older than 30 minutes, so some workflow runs did not start."

### 10. Incident with Actions and Checks API - 9 November (critical)

- **Page:** <https://www.githubstatus.com/incidents/tp8cghg7tlc4>
- **Shortlink:** <https://stspg.io/7yw63xl9ztrv>
- **Status-page impact / final status:** critical / resolved

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 23:37 IST 9 Nov / 18:07 UTC | - | Resolved post: Actions degraded performance begins. |
| 23:42:37 IST / 18:12:37 UTC | investigating | Investigating degraded availability for Actions. |
| 23:44:58 IST / 18:14:58 UTC | investigating | Actions marked `major_outage`. |
| 23:53:13-23:54:13 IST / 18:23:13-18:24:13 UTC | investigating | Webhooks marked degraded. Investigating elevated errors affecting GitHub Actions and the Checks API. |
| 00:00:17 IST 10 Nov / 18:30:17 UTC | investigating | Pages marked degraded. |
| 00:13:24 IST / 18:43:24 UTC | monitoring | Actions, Pages and Webhooks mitigated. |
| 00:29:32 IST / 18:59:32 UTC | resolved | Full resolved post: 28% average / 65% peak workflow-run impact; stuck jobs limiting concurrency; migration traffic-shift cause (see root-cause section). |

**Root cause (published):** during a planned migration of a core Actions database from GitHub's datacenter to a new cloud region, Actions service traffic was shifted to the new region after the migration completed. Request volume exceeded the capacity and rate limits configured there, so a portion of requests were rejected and workflow updates failed. Dependent services, including Webhooks, Pages, and Copilot code review, saw secondary impact.

**Mitigation (published):** some traffic was shifted back to the original region, then capacity and rate limits in the new region were increased before traffic was gradually shifted back. GitHub described this as a one-time migration event for this cluster that is now complete, and said it is improving capacity and limit validation, including shifting traffic in smaller steps. To address stuck runs, it is improving detection and mitigation and "readying an API to enable customers to address this themselves." **Unknown at snapshot time:** whether that customer API has shipped.

### 11. Incident with Git Operations and Pull Requests - 10 November (critical; Actions impacted)

- **Page:** <https://www.githubstatus.com/incidents/hnf21m9jkl3c>
- **Shortlink:** <https://stspg.io/16nsnk2th3rv>
- **Status-page impact / final status:** critical / resolved

**GitHub's published window:** approximately 15:00-17:55 UTC (20:30-23:25 IST), earlier than the first status update at 16:10:52 UTC.

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| ~20:30 IST / ~15:00 UTC | - | Resolved post: degraded performance affecting repository pages, pull requests, and GitHub Actions begins. |
| 21:40:52 IST / 16:10:52 UTC | investigating | Investigating degraded performance for Actions and Pull Requests. Actions component still listed as operational in this first update; Pull Requests marked degraded. |
| 21:56:37 IST / 16:26:37 UTC | investigating | Actions marked degraded; Pull Requests marked major outage. |
| 22:00-22:20 IST / 16:30-16:50 UTC | - | Resolved post: Actions delay rate peaked at about 7% in this interval. |
| 22:01:01 IST / 16:31:01 UTC | investigating | Elevated response times for Git operations and pull requests. GitHub Actions workflows triggered from Pull Requests are delayed. |
| 22:09:58 IST / 16:39:58 UTC | investigating | Actions marked partial outage. |
| 22:37:48 IST / 17:07:48 UTC | investigating | Some pull-request processing and Actions workflows are improving, but delays and failures persist. |
| 22:42:54 IST / 17:12:54 UTC | investigating | Pull Requests marked degraded performance. |
| 22:58:17 IST / 17:28:17 UTC | investigating | "Actions is operating normally." |
| 23:16:33 IST / 17:46:33 UTC | investigating | Actions workflows are recovering. Pull request processing has shown some improvement, but users continue to experience delays and failures. |
| 23:25:03 IST / 17:55:03 UTC | resolved | Full resolved post, including the 1.8% / 7% / 0.09% figures, stale source-branch commit display, and shared-infrastructure cause. |

**Root cause (published):** overload in the shared infrastructure that supports Git operations. Recent changes increased background processing and the work required to compute pull request merges, adding pressure to the same systems used by customer-facing requests.

**Mitigation (published):** disabling a recent merge-processing optimization, pausing background analysis, and temporarily limiting lower-priority traffic. Those temporary limits also restricted access to commit pages for users who were not signed in. Background processing continued to catch up after service health recovered.

**Published follow-ups:** reduced repeated repository-analysis work; adding workload-level concurrency controls to protect customer-facing requests; improving procedures for safely reducing background load; making pull request synchronization more resilient to temporary failures.

### 12. Incident with Webhooks - 11 November (brief; CI-adjacent)

- **Page:** <https://www.githubstatus.com/incidents/1h2wbly1y72t>
- **Shortlink:** <https://stspg.io/r0jrtx03yvll>
- **Status-page impact / final status:** minor / resolved

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 01:52:20 IST 12 Nov / 20:22:20 UTC 11 Nov | investigating | Investigating reports of degraded performance for Webhooks. |
| 01:56:20-01:56:23 IST / 20:26:20-20:26:23 UTC | monitoring, resolved | "Webhooks is operating normally." Then the standard resolved text, with an RCA promised and **not published**. |

**Speculation (not a fact):** this incident closed about 22 minutes before the next, critical, multi-product incident opened. GitHub has not published a link between them.

### 13. Impact across multiple GitHub products including Actions, Pages, and Copilot products. - 11 November (critical)

- **Page:** <https://www.githubstatus.com/incidents/dvy0t3mvh1lg>
- **Shortlink:** <https://stspg.io/jdjlq5zjyl48>
- **Status-page impact / final status:** critical / resolved
- **Root cause: Not published.**

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 02:18:12 IST 12 Nov / 20:48:12 UTC 11 Nov | investigating | Investigating reports of degraded availability for Pages. |
| 02:18:25 IST / 20:48:25 UTC | investigating | Actions experiencing degraded availability; Actions marked `major_outage`. |
| 02:18:46 IST / 20:48:46 UTC | investigating | Webhooks experiencing degraded performance. |
| 02:26:26 IST / 20:56:26 UTC | investigating | Pages experiencing degraded availability; Pages marked `major_outage`. |
| 02:26:49 IST / 20:56:49 UTC | investigating | Investigating issues impacting multiple GitHub products including Actions, Pages, and Copilot products. |
| 02:27:40 IST / 20:57:40 UTC | investigating | Copilot marked `major_outage`. |
| 02:37:24-02:37:26 IST / 21:07:24-21:07:26 UTC | monitoring | Degradation affecting Actions, Copilot, Pages and Webhooks mitigated. Recovery across all services; backlog of Actions jobs being processed. |
| 02:51:32-02:51:39 IST / 21:21:32-21:21:39 UTC | monitoring, resolved | Backlog of Actions jobs fully processed; full recovery observed. Standard resolved text: a detailed root cause analysis will be shared as soon as it is available. |

**Unknown:** cause, contributing factors, mitigation mechanism, percentage of failed or delayed workflow runs, and how large the backlog was. GitHub has not published those.

### 14. Incident with Webhooks API - 12 November (CI-adjacent)

- **Page:** <https://www.githubstatus.com/incidents/gl7yhd7jx00v>
- **Shortlink:** <https://stspg.io/ns57pv7lmynl>
- **Status-page impact / final status:** minor / resolved
- **Root cause: Not published.**

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 00:13:13 IST 13 Nov / 18:43:13 UTC 12 Nov | investigating | Investigating degraded performance for Webhooks. |
| 00:22:18 IST / 18:52:18 UTC | investigating | Webhooks API is experiencing timeouts and errors impacting about 1% of customer traffic. Working through possible mitigation strategies. |
| 00:50:08-00:50:14 IST / 19:20:08-19:20:14 UTC | investigating, monitoring | Traffic has recovered; marked mitigated. |
| 01:03:54 IST / 19:33:54 UTC | resolved | Standard resolved text; RCA promised and not published. |

### 15. Incident with Pull Requests - 13 November (CI-adjacent merge gate; open at snapshot, resolved at recheck)

- **Page:** <https://www.githubstatus.com/incidents/1xzmt89vv61l>
- **Shortlink:** <https://stspg.io/qzj7qjdcr8jc>
- **Status-page impact / final status:** major / resolved (resolved after the original snapshot)
- **Root cause: Not published.** The resolved post says a detailed root cause analysis will be shared as soon as it is available. None was present at the 14 November recheck.

| Time (IST / UTC) | Status | GitHub's statement |
| --- | --- | --- |
| 21:17:12 IST / 15:47:12 UTC | investigating | Investigating reports of degraded availability for Pull Requests. Component moved from operational to partial outage. |
| 21:19:36 IST / 15:49:36 UTC | investigating | Investigating an issue with Pull Requests where some attempts to merge are timing out. |
| 21:39:16 IST / 16:09:16 UTC | investigating | Pull Requests experiencing degraded performance. Component moved from partial outage to degraded performance. |
| 21:42:26 IST / 16:12:26 UTC | investigating | Mitigating action taken. Issue initially impacted 3% of requests and is now down to less than 1%. Continuing to monitor recovery. |
| 22:22:15 IST / 16:52:15 UTC | investigating | Continuing to investigate and monitor ~1% of merge requests timing out. This was the last update inside the original snapshot. |
| 22:58:01 IST / 17:28:01 UTC | monitoring | **Recheck:** the degradation affecting Pull Requests has been mitigated. Monitoring to ensure stability. |
| 23:16:11 IST / 17:46:11 UTC | monitoring | **Recheck:** continuing to monitor the stability of merge requests. |
| 23:59:31 IST / 18:29:31 UTC | resolved | **Recheck:** incident resolved. GitHub thanked customers for their patience and said a detailed root cause analysis will be shared as soon as it is available. No cause or final impact percentage was published in this post. |

Derived status-page duration (started_at 15:47:12 UTC to resolved_at 18:29:31 UTC): 2 hours 42 minutes. The resolved post did not publish a separate customer-impact window.


## Root cause and contributing factors

Published causes only. "Not published" means GitHub has not shipped an RCA or equivalent resolved-post cause as of the snapshot.

| Event | Published cause | Contributing factors GitHub stated | Still unknown |
| --- | --- | --- | --- |
| 3 Nov Git HTTPS | Excess connections on Git HTTPS hosts saturated network capacity | Mitigation side effect: a load-balancer block later had to be removed because it broke some third-party integration users | Which client traffic pattern was blocked, and which integration, were not named |
| 4 Nov shared database | A shared database reached capacity | None further published | Which database, what drove it to capacity, and the Actions-specific failure rate |
| 4 Nov event routing (two waves) | Configuration change to an internal event-routing service paused several event streams because feature flags were not set up as intended, and the service did not handle that gracefully | Rollback reverted configuration but not the code change, so a later deploy reintroduced the fault | Flag names, service name, and exact deploy identity are unpublished (and not needed for this draft) |
| 5 Nov Actions Checks DB | Resource exhaustion / memory pressure on the database that stores Actions Checks data, from bursts of highly concurrent large reads | - | What issued the large reads |
| 5-6 Nov multi-service | Bug in database failover automation during planned maintenance; failover stopped before the new primary was in service, leaving the existing primary unable to accept writes | Planned maintenance | The automation bug itself is not described beyond that failure mode |
| 8 Nov unauthenticated timeouts | Not published | Mid-incident: a "problematic component" was being redeployed | Cause, component, CI impact |
| 9 Nov anonymous 504s | Not published as a formal RCA. Updates describe a surge in anonymous traffic and 504s for anonymous/unauthenticated requests | Recurrence after the first mitigation | Why the surge happened; CI failure count |
| 9 Nov Webhooks and Actions | Degraded host slowed a Kafka broker in a job-queue cluster since 7 November; the broker stopped reporting the metrics alerts rely on | Alerting gap delayed detection until the cluster stalled on 9 November | Host/broker identifiers (unpublished, not required) |
| 9 Nov Actions and Checks API | After a planned core Actions database migration, traffic shifted to a new cloud region exceeded capacity and rate limits configured there | Dependent services saw secondary impact | Exact limits and the size of the stuck-job set |
| 10 Nov Git / PRs / Actions | Overload in shared infrastructure that supports Git operations, after changes increased background processing and pull-request merge computation | Customer-facing traffic shared that infrastructure with background work | Precise subsystem name beyond "shared infrastructure" |
| 11 Nov brief Webhooks | Not published | - | Cause; any relationship to the following critical incident (**speculation** if asserted) |
| 11 Nov multi-product critical | Not published | - | Cause, mitigation, and impact percentages |
| 12 Nov Webhooks API | Not published | - | Cause |
| 13 Nov PR merge timeouts | Not published (resolved at recheck; RCA promised, not posted) | Updates published 3%, then <1%, then ~1% timeout rates; Actions not listed | Cause, whether Actions or checks were indirectly affected, and a final official impact figure |

---

## Detection and response

This section is about two layers: what GitHub published about its own detection, and what a dependent team can actually detect.

**Facts from GitHub's own account:**

- Several resolved posts give an impact start earlier than the first public status update (3 Nov Git HTTPS; 4 Nov database capacity; 4 Nov event routing; 5 Nov Checks DB; 5-6 Nov failover; 9 Nov Kafka/webhooks; 9 Nov Actions migration; 10 Nov Git/PR/Actions). A team that alerts only when githubstatus.com opens an incident will systematically come in late on this month's pattern.
- On 9 November, GitHub said a Kafka broker had been slow since 7 November and had stopped reporting the metrics its alerts relied on, so the fault was not identified until the cluster stalled. That is a published detection gap, not an inference.
- On 9 November (migration), Actions recovered while some jobs remained stuck and continued to consume concurrency until released. "Actions is operational" is not the same as "our queues and concurrency are healthy."
- On 10 November, about 0.09% of pull-request events never started a workflow and needed a retrigger. Those will not show up as failed runs if the run does not exist.
- On 9 November (Kafka), Actions skipped events older than 30 minutes, so a late-draining webhook backlog does not self-heal for workflow start.
- On 11 November, GitHub treated job-backlog drain as a separate condition after services were marked recovered, and only resolved the incident once it said the backlog was fully processed.

**Inference for a dependent team:**

- Detect on *your* signals, not only the vendor status page: workflow start latency, jobs queued but not picked up, checks that never appear on a PR, webhook delivery lag (if you have it), clone/fetch failure rates (especially unauthenticated HTTPS), 504s on release asset downloads, and merge API timeouts.
- After vendor recovery, explicitly hunt for two leftovers: runs that never started (retrigger), and runs that started but are stuck (concurrency leak).
- Prefer authenticated Git and API access in CI. GitHub said anonymous CI jobs and release downloads may 504, and that SSH Git was unaffected during the 3 November HTTPS degradation.
- Keep a merge-freeze / ship-freeze path that can key off Pull Requests degradation even when Actions is listed as operational (the 13 November merge-timeout incident, which did not list Actions, is the example).

**What is not published:** GitHub's internal time-to-detect for most of these incidents, except the Kafka alerting gap described above. Customer-side time-to-detect is likewise unpublished here; this draft has no internal telemetry to add.

---

## What went well / poorly

Everything in this section is **inference**, grounded in GitHub's published statements. It is not GitHub's self-assessment, and it is not a claim about any one customer's experience.

**What went well (inference):**

- Most Actions-impacting incidents received a concrete resolved post with a cause, a mitigation, and at least some numeric impact (the 4, 5, 9 and 10 November Actions events). That is usable evidence for playbooks.
- Several mitigations were relatively fast once started: failover of the Checks database, rolling back the event-routing change, shifting migration traffic back, removing the bad Kafka broker, restoring writes on the original primary.
- GitHub stated specific preventive work (flag pre-deploy checks, Kafka host-slow alerts within 30 minutes, concurrency limits on large Checks reads, workload-level concurrency controls, a forthcoming customer API for stuck runs). Those are commitments, not proof they are already in place.
- The 3 November Git incident explicitly scoped impact to HTTPS and stated SSH was unaffected, which is an actionable split for CI checkout.

**What went poorly (inference):**

- November still produced three critical Actions-involved incidents in three consecutive UTC days (9, 10, 11 November), plus two major Actions waves on 4 November, one of them a reintroduction after an incomplete rollback.
- Detection lagged impact start on multiple events, and on the Kafka incident lagged by days because the broker stopped reporting the metrics alerts used.
- Recovery of the Actions *component* repeatedly left residual customer work: skipped events older than 30 minutes, 0.09% of PR events that never started, stuck jobs holding concurrency, and a post-mitigation Actions job backlog on 11 November.
- The 11 November critical incident, the 12 November Webhooks API incident, the 8-9 November anonymous/unauthenticated timeout incidents, and the 13 November merge-timeout incident (open at snapshot, resolved at recheck) have no published root cause. A dependent team cannot yet truthfully tell customers "this was caused by X."
- Vendor mitigations had side effects: unauthenticated commit-page limits on 10 November; a load-balancer block that broke some third-party Git HTTPS users until 13:26 UTC on 4 November.

---

## Action items for a team that depends on GitHub Actions

These are recommended internal actions. They are **inferences** from the published incidents, not GitHub-assigned work.

1. **Status-page alerting is necessary and insufficient.** Subscribe to [GitHub Status](https://www.githubstatus.com/) webhooks or email/SMS, but also alert on first-party signals: workflow time-to-start, checks missing on a PR after a grace period, clone/fetch error rate, merge API timeouts, and webhook ingress lag.
2. **Build a retrigger playbook for runs that never started.** GitHub published both "Actions skips events older than 30 minutes" and a 0.09% no-start rate on 10 November. Waiting for a failed check will miss these.
3. **After Actions recovers, drain stuck runs and measure concurrency.** The 9 November migration left jobs that limited available concurrency until released. Confirm reserved / in-progress counts return to baseline before calling CI healthy.
4. **Authenticate CI Git, API, and release-download traffic.** On 9 November GitHub warned that anonymous CI jobs and release downloads may 504. On 3 November, SSH Git was unaffected while HTTPS was degraded. Prefer `GITHUB_TOKEN` / a bot identity, and keep SSH checkout as a fallback.
5. **Retry policy that is safe under backlog.** Retries should be idempotent, bounded, and aware that a late retry will not recreate an event Actions has already dropped. Prefer explicit `workflow_dispatch` / `gh run rerun` over depending on the original webhook.
6. **Fallback runners for the critical path.** Self-hosted or alternate-provider runners do not remove dependence on the Actions control plane, Checks API, or checkout, but they reduce exposure to hosted-runner queue delay when the control plane is up and only job execution is slow. **Unknown:** how much of this month's impact was runner capacity versus control-plane or event routing; GitHub did not break that out except where it named queues, Checks, or event routing.
7. **Merge-freeze policy independent of Actions status.** If Pull Requests or Git Operations are degraded, freeze merges and releases even when Actions is `operational`. Revisit the freeze only after merge success rate and PR head-SHA freshness are confirmed (GitHub said some PRs kept showing an earlier source-branch commit after the 10 November degradation ended).
8. **Pages and webhook consumers get their own health checks.** Pages volume fell to about 4% of normal and webhook deliveries to about 58% on 4 November; webhook delay peaked at 30% on 9 November. Do not infer Pages or webhook health from a green Actions component.
9. **Treat GitHub migrations and status "investigating" as a change window.** The 9 November critical incident was a planned database migration plus a traffic shift that exceeded new-region limits. If a ship is not urgent, delay it while a critical or major Actions, Git, or PR incident is open, and while an unpublished-RCA critical incident is still recent.
10. **Do not ship a "GitHub was down" customer statement from a bot.** Impact percentages vary widely by incident (0.09% non-starts versus a 65% peak). Quote only a cited incident page, or wait for GitHub's resolved post.


## Release-readiness checklist

Run this immediately before shipping a fix, including a fix written in response to CI breakage. Tick only from evidence gathered in this change window.

1. **Vendor status:** Read <https://www.githubstatus.com/> and any open incident pages. Record component states for Actions, Git Operations, Pull Requests, Webhooks, Pages, API Requests, Packages, and Codespaces. If Actions, Git Operations, or Pull Requests are not operational, default to no-ship unless a human overrides (item 14).
2. **Unresolved RCA debt:** If a recent critical or major Actions-involved or merge-path incident still has **Not published** as its root cause (true of the 11 November critical incident, and of the 13 November Pull Requests incident, at recheck), say so explicitly in the go/no-go note. Do not invent a cause.
3. **CI is green on the fix SHA:** Required checks have passed on the exact commit you intend to ship, not on an earlier commit of the same branch. Re-check that the PR head SHA matches that commit (stale PR metadata was a published 10 November symptom).
4. **Missing runs, not just failed runs:** Confirm every expected workflow actually started. If a GitHub incident involved dropped events or a backlog, retrigger any missing workflows before declaring green.
5. **Rerun flakes and leftovers:** Rerun failed or flaky jobs once on the fix SHA. After a vendor incident, also inspect queued, waiting, or cancelled jobs that may be holding concurrency.
6. **Authenticated, retry-safe path:** Checkout, API calls, and release-asset downloads in the shipping workflows use authenticated requests. Retries are idempotent. SSH or a documented fallback exists for Git if you rely on HTTPS.
7. **Rollback plan:** Written and tested enough to execute without the current branch: previous artifact or image, revert commit or previous release, and (if used) a flag-off path. The rollback must revert the actual failing change, not only a config wrapper. GitHub's 4 November reintroduction is the cautionary case.
8. **Feature flag or equivalent gating:** The fix can be disabled without a full revert, or you have explicitly accepted that it cannot and have documented why.
9. **Canary or staged rollout:** Ship to a small slice or a non-critical environment first. Watch a pre-agreed set of signals for a pre-agreed bake time before widening.
10. **Monitoring and alerts:** Dashboards and alerts for the changed path are live (error rate, latency, and, for CI-platform changes, workflow start time and missing-check rate). On-call knows what will fire.
11. **Merge and release path health:** A test merge or dry-run release against current GitHub PR/merge APIs succeeds. If merges are timing out, stop.
12. **Communications draft:** Internal note and, if needed, a customer note are drafted but **not sent**. They cite only published GitHub wording and do not assert a cause GitHub has not published.
13. **Blast radius and owners:** Service owners, on-call, and a rollback owner are named and reachable. The change window does not collide with a known GitHub migration or an open major/critical incident unless item 14 explicitly accepts that risk.
14. **Change approval / go-no-go:** A human records ship, wait, or abort, with the evidence from items 1-13. This step is not automated (see the next section).
15. **Post-ship watch:** A human or a tightly scoped automated watch stays on the canary signals through the bake window, with authority to roll back.

---

## Which steps still need a human to sign off

### Require a human

| Step | Why a human still has to sign |
| --- | --- |
| **Go / no-go to ship** | This is a risk acceptance, not a threshold. Green CI on a fix SHA can coexist with an open PR-merge incident, an unpublished critical RCA, or a backlog GitHub has not fully described. A person has to weigh urgency against that residual vendor risk. |
| **Merge-freeze and freeze-lift** | Freezing (and especially unfreezing) changes delivery commitments. Lifting after "Actions is operating normally" would have been premature on 10 November, when PR processing was still failing and some PRs showed a stale head commit. |
| **Rollback decision** | Automated rollback is fine as a *proposal* or as a pre-agreed safety trip. The decision to take production backwards, and to confirm the rollback actually removes the failing change, needs a person who can see side effects. |
| **Customer-impact statements** | Impact numbers this month ranged from 0.09% non-starts to a 65% peak of affected workflow runs, and several incidents have no published cause. Auto-posted blame or percentages will be wrong or unfair. |
| **External communications** | Status tweets, customer emails, and support macros are attributable speech. Drafts can be prepared automatically; sending cannot. |
| **Root-cause acceptance of our fix** | A human has to accept that the fix addresses the failure mode you actually saw, and that you are not shipping a coincidental change during a vendor incident. Vendor RCA, when unpublished, cannot be filled in by a model. |
| **Change approval for production** | Authorization and accountability stay with a named owner, especially when the checklist has exceptions (shipping during an open GitHub incident, skipping a canary, no feature flag). |
| **Accepting residual risk when GitHub's RCA is still "Not published"** | The 11 November critical incident, and the resolved-but-unexplained 13 November merge-timeout incident, are the current examples. Only a person can decide whether "backlog cleared, cause unknown" is an acceptable environment in which to ship. |

### Safe to automate (with a human-visible audit trail)

| Step | Automation is appropriate because |
| --- | --- |
| Polling GitHub Status and opening an incident ticket or chat thread when Actions, Git, PRs, Webhooks, or Pages change state | Mechanical, and it should be faster than a human refreshing the page. It must not be the only signal. |
| Confirming required checks are green on the exact ship SHA, and flagging missing expected workflows | Deterministic against the Checks / Actions APIs. |
| Retriggering failed, dropped, or stale jobs under a documented policy after vendor recovery | Repeatable, and it directly addresses published non-start and backlog behavior. A human should still see the list. |
| Enforcing authenticated checkout / download in workflow templates, and failing the workflow if a job is anonymous | Policy as code. |
| Canary metric gates that pause a rollout when pre-agreed error or latency thresholds trip | Safe if the alternative is a human override, not an automatic widen. |
| Drafting (not sending) internal and external comms from a template that only includes cited status-page text | Draft generation is reversible. Sending is not. |
| Collecting the checklist evidence pack (status snapshot, check URLs, SHA, flag state, monitoring links) | Speeds the human go/no-go; does not replace it. |

**Inference:** automate evidence collection, detection, retriggers, and safety trips. Keep humans on decisions that assert cause, accept risk, change customer-visible state, or speak for the team.

---

## Gaps and unknowns

- **No November 2026 GitHub Availability Report** was published as of the snapshot. The latest report is [October 2026](https://github.blog/news-insights/company-news/github-availability-report-october-2026/), posted 12 November 2026. It is outside this window. It does discuss Actions-related reliability work in October (capacity for Azure control-plane requests, a NAT-gateway mitigation, and moving a high-traffic Actions endpoint to a more efficient path) and states that October had eight incidents with degraded performance. Those October incidents are not used as evidence in the timelines above.
- **Root cause not published** for the 11 November critical multi-product incident, the 11 November brief Webhooks incident, the 12 November Webhooks API incident, the 8 November unauthenticated-timeout incident, the 9 November anonymous-504 incident (no formal RCA), and the 13 November Pull Requests incident (resolved at recheck, RCA still not published).
- **Status-page start versus published impact start** differ on multiple incidents, sometimes by hours (3 November Git HTTPS; 10 November Git/PR/Actions; early 9 November webhook delays from 01:00 UTC). Customer-visible start may be earlier than the first public update; it is not known how much earlier in cases GitHub did not quantify.
- **No Packages or Codespaces incidents** appear in the November dataset. That is not evidence those services had no unpublished issues; it is only evidence none were posted on the status page.
- **Hosted-runner versus control-plane split** is generally unpublished. Where GitHub named a cause, it was event routing, Checks data, queue/Kafka, database capacity or failover, migration rate limits, or shared Git infrastructure - not a published hosted-runner shortage.
- **Customer-level impact** (how *your* org's workflows behaved) is not in the public record and is not estimated here.
- **The 13 November Pull Requests incident was still open** at the original snapshot (22:22:15 IST / 16:52:15 UTC), last update then `investigating`, with ~1% of merge requests timing out. At the 14 November recheck it was **resolved** at 23:59:31 IST / 18:29:31 UTC on 13 November. Root cause was still not published.
- **Stuck-run customer API** announced on 9 November was described as "readying," not as generally available. Current availability is unknown.
- The incidents API retains a rolling set of recent incidents (50 at snapshot). Coverage of 1-14 November 2026 was complete in the recheck payload (21 November incidents). Older history was not re-fetched beyond the API's 50-incident window.

---

## November incidents not used as CI evidence

These were posted in the same window but are out of CI scope unless a later RCA says otherwise. Listed for completeness:

- [Elevated errors on GPT-5.4 Nano](https://www.githubstatus.com/incidents/db4x8hh362t8) - 2 November, minor, Copilot model provider, resolved. Upstream model-provider issue.
- [Incident with API, Git, and Copilot requests](https://www.githubstatus.com/incidents/rxlj89wpk7px) - 3 November, minor, resolved. The title includes "Git," but the updates marked Copilot, Issues, and API Requests, and no RCA was published. It is not treated as a CI incident here.
- [Increased task-related errors for Copilot Mission Control](https://www.githubstatus.com/incidents/tc28c0wzrnpz) - 4 November, minor, resolved.
- [Degraded availability for GPT-5.3 Codex and GPT-5.6 models in Copilot](https://www.githubstatus.com/incidents/6jm5k16vyhfg) - 9-10 November, major, resolved.
- [Disruption with Email Delivery](https://www.githubstatus.com/incidents/nwbjzl7nfrc2) - 12 November, minor, resolved. Sign-in, verification, password reset, and notification email; an upstream email provider was named. Not treated as CI.
- [Copilot experiencing degraded model availability for some GPT models](https://www.githubstatus.com/incidents/51drrqvsv66g) - 13-14 November, minor, resolved after the snapshot. Upstream model-provider issue. Not treated as CI.

---

## Appendix: incident index (November 2026 through the 14 November recheck)

| # | Title | UTC date (start) | Impact | Status at snapshot | URL |
| --- | --- | --- | --- | --- | --- |
| 1 | Elevated errors on GPT-5.4 Nano | 2 Nov | minor | resolved | https://www.githubstatus.com/incidents/db4x8hh362t8 |
| 2 | Incident with API, Git, and Copilot requests | 3 Nov | minor | resolved | https://www.githubstatus.com/incidents/rxlj89wpk7px |
| 3 | Incident with Git Operations | 3 Nov | minor | resolved | https://www.githubstatus.com/incidents/13b6lyl8d7bz |
| 4 | Increased task-related errors for Copilot Mission Control | 4 Nov | minor | resolved | https://www.githubstatus.com/incidents/tc28c0wzrnpz |
| 5 | Incident with Pull Requests, Git Operations and Issues | 4 Nov | major | resolved | https://www.githubstatus.com/incidents/smtdswd1h2w4 |
| 6 | Incident with Actions and Pages | 4 Nov | major | resolved | https://www.githubstatus.com/incidents/xr3myzj7ms51 |
| 7 | Incident with Actions | 4 Nov | major | resolved | https://www.githubstatus.com/incidents/h02n9572mx9y |
| 8 | Incident with Actions | 5 Nov | minor | resolved | https://www.githubstatus.com/incidents/t89g2q1800fr |
| 9 | Degraded performance for multiple GitHub services | 5 Nov | minor | resolved | https://www.githubstatus.com/incidents/pfbmpxxfq1cy |
| 10 | Increase in timeouts for some unauthenticated users | 8 Nov | minor | resolved | https://www.githubstatus.com/incidents/pyynvrm1vskw |
| 11 | Increased timeouts due to surge in anonymous users | 9 Nov | minor | resolved | https://www.githubstatus.com/incidents/c9px7cw2ghrs |
| 12 | Incident with Webhooks and Actions | 9 Nov | major | resolved | https://www.githubstatus.com/incidents/xxb3l2bm76dr |
| 13 | Incident with Actions and Checks API | 9 Nov | critical | resolved | https://www.githubstatus.com/incidents/tp8cghg7tlc4 |
| 14 | Degraded availability for GPT-5.3 Codex and GPT-5.6 models in Copilot | 9 Nov | major | resolved | https://www.githubstatus.com/incidents/6jm5k16vyhfg |
| 15 | Incident with Git Operations and Pull Requests | 10 Nov | critical | resolved | https://www.githubstatus.com/incidents/hnf21m9jkl3c |
| 16 | Incident with Webhooks | 11 Nov | minor | resolved | https://www.githubstatus.com/incidents/1h2wbly1y72t |
| 17 | Impact across multiple GitHub products including Actions, Pages, and Copilot products. | 11 Nov | critical | resolved | https://www.githubstatus.com/incidents/dvy0t3mvh1lg |
| 18 | Incident with Webhooks API | 12 Nov | minor | resolved | https://www.githubstatus.com/incidents/gl7yhd7jx00v |
| 19 | Disruption with Email Delivery | 12 Nov | minor | resolved | https://www.githubstatus.com/incidents/nwbjzl7nfrc2 |
| 20 | Incident with Pull Requests | 13 Nov | major | resolved (was investigating at snapshot) | https://www.githubstatus.com/incidents/1xzmt89vv61l |
| 21 | Copilot experiencing degraded model availability for some GPT models | 13 Nov | minor | resolved (after snapshot) | https://www.githubstatus.com/incidents/51drrqvsv66g |

*Rows 1-20 were compiled from the GitHub Status API snapshot at 22:22:15 IST / 16:52:15 UTC on 13 November 2026. Row 20's status and row 21 were updated from the recheck at about 3:05 PM IST / 09:35 UTC on 14 November 2026 (page `updated_at` 05:54:27 UTC / 11:24:27 IST).*
