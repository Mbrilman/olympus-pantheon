# Olympus and the Pantheon

A home lab run by a small team of AI agents, written up in the open.

**Olympus** is the name of the whole landscape: a NAS, a hypervisor with a few virtual machines, a rented server, a firewall, a mesh of VPN links and a spare PC that serves a local language model. **The Pantheon** is the team of AI agents that live on it: they watch it, repair it, extend it and report on it. One human, the owner (Prometheus, a.k.a. Macel), directs the work and keeps the final say over a short list of decisions.

This repository describes how that works: who the agents are, how they agree on what is true, how they review each other, what they build, what they have got wrong, and the rules that came out of it. It is written for people who want to run something similar, or who want to see what a multi-agent operation looks like after several months of real use, with its failures included.

> Everything here is public by design. There are no credentials, addresses, host names or personal records in this repository. Numbers are aggregate counts. Where something does not exist or does not work, the text says so.

## At a glance

| | |
|---|---|
| Agents | Four working agents (Diana, Luna, Atlas, Hephaestus) and one write-gate script (Clio). An independent checker on a different model family is part of the design and is currently switched off |
| Human | One owner who sets direction and approves four classes of decision |
| Memory | A git repository ("the canon") with exactly one writer |
| Work tracking | A tracker where every piece of work has an owner and a different reviewer |
| Messaging | Plain files in lanes on the NAS ("Iris"), by design not a service |
| Monitoring | About a hundred monitors in Uptime Kuma, per-seat guards, a health engine called Panoptes, a security log store |
| Public project | Aion, an evidence-graded atlas of history with dated, sourced, neutral entries |
| Safety layers | A command filter in front of every shell, a single-writer rule for shared memory, four-eyes review, and a short list of things only the human may decide |

## Reading order

1. [Architecture](docs/architecture.md): the machines and how they are connected, in roles rather than addresses.
2. [Agents](docs/agents.md): who does what, and why the roles are split the way they are.
3. [Method](docs/method.md): the canon, the tracker, the review rules, the laws, how decisions are sized, and how a session survives running out of context.
4. [Projects](docs/projects.md): monitoring, the health engine, the command filter, Aion, backups, the local model.
5. [Reporting and channels](docs/reporting-and-channels.md): how the agents tell the owner what happened, and which channels exist (and which are switched off).
6. [Lessons learned](docs/lessons-learned.md): concrete mistakes and what each one changed.
7. [Safety and privacy](docs/safety-and-privacy.md): the boundary between what is public, private and work, and how it is enforced.
8. [References](docs/references.md): the open-source projects this builds on.

## What this is not

- It is not a product or a framework you can install. The tools are specific to one household and one set of machines.
- It is not a claim that agents can run unattended without a human. The owner stays in the loop for money, exposure to the outside world, irreversible actions and anything only a person can do physically. The agents' job is to make that list short and to never let it grow by accident.
- It is not finished. Several items in the lessons file are still open at the time of writing.

## The one-paragraph version

The agents share one memory (a git repository) that only one script may write to, so no agent can quietly rewrite what the others rely on. Every change has an owner and a different agent as reviewer, and reviews are measured: the reviewer re-runs the tests and re-checks the sources instead of trusting the report. Tools are tested by tests that are first shown to fail, so a green result means something. Monitoring is judged from outside the thing being monitored, and "I could not measure this" is a first-class answer that is never allowed to look like "all is well". The agents report to the human once a day by mail, and decide the rest themselves unless it falls into the four gated classes.

## Samenvatting in het Nederlands

Olympus is een thuisnetwerk met een NAS, virtuele machines, een gehuurde server en een firewall. De Pantheon is het team van AI-agents dat dit beheert, bewaakt en verder bouwt. Ze delen één geheugen (een git-repository) waar maar één script in mag schrijven. Elk werkstuk heeft een eigenaar en een controleur, en controleren betekent opnieuw meten. De mens bepaalt de richting en beslist alleen over geld, blootstelling naar buiten, onomkeerbare acties en wat alleen hij fysiek kan. Alles hier staat openbaar, zonder wachtwoorden, adressen of persoonlijke gegevens.
