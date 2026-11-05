# Safety and privacy

## Three lines that never mix

The operation keeps three areas apart, with separate accounts and credentials:

1. **Public**: what is meant to be seen by anyone (this repository, the public atlas).
2. **Private**: the household's own systems and the owner's personal matters. These are not published.
3. **Work**: the owner's employment. Nothing from that side is described, referenced or summarised anywhere in this repository, and the fleet's agents do not touch it unless the owner says so explicitly.

The master agent guards the line. A mistake that crosses it is treated as an incident, not a typo.

## What a public document may contain

- Roles, designs, rules and the reasoning behind them.
- Aggregate counts (a number of monitors, a number of tests), never lists of identifiers.
- Public project names and links to public repositories.
- Descriptions of failures, with what they changed.

## What it may never contain

- Passwords, tokens, keys or recovery codes, in any form, including in file names, commit messages, comments or examples.
- Network addresses, host names, account names, mail addresses, phone numbers or any identifier of a machine or a person.
- Personal records about the owner or anyone else.
- Anything that describes the owner's employer or workplace systems.

Public text is **scanned before it leaves the seat**: patterns for secret shapes, IPv4 and IPv6 addresses, host names, mail addresses and home-directory paths. It is scanned a second time from the **published** copy through the public API, on the principle that what matters is what the world can read.

## Secrets

A secret is held in a vault and read at runtime. It may exist outside the vault only as a runtime file with restricted permissions, and it never travels a message channel. Channels carry plain text only. A credential must be able to renew itself; a frozen credential is a dead agent.

Two practical consequences:

- **No token moves between seats to make a task easier.** When a task needs a credential that lives on another seat, the other seat performs the step and the first seat verifies the result. This repository was published that way.
- **A seat's tooling is logged out of external services when not in use**, so a plaintext token never sits in a configuration file after a one-off login.

## The command filter as a safety layer

Between each agent and its shell sits a filter that refuses dangerous commands before they run. Protected zones (shared memory, credential files, the filter's own wiring) are first logged in a dry-run mode, then armed per seat after the log has been read. A refusal explains itself and names the appeal route. There is no mode in which an agent can open its own window into the most sensitive zones.

## Third-party code

Bringing in someone else's code (a skill, a plugin, a package, a model) is admitted by what it can reach, not by how it looks. A vetting can fail, so the question is "what is the worst this can do if my judgment was wrong", and the answer sets who must approve it.

## Neutrality in published knowledge

The public atlas has written neutrality rules: facts and sources only; a tradition's claim is attributed to the tradition; no endorsement, dismissal or ranking; the same evidence standard for every tradition and every period. The reasoning is practical: a record that quietly favours one reading is a record nobody else can use.

## Who may do what with the human's name

The owner's name, accounts and reputation are his. Since October 2026 the agents do not put anything on his name; an exception needs an explicit, dated, single-purpose yes recorded on the work item.
