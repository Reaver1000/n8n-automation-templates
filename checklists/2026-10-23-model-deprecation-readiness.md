# The Model Deprecation Calendar — an Oct 23, 2026 readiness checklist (and the six waves behind it)

Oct 23 is not the event. The calendar is.

On October 23, 2026, OpenAI shuts down a batch of legacy models — and n8n 3.0 removes or
changes a number of nodes in the same window. Workflows that still call the old models will
start failing. If you fix them once and go back to sleep, you will be doing this again in
December. And January. And February.

OpenAI now publishes a rolling deprecation calendar. Between now and April 2027, something
you may depend on is scheduled to shut down every month. One-time fixes lose. Readiness is
a habit.

## Part 1 — What shuts down on October 23, 2026

| Shutting down | Replace with |
|---|---|
| `gpt-3.5-turbo` / `gpt-3.5-turbo-0125` | `gpt-5.6-terra` |
| `gpt-4` / `gpt-4-0613` / `gpt-4-1106-preview` / `gpt-4-turbo` | `gpt-5.6-sol` |
| `gpt-4.1-nano` | `gpt-5.6-luna` |
| `gpt-4o-2024-05-13` | `gpt-5.6-sol` |
| `o1` / `o1-pro` | `gpt-5.6-sol` |
| `o3-mini` | `gpt-5.6-sol` |
| `o4-mini` | `gpt-5.6-terra` |
| `gpt-image-1` | `gpt-image-2.5-sunburst` or `gpt-image-2.5-flare` |
| fine-tunes on `ft-gpt-3.5-turbo`, `ft-gpt-4`, `ft-babbage-002`, `ft-davinci-002` | re-train on the 5.6 family |

Source: OpenAI's deprecations page (platform.openai.com/docs/deprecations). Check that page,
not this file, when you run your audit — the calendar moves.

## Part 2 — The waves already scheduled behind it

| Date | What shuts down | Replacement family |
|---|---|---|
| Nov 30, 2026 | OpenAI Evals API + Agent Builder + reusable prompts | Promptfoo / Agents SDK |
| Dec 1, 2026 | `gpt-image-1-mini`, `gpt-image-1.5`, `chatgpt-image-latest` | `gpt-image-2.5-*` |
| Dec 11, 2026 | `gpt-5` snapshots, `o3`, `o3-pro` | `gpt-5.6-*` |
| Jan 6, 2027 | all legacy TTS (`tts-1`, `tts-1-hd`, `gpt-4o-mini-tts-*`) | `gpt-realtime-2.1-mini` |
| Jan 20, 2027 | legacy realtime/audio (`gpt-realtime`, `gpt-audio`, `gpt-4o-audio`, and mini variants) | `gpt-realtime-2.1` / `gpt-audio-1.5` |
| Feb 26, 2027 | `whisper-1` + all `gpt-4o-transcribe*` | `gpt-live-transcribe` / `gpt-transcribe` |
| Apr 1, 2027 | `gpt-5.1`, `gpt-5.3-codex`, `gpt-5.4-nano` | `gpt-6-sol` / `gpt-6-luna` |

## Part 3 — The 30-minute readiness checklist (before Oct 23)

1. **Export everything.** `n8n export:workflow --all` (or Settings → Export in the UI).
   This is also your backup. Do not skip it.
2. **Scan for what breaks.** Run [n8n-sunset](https://github.com/larik15/n8n-sunset) —
   `npx n8n-sunset@latest your-workflows/` — an open-source CLI by Larik that lists every
   node and model in your exports that breaks on n8n 3.0 or the OpenAI shutdowns, with
   dates and suggested replacements. Credit where due: it's a good tool.
3. **Map replacements.** Use the tables above; verify against OpenAI's deprecations page.
   Never trust a secondhand list — including this one.
4. **Test before you switch.** Duplicate the workflow, swap the model, run with pinned data,
   and compare outputs. A green execution is not the same output; different models behave
   differently.
5. **Wire an error workflow BEFORE you switch.** The step everyone skips. If you don't have
   one, `workflows/alert-monitoring.json` in this repo is a starting point. Green executions
   can still contain empty results — an error workflow that watches item counts and response
   content catches what the logs don't.
6. **Stage the rollout.** Dev → test → prod. Pin your n8n version until you've read the 3.0
   breaking-changes release notes; never blind-upgrade production in the same week as a
   model shutdown.
7. **The 72-hour rule.** It isn't done until it has run unattended for three days without
   surprises.

## Part 4 — Make readiness a habit (the calendar is the point)

- Put the seven dates above in your calendar today, one hour each: "Audit for next wave."
- One shared error workflow covering every LLM-calling workflow = your always-on early warning.
- A 15-minute monthly audit: re-run the scan, re-read the deprecations page, review the error log.
- If you run many workflows across instances or clients: the audit stops being manageable
  in your head long before it stops being manageable in a spreadsheet. That's the problem
  I work on.

---

*Author: davidbuilds (n8n community). I run readiness audits and ongoing reliability
monitoring for teams with many workflows — async, in writing, no calls required. This
checklist is MIT-licensed; the deprecation data belongs to OpenAI and changes without
notice — verify before you act on it.*
