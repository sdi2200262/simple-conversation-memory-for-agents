# Simple session memory for agents

A skill for Claude Code and Codex that keeps a simple memory structure with a Markdown
log per conversation, across compactions and model changes. Ask your agent to save progress,
recover earlier context, or prepare a handoff.

<p align="center">
  <img src="assets/diagram.svg" alt="Conversation logs, context periods, and supporting files linked through a shared memory index" width="800"/>
</p>

## Install

Install the plugin in each assistant you use. Each install adds the
`simple-session-memory` skill from the same marketplace.

<details>
<summary><strong>Claude Code</strong></summary>

Run in Claude Code and choose user-wide or project scope when installing:

```text
/plugin marketplace add sdi2200262/simple-session-memory-for-agents
/plugin install simple-session-memory@simple-session-memory-for-agents
```

</details>

<details>
<summary><strong>Codex</strong></summary>

Run in a terminal:

```sh
codex plugin marketplace add sdi2200262/simple-session-memory-for-agents
codex plugin add simple-session-memory@simple-session-memory-for-agents
```

Start a new Codex session after installation.

</details>

## Use

Ask your agent to log progress, recover context from past conversations, or
prepare a handoff for future ones. You can also invoke `simple-session-memory`
directly.

Write memory on request or through an established workflow's instructions,
including autonomous workflows. Each checkpoint updates current state and next
steps, preserves history by context period, and recovers missing earlier work
from available transcripts. Sources and gaps are recorded.

Logs and important supporting files live in dated folders under `.agents/memory/`,
linked from `index.md`. An existing project memory location takes precedence.
For orientation at startup, add an index pointer to `AGENTS.md` or `CLAUDE.md`.

## Archives

Archive completed conversations on request or through your workflow. Each archive
combines their history into a narrative and keeps important supporting files
alongside it:

```text
.agents/memory/archives/
  archive-2026-10-01-to-10-18/
    archive-2026-10-01-to-10-18.md
    archive-2026-10-01-to-10-18-sessions.zip
    retained-report.md
```

The ZIP preserves the complete original session folders, logs, and supporting
files. It is optional when originals remain and required before authorized
cleanup removes them. Its contents must be verified against the originals first.
Important files remain available unpacked and linked from the narrative.

The shared `index.md` links sessions and archives, with short monthly summaries
for orientation.

## License

[MIT](LICENSE).

<p align="center">
  <a href="https://github.com/sdi2200262" target="_blank">
    <img src="assets/cobuter-man.png" alt="CobuterMan" width="150"/>
  </a>
</p>
