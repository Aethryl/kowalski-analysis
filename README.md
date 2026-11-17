<p align="center">
  <img alt="Kowalski drawn as the Claude mascot" src="assets/kowalski-claude.svg" width="200">
</p>

<h1 align="center">kowalski</h1>

<p align="center">
  <b>One command that tells you what the current session is actually doing.</b><br>
  A status briefing for Claude Code, recalled from memory, costing nothing until you ask.
</p>

<p align="center">
  <img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-blue.svg">
  <img alt="Claude Code v2.1.310" src="https://img.shields.io/badge/Claude%20Code-v2.1.310-d97757.svg">
  <img alt="Always-on cost: zero" src="https://img.shields.io/badge/always--on%20cost-zero-2f9e6e.svg">
</p>

---

You are twelve tangents deep, the context compacted once, three agents are off in their own
worktrees, and you cannot remember which of their results you have actually checked. Type
`/kowalski` and get this:

> ### Kowalski, analysis
>
> **Mission:** Ship per-tenant rate limits in the API and retire the old global throttle.
> *Compacted: history before the schema discussion not visible*
>
> **Objectives**
> 1. Rate-limit middleware: verified, `a41c9e2`
>    - limiter, sonnet, `~/Repos/Worktrees/api/rate-limit`: returned
> 2. Drop the global throttle: returned
>    - cleanup, sonnet, `~/Repos/Worktrees/api/drop-throttle`: returned
> 3. Docs page for the new limits: planned
>
> **In flight:** *nothing*
>
> **Next:** Verify the `drop-throttle` diff and run its tests.
>
> **Needs you**
> - Decide: keep the old `X-Throttle` header for one release, or remove it now?
> - Review: `~/Repos/Worktrees/api/rate-limit`, ready for your eyes.
>
> 🐧❗ *We have a situation.*

It's plain Markdown, so it renders natively in the terminal and wraps cleanly in narrow clients
such as mobile apps, where a fixed-width box would break.

## What it does

It briefs you as an orchestrator: the mission, then each objective with its state, and under it
the agent working on it (name, model, worktree). Every objective sits on one ladder, `planned`,
`dispatched`, `returned`, `verified`, `reported`, then `committed`, `pushed` or `MR !n`, so a
result an agent returned but nobody checked stands out. Needs you holds only decisions and
reviews. A session with no agents still gets a valid briefing.

## Install

```sh
git clone https://github.com/Aethryl/kowalski-analysis ~/.claude/skills/kowalski
```

Then `/reload-skills`, or start a new session. The command comes from the frontmatter, not the
directory, so the folder can be called anything. Needs Claude Code new enough for
`disable-model-invocation` in skill frontmatter; tested on v2.1.310.

## What it costs

Nothing until you type it. Most skills pay a standing tax: their description sits in every
request so the model can choose them. This one sets `disable-model-invocation: true`, which
keeps the description and the body out of the preload entirely. You are the trigger, so it
never needs to be chosen. Measured rather than estimated — with the flag on, the skill is
absent from a live session's skill listing; take the flag off and it appears.

The briefing is recalled from the session already in context. It never reads the transcript,
agent transcripts or agent output files. It runs a few light read-only probes in parallel, each
skipped when it doesn't apply: the live agent roster and `mr-status ls`. Nothing else is
touched.

## What it will not do

- **Claude cannot invoke it.** `disable-model-invocation` is what makes it free; the price is
  that asking for a status report in words won't produce one. `/kowalski` is the only way in.
- **It only knows this session.** By design, because it's there to catch you up on the session
  you are sitting in.
- **It reports, it does not act.** No commits, no cleanup, no writes of any kind.

## License

MIT
