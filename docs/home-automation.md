# Home automation

Home automation is part of the landscape and not a fleet project. The owner runs it and uses it, and has said plainly that he does not want it turned into a stream of questions or proposals from the agents. This page describes how the fleet relates to it.

## What exists

A home-automation platform runs in a container on the NAS and is reachable only inside the network. Its integrations cover household energy and a few devices; which ones is the owner's business and is not described here. Cameras and other IoT devices sit on their own network segments, separate from the general segment and with their own firewall rules.

## How the fleet relates to it

| Concern | Rule |
|---|---|
| Ownership | The owner holds the main account. Access tokens live in the vault and never travel in a message. |
| Configuration | Edited as files and applied through the platform's own interface. Anything configured in the interface is changed through the interface. |
| Health | Watched like everything else. The integrations have a group monitor, and one that goes down appears in the daily report. A failure that is known is reported and not hidden, and it is not turned into a project. |
| Decisions | The fleet decides nothing about how the household uses energy or its devices. Those choices are the owner's. |

## The boundary is the useful part

A system can be monitored, kept patched and documented without being adopted as a project by the agents. The rule "do not turn this into a project and do not ask questions about it" is recorded in the agents' own memory and is applied: a failure is reported once, and the owner decides whether anything should be done about it.
