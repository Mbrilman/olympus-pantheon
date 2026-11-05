# References

Open-source projects and public resources this work builds on or refers to.

## Tools the operation uses

- **destructive_command_guard (dcg)**, a Rust safety hook that blocks destructive shell, git and filesystem commands before AI coding agents run them: <https://github.com/Dicklesworthstone/destructive_command_guard>. The owner keeps a fork at <https://github.com/Mbrilman/destructive_command_guard>. The fleet runs it as a fail-closed gate and adds its own rule packs and a rollout gate (see [projects](projects.md#the-command-filter) and lesson 8 in [lessons learned](lessons-learned.md)).
- **Uptime Kuma**, a self-hosted monitoring tool: <https://github.com/louislam/uptime-kuma>.
- **Claude Code**, the terminal agent harness the master and its peers run in: <https://www.anthropic.com/claude-code>.
- **Git**, the canon's storage.
- **Mermaid**, used for the diagram on the [architecture](architecture.md) page: <https://mermaid.js.org>.

## Related

- **Aion, the time globe** (the public project this fleet builds): the globe <https://aion.eanna.house/site/> and its page <https://github.com/Mbrilman/aion-time-globe>.

## Ideas that shaped the method

- Four-eyes review from change management, applied to every structural change by an agent other than the builder.
- Mutation testing: tests are shown to fail on a deliberately broken version of the thing they guard.
- "Don't automate chaos": make it work by hand first, automate only what demonstrably repeats.
- Measure first, then write the state down.

## About this repository

It describes a private household's lab at an aggregate level. It does not ship the tools.
