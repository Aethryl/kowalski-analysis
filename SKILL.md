---
name: kowalski
description: Status briefing for the current session.
disable-model-invocation: true
allowed-tools: ListAgents
---

# Kowalski, analysis

Brief the user on the current session as an orchestrator: what is being pursued, who is doing
each part, and how far each part has got. Recall is the base. Do not read the transcript, agent
transcripts or agent output files, and do not re-read files or re-run tools to recheck what you
already know. The probes below are the only live checks.

## Output

Reply with plain Markdown in exactly this shape. No code fence, no box drawing, no tables, no
text before or after. It has to read well in a terminal and in clients that wrap lines on a
narrow screen, so every line is ordinary prose that can wrap anywhere.

```markdown
### Kowalski, analysis

**Mission:** the goal in the user's terms, not the last tangent
*Compacted: history before <point> not visible*

**Objectives**
1. <objective>: <state>, `<sha>` or MR !n
   - <agent name>, <model>, `<tree>`: <agent state>
2. ...

**In flight:** running agents and background shells, or *nothing*

**Next:** the immediate next action

**Needs you**
- Decide: the open question, with the options
- Review: the diff or MR ready for the user's eyes, with path or MR ref

🐧 *Analysis complete.*
```

- Keep the order above. Put a blank line between sections, or Markdown merges them into one
  paragraph.
- **Mission**, **In flight** and **Next** always appear, each on one line. **In flight** says
  *nothing* when idle, because knowing that nothing is running is the point.
- The *Compacted* line sits directly under **Mission**, only when the session was compacted. It
  names what you therefore cannot see. A recap that silently covers only post-compaction history
  is a lie, and the likeliest way this briefing misleads.
- **Objectives** is a numbered list, one item per goal being pursued. Give the state from the
  ladder below, plus the commit sha or MR once one exists. A solo session with no agents still
  lists its objectives, with no nested bullets.
- The agent bullet is the only nesting allowed: one per agent under its objective, with name,
  model, tree and state (`running`, `returned`, `failed` or `stopped`). Work done by this
  session itself is marked `inline` in place of an agent.
- **Needs you** is a bullet list. Leave it out entirely when it is empty; never write "none".
- Each bullet is one short sentence. Put paths, commands, branches and IDs in `inline code`.
  No bold inside bullets.

## State ladder

An objective's state is one word, in this order: `planned`, `dispatched`, `returned`,
`verified`, `reported`, then `committed`, `pushed` or `MR !n`. State is a fact about the
objective, not a mood.

- `reported` means not committed yet, so uncommitted or unpushed work shows up on the ladder.
- `verified` means the orchestrator checked the diff and ran the checks. An agent's success
  report alone is `returned`.
- Anything that exists only as an intention is `planned`, and goes under **Next** when it is the
  immediate action.
- A `returned` objective that is not yet `verified` is the main signal to surface, so say so
  plainly.

## Probes

Run the probe once; it is read-only. Skip it when it does not apply.

1. `ListAgents`: the live roster, mapped to objectives by name and recall. Skip when no agents
   were spawned.
A probe that errors or times out is skipped silently; fall back to recall.

## Rules

1. **Done means verified.** Separate "wrote it" from "ran it and it passed" by the state ladder.
2. **Needs you has exactly two kinds of bullet:** `Decide:` for questions waiting on the user
   and `Review:` for work ready for their eyes. A command only the user runs, such as a
   migration or a remote script, is a `Decide:` phrased as an approval.
3. **At most five bullets per section.** Beyond five, keep the most recent and end with a bullet
   saying `+N more`. Never truncate silently.
4. **An empty session gets the three mandatory lines and the footer.** Do not invent a mission.

## Footer

The last line is one emoji with a mood mark and one italic sign-off, as plain text, not inline
code. Take the first row that matches the actual state:

| Emoji | Sign-off | When |
|---|---|---|
| 🐧❗ | *We have a situation.* | Needs you is not empty |
| 🐧💥 | *You didn't see anything.* | an agent or check failed |
| 🐧👋 | *Just smile and wave, boys.* | everything is pushed or reported |
| 🐧 | *Analysis complete.* | otherwise |

The title, the emoji and the sign-off are the whole joke budget. Never let the bit soften,
inflate or reword a fact. If the session went badly, say so plainly and let the emoji look
worried.

A filled-in briefing is in [references/example.md](references/example.md).
