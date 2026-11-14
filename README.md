# github-ci-november-2026

Unofficial, independent analysis of public GitHub Status incidents in November 2026, focused on CI. That means GitHub Actions, plus adjacent surfaces that can stop a ship even when Actions looks fine: Git operations, webhooks, pull-request merges, Pages, and anonymous checkout or release downloads.

This is a reading of public status pages and incident posts. It is not a GitHub postmortem, it uses no insider information, and it is not written for or on behalf of any employer.

**Suggested repository:** [github.com/jyotirawattech/github-ci-november-2026](https://github.com/jyotirawattech/github-ci-november-2026)

## What's in this repository

- [POSTMORTEM.md](POSTMORTEM.md) is the full document. It has the timeline, published impact figures, labelled inferences, sources, a release-readiness checklist, and a list of steps that still need a human to sign off.
- [CHECKLIST.md](CHECKLIST.md) is the same readiness checklist as GitHub task-list checkboxes, written so a team can run it before shipping a fix.
- [LICENSE](LICENSE) covers the documentation in this repository under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## How to use the checklist

Run [CHECKLIST.md](CHECKLIST.md) immediately before shipping a fix, including a fix written in response to CI breakage. Tick an item only from evidence gathered in this change window, and keep the evidence (status snapshot, check URLs, ship SHA, monitoring links) next to the ticks.

1. Open GitHub's live status page and any open incident pages, not a cached summary.
2. Confirm required checks passed on the exact commit you intend to ship, and that every expected workflow actually started. A missing run will not show up as a failed check.
3. After a vendor incident, retrigger dropped runs and look for stuck jobs that may still be holding concurrency.
4. Confirm the rollback reverts the failing change itself, not only a configuration wrapper.
5. Stop if merges are timing out, even if Actions is listed as operational.
6. Leave go/no-go, merge-freeze, rollback, customer-facing wording, and residual-risk acceptance to a named human. The last section of the checklist is for those signatures.

The reasoning, figures, and citations behind each item are in [POSTMORTEM.md](POSTMORTEM.md).

## Snapshot and recheck

The first-pass timeline was compiled from GitHub's public incident API at 10:22:15 PM IST / 16:52:15 UTC on 13 November 2026 (Asia/Calcutta, UTC+5:30).

A recheck of the same public sources at about 3:05 PM IST / 09:35 UTC on 14 November 2026 found:

- The [13 November Pull Requests incident](https://www.githubstatus.com/incidents/1xzmt89vv61l) had been resolved at 11:59:31 PM IST / 18:29:31 UTC on 13 November. GitHub still had not published a root cause.
- One new incident had appeared, a [Copilot model-provider degradation](https://www.githubstatus.com/incidents/51drrqvsv66g). It is out of CI scope.
- No older November incident had gained a new root-cause post. The [November 2026 availability report](https://github.blog/news-insights/company-news/github-availability-report-november-2026/) was still unpublished (HTTP 404).
- No incidents were open. The CI-primary count was unchanged: nine Actions-listed incidents, eight distinct events, out of 21 November incidents in the API.

## Sources

- GitHub Status: <https://www.githubstatus.com/>
- Incidents API: <https://www.githubstatus.com/api/v2/incidents.json>
- Unresolved incidents: <https://www.githubstatus.com/api/v2/incidents/unresolved.json>
- History: <https://www.githubstatus.com/history>
- Latest published monthly report at recheck: [GitHub availability report: October 2026](https://github.blog/news-insights/company-news/github-availability-report-october-2026/)

Individual incident URLs are cited in [POSTMORTEM.md](POSTMORTEM.md).

## License

Documentation in this repository is licensed under Creative Commons Attribution 4.0 International. See [LICENSE](LICENSE).
