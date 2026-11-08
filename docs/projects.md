# Projects

The fleet's own building blocks, one section each. Plans that are not built yet are on the [plans](plans.md) page.

**Contents**

- [Monitoring](#monitoring)
- [Panoptes, the health engine](#panoptes-the-health-engine)
- [The command filter](#the-command-filter)
- [Aion, an evidence-graded atlas of history](#aion-an-evidence-graded-atlas-of-history)
- [Backups and restore](#backups-and-restore)
- [The local language model](#the-local-language-model)
- [Reporting tools](#reporting-tools)
- [Smaller threads](#smaller-threads)

## Monitoring

**Layers.** (A fuller description with the log store and the notification path is in [Monitoring and the log store](monitoring-and-logs.md).)

1. **Uptime monitoring.** Around a hundred monitors in a self-hosted Uptime Kuma instance: HTTP and keyword checks, pings, and "push" monitors that fire when a job fails to check in.
2. **Guards.** Every seat runs recurring jobs through a small wrapper that records a heartbeat and the job's verdict. A *guard* can be alive-and-green, alive-and-red, or stale. Each state is shown differently, and a guard that has not reported is never read as healthy.
3. **A security log store** that collects firewall, intrusion-prevention and DNS-filter events, with category-level switches that were tuned to keep volume useful.
4. **A health engine** (below) that judges the whole fleet from an independent vantage point.

**Principles that came out of use.**

- **Silence must be distinguishable from health.** A monitor whose own feed is blind reports *unmeasurable*, a state that is red-adjacent and visible, not green.
- **A check that shares fate with what it checks verifies nothing.** A seat's opinion about itself is evidence, not a verdict. The register compares what a seat claims with what an independent source says.
- **Alerts go to a separate address** from the one the owner reads for conversation. The owner does not want infrastructure mail in his normal inbox, and that decision is respected in the routing, not just in a note.
- **Measure the delivery path of an alert**, not just whether the alert fires. A notification that reaches nobody is a monitor that does not exist.

## Panoptes, the health engine

Named after the hundred-eyed watchman. It reads an **expected set** (which seats and watchers should exist and what state they should be in), judges every watcher and seat from a vantage point other than its own, acts where it is allowed to, confirms the result, and surfaces a short per-agent list of what needs attention.

Its design has nine hard edges and four states. Two of the states matter most: *gone* (a thing that used to report and stopped) and *never seen* (a thing the register expects but that has never reported). Treating those two the same hides the most dangerous failure, a watcher that was never actually installed.

A **pre-acceptance gate** of seven measurements must all pass before the engine counts as accepted. The gate is run nightly across the entire test population and publishes a status page. When the gate cannot finish in its time limit, the page says **unmeasurable** rather than green or red, and this has happened: see the lesson on creeping run time.

## The command filter

Every shell command an agent runs passes through a filter that refuses dangerous commands before they execute (destructive git operations, recursive deletes, writes to credential files, and so on). The fleet builds on an open-source tool, [destructive_command_guard](https://github.com/Dicklesworthstone/destructive_command_guard), written in Rust, and layers its own rule packs on top.

A few properties of how it is run:

- **Fail closed.** If the filter cannot reach a verdict in time, the command is denied, not allowed.
- **A rollout gate for new filter releases.** A tool compares the old and new binaries on a corpus of commands and reports every command whose verdict changed. Each change must be judged by a reviewer; an unexpected one turns the gate red.
- **Human-readable denials with an appeal path.** A denial names its rule and says where to appeal. There is no self-opened window for the most sensitive zones (credential files), because a window the agent opens itself is exactly the self-approval the zone exists to stop.
- **Zones are armed per seat after a dry-logging period.** A new protected zone first only logs what it would have refused. After a measured stretch the log is read and the zone is armed. A recent example: a zone was armed after a dry-logging period of about a week and a half showed one deliberate edit and none to the credential files. Which zones are armed on which seat is deliberately not published: that is a security detail, not a design.

## Aion, an evidence-graded atlas of history

Aion is a map-and-timeline atlas of historical sites, events and processes. It has its own public page, with the story, the evidence rules and how to help: <https://github.com/Mbrilman/aion-time-globe>. Its rules are what make it interesting:

- **Every entry must be real and sourced.** Entries are checked against the actual pages they cite, and an unverifiable statement is removed, not softened. A photo-verification project found five failure modes in automated captioning (invented names, misread text, category anchoring, self-contradiction, self-reference) and each got a mechanical lint.
- **Dating carries a grade.** Dates are marked mainstream, disputed or speculative, per date. The grade of an *identification* (is this the place the text names?) is separate from the grade of its *date*.
- **Neutrality rules for every tradition.** Facts and sources only. Where a place's meaning is a claim of a tradition, the text says whose. No ranking, no endorsement, no dismissal. The same two-source rule and the same grade rule apply to every tradition.
- **Tags drive filters.** Sites carry tags from one closed vocabulary that a linter enforces. A filter for places of worship is live on the atlas, and more places are being added in groups; it groups places by what the place is, whose it is, how sure the identification is, and which text names it. A tag never claims more than the row's own text says.
- **Layers are cheap to add and expensive to get right.** The drowned rivers of a submerged landscape are live as a layer, and royal roads, ancient-DNA finds and pilgrimage routes are planned, each with its own data rules (for example, a named individual find is allowed only when it is old, famous, published once, and has no descendant-community objection on record).

Every change to the atlas follows the review rules above, and a browser-based test suite can run the real page in a headless browser; it needs a served tree and a dedicated environment, so it runs on changes and in the nightly battery rather than being a constant background check.

### How an Aion entry is built

Research helpers find candidate sources and report quotes with their addresses; the owner of the row then fetches every page raw and tests each sentence as an exact string. Photos go through a licence read and a visual check. A dates test and the data checker run on every change, a browser test runs the page, and a change is restamped before it is pushed so that visitors never see a stale cached file. That pipeline, not any one agent's judgment, is what the grades rest on.

## Backups and restore

Daily snapshots on the NAS and a weekly off-site bundle. The restore order is written down and has been performed: clone the canon *before* trusting anything installed on a machine, because most tools on each seat are symlinks into that clone.

## The local language model

A language model runs on a spare PC and is used for light language work. Two uses are worth naming:

- **A free first draft** for summaries of logs that feed the daily report. When the model returns nothing, the raw source is passed on and the report says so; there is deliberately no silent fall-back to a paid model. The model is off by default and switched on on request.
- **A second, cheaper reader** for verification work, listed under a routing table with the paid models so the choice is explicit.

## Reporting tools

A set of small tools builds the daily and weekly reports from the worklog, the message archive, the nightly self-improvement ("dream") summary, the drift audit, the fleet status and the list of failing guards. The report ends with a block listing what the fleet decided on its own that day.

## Smaller threads

- **A drift audit** (Democritus) runs about twenty cross-checks every morning: ledger uniqueness, index completeness, tracker-versus-canon rendering, cron-versus-register agreement, monitor token uniqueness. It distinguishes *match*, *drift*, *retired* and *unverifiable*.
- **Home network**: a three-tier segmentation (general, IoT, cameras) completed in mid-2026, with a DNS filter and a firewall-managed VPN mesh.
- **A red-team and OSINT lab** exists only as a design. It is not built and does not run.
