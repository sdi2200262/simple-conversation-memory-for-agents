---
name: simple-session-memory
description: Read or recover conversation memory, log progress, prepare a handoff, or archive completed sessions when requested directly or instructed by an established workflow.
---

# Simple Session Memory

Keep one Markdown log per conversation so agents can resume work after
compaction or in future conversations. Write for an agent with access to the log
and workspace files but no conversation context.

## Conversation, context period, checkpoint

A **conversation** is the continuing user exchange. It keeps the same log through
compaction, model changes, and runtime restarts. A new conversation gets its own
log, optionally linked to the log of the conversation it continues.

A **context period** is the portion of work from the conversation's start to its
first completed compaction, or from one completed compaction to the next. The
current period stays open until compaction completes. Restarts, model changes,
checkpoints, and failed compaction attempts do not start a new period. Retained
messages or summaries from earlier periods do not move that earlier work into
the new one.

A **checkpoint** is a log update requested directly or triggered by an established
workflow. One period can have several checkpoints; one checkpoint can recover
work from several periods.

## Location and identity

Follow an established project memory location; otherwise use `.agents/memory/`
under the workspace root. Both assistants share this location. Read `index.md`
first. Match the conversation ID and transcript pointers against existing logs
before creating one; do not assume the newest log belongs to this conversation.

For a new log, create `session-YYYY-MM-DD-NNN/`, taking the next unused daily
number starting at `001`. Create that directory without overwriting another
conversation's files; retry with the next number if it already exists. Its main
file has the same stem: `session-YYYY-MM-DD-NNN.md`. Keep useful supporting notes
beside it and link them from the log. Create only files the request needs.

## Writing a checkpoint

1. Read the existing log and identify which work it covers and which work is
   missing, including gaps in periods that already have entries.
2. Locate the conversation transcript. Compare its compaction boundaries and
   messages with the log. Recover missing earlier work from the transcript,
   checking relevant files and Git history to confirm what was done. Read long
   transcripts in small portions so recovery does not exhaust the context window.
3. Add recovered work under the context period where it happened. Record the
   source: directly observed, reconstructed from transcript, or recovered from a
   prior log or compaction summary. Treat recovery based only on summaries as
   partial evidence and identify its limits. If a transcript is unavailable or
   incomplete, record the supported outline and specific gaps instead of inventing
   a complete history.
   Do not invent period numbers when boundaries cannot be established.
4. Extend the current period's record and preserve accurate earlier history.
   Correct demonstrated errors explicitly. Refresh Current state and Next steps
   in place, so they describe the workspace at this checkpoint.
5. Update the checkpoint timestamp, coverage, and transcript pointers. Add or
   update this log's linked row in the index, preserving other conversations'
   entries. Report the log path and any material recovery gaps briefly.

Record user requirements, work and findings, decisions with reasons, useful
verification results, unresolved issues, and concrete next actions. Distinguish
proposed work from completed work. Cite relevant paths or commits. Treat memory
as a dated account and base technical decisions on current files.
Keep credentials and unnecessary personal information out of the log.

## Log format

Use this template, adapting the prose and omitting optional fields that do not
apply:

```markdown
---
created: YYYY-MM-DD
updated: YYYY-MM-DDTHH:MM:SSZ
conversation_id: <known conversation or thread ID; omit if unavailable>
transcripts:
  - <known transcript path or retrievable identifier; omit if unavailable>
continues: <prior conversation log path; only for a new conversation resuming it>
---

# Session: <short description>

## Current state
Task, requirements, current result, and remaining issues.

## History

### Context period 1
Coverage: <start and last covered message IDs or timestamps, when available>.
Source: <direct observation, transcript location, prior log, or summary>.

Work, findings, decisions and reasons, verification, and handoff state.
Gaps: <specific missing coverage, if any>.

## Next steps
Concrete next actions with the paths and details needed to continue.
```

Add another period heading only when needed. For uncertain boundaries, use
`### Earlier work (context boundaries uncertain)` with the known range and gaps.
Coverage identifies the work recorded, rather than when the checkpoint was
written. Extend it only when supported by evidence; name any gaps within the
range. Add source notes where one period combines direct and reconstructed
accounts.

## Index

Keep `index.md` concise. Use a Sessions table with a link to each log, its date,
and a one-sentence summary, newest first. When needed, add an Archives table
with links to archive narratives, date ranges, and summaries. Add monthly
Appendices for significant outcomes and knowledge useful to future work.
Keep detailed history and working state in the logs and archives.

## Archival

Archive when requested directly or instructed by an established workflow.
Select completed conversations and leave ongoing ones available for continued
logging. Use a date-range name covering the source sessions:

```text
archives/
  archive-2026-10-01-to-10-18/
    archive-2026-10-01-to-10-18.md
    archive-2026-10-01-to-10-18-sessions.zip
    retained-report.md
```

The main narrative and ZIP share the archive directory's stem. Include supporting
files only when useful. The ZIP is optional while original sessions remain and
required before removing them.

1. Read the selected logs and all supporting files in their directories.
   Synthesize a coherent history preserving outcomes, decisions and reasons,
   useful verification, and unresolved work. Remove repetition and obsolete
   working state. Record the source sessions, covered dates, and available
   conversation and transcript identifiers.
2. Keep files whose exact contents remain important unpacked beside the archive
   narrative, and link them from it. Incorporate other useful findings into the
   narrative. Resolve filename collisions without overwriting files, and repair
   links so the archive remains usable without the original session directories.
3. When creating a ZIP, name it `<archive-name>-sessions.zip`. Include the complete
   selected session directories, with their original logs and every supporting
   file, using paths relative to the memory root. Preserve the source files
   unchanged. Record the ZIP filename and included sessions in the narrative.
4. Before removing source directories, verify the ZIP can be read and lists the
   same paths and contains the same file contents as the source directories.
   A successful ZIP command alone is insufficient. If preservation or verification
   fails, retain the originals and report the problem. The ZIP preserves the
   original record; the narrative and unpacked attachments provide convenient access.
5. Replace the selected session rows in the index with a linked archive entry.
   Add a short summary under the covered month or months in Appendices, capturing
   milestones and durable observations. Retain the source-session listing in the
   narrative so originals remain discoverable, whether unpacked or in the ZIP.
6. Remove source directories only when the request or workflow authorizes
   cleanup, after verifying the ZIP, narrative, attachments, and index. Otherwise
   keep them. Report the archive path, ZIP status, and whether originals remain.

You may delegate archival to subagents.
