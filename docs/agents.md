# Agents

The Pantheon has four working agents and one write-gate script. A fifth, independent checker is part of the design and is currently switched off.

## The roster

| Name | Role | Why it exists |
|---|---|---|
| **Diana** | Master. Delegates, decides, reviews. First point of contact for the owner. | Someone has to hold the whole picture and be accountable for decisions that do not belong to a single owner. |
| **Luna** | Twin of the master. Second reviewer, backup, and owner of the command filter work. | A decision-maker with no equal reviewer is a single point of failure. Luna is deliberately allowed to say no to Diana. |
| **Atlas** | Operations agent. Recurring jobs, monitoring, websites, updates, mail. | Always-on operations need one owner who is not busy building. |
| **Hephaestus** | Builder. Forges new tools and hands them to the operations agent. | Building and operating pull in opposite directions: a builder wants change, an operations agent wants stability. |
| **Momus** | Independent checker on a different model family. No write access to the canon. **Switched off since October 2026**, the design stays. | Agents on the same model can share the same blind spots. Momus was the answer to "who checks the checkers", and the question stays open while he is off. |
| **Clio** | Not an agent. A deterministic script on the operations agent's seat that is the **only writer** of the canon. | One pen means no quiet rewrites. Clio validates, scans for secrets and commits. |

The agents are named after figures from Greek and Roman myth, with one deity per role and no duplicates. The point is practical: a name in a log should tell you what kind of thing is speaking.

## How the roles split the work

One rule decides who owns what kind of work: **decisions and review to the master, always-on operations to the operations agent, new builds to the builder, continuity and second review to the twin.** An agent does not take another's role because it is idle. Work that falls between roles gets an owner assigned on the tracker the moment it is noticed, with a reviewer who is not the owner.

## Decisions by size

A decision ladder sets who decides what, so that nothing waits on the human by default:

1. **The item's owner decides** what is theirs, reversible, and internal to the fleet. They write the decision and the reason on the item and carry on.
2. **The master and the twin decide together** when a change is structural or touches more than one item owner.
3. **All seats must consent**, with a 24-hour clock and a dead-seat rule, for decisions that change how the whole fleet works.
4. **Parked, with a reason and an item owner who is an agent**, for anything in the gated classes below, so that "waiting for the human" is never the silent default.

## What only the human decides

Four classes stay with the owner, and everything else is the fleet's to do and report:

- money and anything billable,
- exposure and trust boundaries: what becomes reachable from outside, and what a new party is allowed to touch,
- actions whose worst failure is irreversible,
- what only a person can physically do.

Since October 2026 there is a stricter standing order: **nothing is put on the owner's name.** No work item is owned by, reviewed by or blocked on the human; no standing "waiting for the human" list exists. If an agent needs a gated decision, it picks the safe, reversible variant, writes the reasoning on the item, and parks the rest.

A recent example of a gated class being applied: when the owner asked for this very repository, the instruction was recorded verbatim on a tracker item as an explicit, single-purpose yes, because an earlier incident (an upstream bug report filed under the owner's account without a recorded yes) had shown that a mandate read past a later order is exactly how things end up on someone's name.

## Sessions, memory and restarts

Agents are long-running terminal sessions. Their context fills up. Three mechanisms keep work from being lost:

- **Handover at about 80 percent of the context window**: the agent writes down what it was in the middle of, what is promised but not yet an item, and the exact next step.
- **A restart tool** that starts a fresh session from that handover once the agent is idle, with guards against restarting a seat that has a live twin or is in the middle of a turn.
- **A rule that automatic compaction stays off.** A decision was made to restart from a written handover rather than let a summariser silently decide what to forget. The consequence is accepted: an agent can be quiet for a while around a restart.

Session budgets are managed centrally: the agents share one subscription pot, and a budget service tells each agent whether heavy work is allowed right now. Reviews are exempt from the brake because review is considered the most important work.

## Scheduled work orders

A trigger service hands each agent work in alternating modes (oldest open item, highest-priority open item) so that old items do not starve behind new urgent ones. Each order must be answered with a closing message, and an order with nothing workable is closed as "lapsed" with a reason, not ignored. Reviews arrive as their own order type and are considered every round.
