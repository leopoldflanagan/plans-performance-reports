# Plans Performance Reports

Delivery metrics for the Plans team, rebuilt from Jira every 15 minutes and
published as a static site. Same method as the EDW and Database Services sites;
what differs is in `config/team.json`, not in the code.

## What is different about Plans

**Two pods on one board.** Plans is the only team at Wellfit whose board carries
two delivery pods: Plans and Launch Loyalty. Board 15 also carries EDW's sprints,
Platform's and QA's, and more than one sprint can be active at the same time. A
sprint belongs to a pod by how it is named, because nothing else in Jira
separates them. That mapping is in `config/team.json` under `pods`.

Plans has used six sprint naming schemes since 2024 — `Plans Sprint 0`,
`Plan-R8.12-S1`, `R8.26 S1`, `Plans-R8.27 S1`, `Plans-Sprint 6-26` — so the pod
lists several prefixes and the longest match wins.

**Work that belongs to no pod.** In release 9.08, 371 stories and bugs closed:
124 in Plans sprints, 129 in LL sprints, and 104 — 28% — in no sprint at all.
That third figure is reported as itself. It is either work that bypasses sprint
planning or a Jira hygiene gap, and folding it into either pod would hide which.

**The combined view has a limit.** Throughput, cycle time and the step breakdown
pool across both pods, because they are counts over a release window. Sprint
metrics — commitment, scope change, spillover, burndown — do not: the two pods'
sprints do not share dates, so a merged burndown would be an invention. Those are
reported per pod.

**No reactive label.** Plans applies none: zero uses across a whole release. The
Planned vs Unplanned measurement is therefore reported as unavailable, with that
reason, and the front page does not list it among the metrics. It is not 0%.

**No capacity page and no sign-off register yet.** Those panels report as
unavailable until the Confluence pages exist and their ids go into
`config/team.json` under `pages`.

**A full QA and staging pipeline.** Unlike EDW and DS, Plans moves work through
`Ready for QA`, `In QA`, `Ready for Stage`, `In Stage`, `In PreProd` and
`Ready Prod`. The time-in-status breakdown is where this site earns its keep:
it shows which step the time actually goes to.

## Where things live

| | |
|---|---|
| `config/team.json` | everything specific to Plans. Start here. |
| `scripts/fetch_jira.py` | reads Jira and Confluence, writes `data/` |
| `scripts/build_reports.py` | turns `data/` into the pages |
| `scripts/teamconfig.py` | loads and validates the config |
| `data/frozen.json` | measured periods, never recomputed once closed |
| `data/current.json` | the period in progress |
| `data/pending.json` | periods that ended and still need closing |

See `SETUP.md` for the token and the scheduled job.
