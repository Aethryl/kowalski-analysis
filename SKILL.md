---
name: kowalski
description: Status briefing for the current session.
disable-model-invocation: true
allowed-tools: Bash(git status:*), Bash(git rev-list:*)
---

# Kowalski, analysis

Brief the user on the current session, recalled from context. This is a recollection, not an
investigation: do not read the transcript, re-read files or re-run tools to recheck what you
already know.

## Output

Reply with plain Markdown in exactly this shape. No code fence, no box drawing, no tables, no
text before or after. It has to read well in a terminal and in clients that wrap lines on a
narrow screen, so every line is ordinary prose that can wrap anywhere.

```markdown
### Kowalski, analysis

**Mission:** the goal in the user's terms, not the last tangent

**Done**
- finished, and verified

**In flight:** running agents and background shells, or *nothing*

**Next:** the immediate next action

**Needs you**
- what is blocked on the user

**Snags**
- what broke, what was assumed, what was skipped

**On disk**
- uncommitted, untracked or unpushed work

`<(o_o)>` *Analysis complete.*
```

- Keep the order above. Put a blank line between sections, or Markdown merges them into one
  paragraph.
- **Mission**, **In flight** and **Next** always appear, each on one line. **In flight** says
  *nothing* when idle, because knowing that nothing is running is the point.
- **Done**, **Needs you**, **Snags** and **On disk** are bullet lists. Leave a section out
  entirely when it is empty; never write "none".
- Each bullet is one short sentence. Put paths, commands, branches and IDs in `inline code`.
  No nested bullets, no bold inside bullets.

## Rules

1. **Done means verified.** Separate "wrote it" from "ran it and it passed". Anything that exists
   only as an intention goes under **Next**, never **Done**.
2. **Say when the session was compacted.** Put it under **Snags** and name what you therefore
   cannot see. A recap that silently covers only post-compaction history is a lie, and it is the
   likeliest way this briefing misleads.
3. **Needs you is for real blocks:** decisions waiting on the user, questions asked and never
   answered, and commands only they run, such as full test suites, migrations and data scripts.
4. **On disk needs one probe, and only if this session wrote files:**

   ```sh
   git status --short -b; git rev-list --count '@{upstream}..HEAD' 2>/dev/null
   ```

   Quote `'@{upstream}..HEAD'`, or the brace-expansion guard rejects the command. Skip the probe
   in a read-only session, and leave the section out when the tree is clean. A branch with no
   upstream is normal: report nothing, not a snag.
5. **At most five bullets per section.** Beyond five, keep the most recent and end with a bullet
   saying `+N more`. Never truncate silently.
6. **An empty session gets the three mandatory lines and the footer.** Do not invent a mission.

## Footer

The last line is one penguin in inline code and one italic sign-off, chosen from the actual state:

| Penguin | Sign-off | When |
|---|---|---|
| `<(o_o)>` | *Analysis complete.* | all clear |
| `<(O_O)>` | *We have a situation.* | something is blocked on the user |
| `<(-_-)>` | *Just smile and wave, boys.* | nothing left to do |
| `<(x_x)>` | *You didn't see anything.* | something failed |

The title, the penguin and the sign-off are the whole joke budget. Never let the bit soften,
inflate or reword a fact. If the session went badly, say so plainly and let the penguin look
worried.
