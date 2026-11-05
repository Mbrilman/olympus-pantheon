# Lessons learned

Real incidents, each with what it cost and what it changed. They are listed because rules without their history get dropped the first time they are inconvenient.

## 1. A stale sentence in the highest source nearly removed a monitor

**What happened.** The constitution said a certain machine carried no fleet work. That had been true, and stopped being true when it was drafted into verification work. The sentence stayed. A tracker item was filed proposing to remove the health check on that machine, a correct reading of an out-of-date sentence. The check was saved because the item was reviewed before acting.

**What it changed.** When a lower document contradicts the constitution, the constitution wins. So an out-of-date sentence there is not a cosmetic fault but a ranking problem: it overrides every correct line elsewhere. Corrections to the constitution now go first, not last. A rule was added that a status page carries pointers, not copies of state.

## 2. A health verdict built only from the judged seat's own words

**What happened.** The first version of the fleet-health verdict called one seat *unmeasurable* and another *offline by design* while both landed commits in the canon within the hour. Every one of its five sources was reported by the seat being judged. The one independent signal, the uptime monitor, was named in none of them. The tracker recorded twelve findings against the verdict, three of them high.

**What it changed.** The health engine reads an expected set and judges each seat from a vantage point other than its own, and the design has separate states for *gone* (it reported and stopped) and *never seen* (it is expected and has never reported). Two laws came out of it: *silence must be distinguishable from health*, and *an instrument must not lie about itself*, including the wording of its error text and the numbers in its alerts.

## 3. A container restart that wedged the daemon

**What happened.** A routine restart of one container on the NAS wedged the container daemon, and the uptime monitor, which ran there, was dead for about eight minutes. A watcher that lives on the thing it watches goes down with it.

**What it changed.** Restarting containers on that host is treated as an event that can take the watchers with it and is done with that in mind, and a check that shares fate with what it checks is recognised as checking nothing (the law of that name).

## 4. The same test, a different verdict under another account

**What happened.** A test suite gave a different result depending on which agent account, seat or environment ran it. A pass on the author's seat said little about the nightly gate, which runs in a sandbox with its own home directory and path.

**What it changed.** A suite is measured under each environment it must work in, not only where it was written. A newly written suite that names a network tool must be listed in the same change as the meta-suite that audits such suites, or that meta-suite goes red the night it lands.

## 5. An approval that closed too much

**What happened.** A first deliverable of a multi-step item was approved, and the item was marked done. The remaining steps were in the request, under a heading the reviewer had skimmed.

**What it changed.** The reviewer reads the "not done / next" part of the request before changing a status, and an approval is worded as covering *this* commit only.

## 6. A report filed under the owner's name without his yes

**What happened.** An upstream bug report on a public project was filed from an account belonging to the owner, on the basis of a standing mandate that a newer order ("put nothing on my name") had overtaken six days before the report was filed. Nobody could find a recorded yes.

**What it changed.** External filings under his name stopped. The material was parked until the fleet has its own account. A rule was added that any such action needs an explicit, dated, single-purpose yes on the item, which is exactly how this repository was authorised.

## 7. A gate that outgrew its time limit

**What happened.** A nightly gate of about a thousand suites grew from roughly eleven minutes to over a hundred, and one night it reached its limit and published "unmeasurable". The cause was not one thing:

- a meta-suite that checks which tests call out to the network had gone red because two new suites had not been listed; a red first call is rerun, so that suite ran twice every night (about 340 seconds);
- a newly added test harness for the command filter took 441 seconds alone, more than the default per-suite limit of 300;
- a handful of other suites ran two or three times because of first-call failures that happen only under the gate's load.

**What it changed.** A named limit per suite instead of one default; a per-suite timing log that survives a killed run (it already existed: see lesson 9); a switch that makes the expensive mutation checks opt-in while keeping the cheap regression cells in the nightly run, with the green line worded as "cells only, mutants skipped" so nobody reads it as a mutation proof; and a rule that a new suite naming a network tool is registered in the same change.

## 8. A new release of a safety tool that refused ordinary text

**What happened.** The first seat to install a new release of the command filter noticed it denied a long, ordinary comment of about 4 KB with a "could not complete safety evaluation" message after about 0.8 seconds of CPU, well below its configured limit. The previous release allowed it. The rollout gate had 146 short commands and no long one. Later measurements showed that what triggers the give-up depends on the content of the text, not only on its length.

**What it changed.** The gate got a fixture of long heredocs and a way to accept a *known, deterministic* limit by name, bound to the hash of the binary it was measured on, with the measured trigger stated. The acceptance ends with that binary: the next release has another hash, so the entry reads "expected but not seen" until a person measures and judges it again. A new release was held on the master seat until the gate could judge the case. The workaround for authors is to write long text to a file and pass the file.

## 9. The recorder that already existed

**What happened.** While investigating the run time of the gate, the master agent commented on the tracker that no per-suite timing existed, started building a stopwatch in the gate, and found a name collision with a recorder it had added itself weeks earlier (a log file already holding every suite's duration). Nothing was committed; the tracker comment was corrected the same hour.

**What it changed.** Before saying a measurement does not exist, search the tool and its log directory for it. Before adding an instrument, grep for its subject.

## 10. A restart that kept losing the work in flight

**What happened.** A seat died of a full context window three times in two weeks; once it stayed on the wall for three days. Automatic compaction was off by decision, and the restart path was refused by a guard meant to prevent two live sessions of the same seat.

**What it changed.** Handovers at 80 percent, a restart tool that can tell an idle seat from a busy one, a rule that a handover written *before* the restart order does not count (the order is reissued and the agent must rewrite it), and an allow-list for a known interactive twin session. The remaining gap, a handover at the 90 percent wall with a live twin, is still on the tracker.

## 11. An alarm that fired because a one-hour-old fact was still being read

**What happened.** A guard turned red for one run and was reported as red to the owner the same morning; a hand run minutes later was green. The red probably coincided with a canon update landing while the guard ran; that is a reading, not a proven cause.

**What it changed.** The report says "red since the morning, green by hand at the time of writing, probably a transient", and a recheck is scheduled, rather than either hiding it or claiming a failure.

## A short list of habits that outlived their incidents

- Write down the promise and file the item **in the same turn**. "That will become an item" is a debt.
- If you find a flaw on the way to something else, file it with an owner and a reviewer, even outside your area.
- Ask the question you can answer yourself first: the canon, then the owning agent, and only then the human.
- Say plainly what you did not measure.
