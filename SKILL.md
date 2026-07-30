---
name: kowalski
description: Status briefing for the current session.
disable-model-invocation: true
allowed-tools: Bash(git status:*), Bash(git rev-list:*), Bash(git branch:*)
---

# Kowalski, analysis

Brief the user on the current session. Recall it from context. Do not read this session's
transcript, do not re-read files to rebuild history, and do not re-run tools to recheck state
you already know. This is a recollection, not an investigation.

Emit exactly one code fence, with nothing before or after it:

```
KOWALSKI, ANALYSIS
══════════════════════════════════════════════════
Mission     the goal in the user's terms, not the last tangent
Done        · finished, and verified
In flight   running agents and background shells, or: nothing
Next        the immediate next action
Needs you   · what is blocked on the user
Snags       · what broke, what was assumed, what was skipped
On disk     · uncommitted, untracked or unpushed work
──────────────────────────────────────────────────
<(o_o)>  Analysis complete.
```

Copy the two rule characters exactly as shown, `═` above and `─` below, each run matching the
width of the longest line. Labels sit in a 12 character column. `Mission`, `Next` and `In flight` always appear.
`In flight` prints `nothing` when idle, because knowing that nothing is running is the point.
Drop `Done`, `Needs you`, `Snags` and `On disk` entirely when they are empty, rather than
writing "none".

## Rules

1. `Done` means verified. Separate "wrote it" from "ran it and it passed". Anything that exists
   only as an intention is `Next`, never `Done`.
2. If the session was compacted, say so under `Snags` and name what you therefore cannot see.
   A recap that silently covers only post-compaction history is a lie, and it is the likeliest
   way this briefing misleads.
3. `Needs you` is for real blocks: decisions waiting on the user, questions asked and never
   answered, and the commands only they run, such as full test suites, migrations and data
   scripts.
4. `On disk` is conditional. Only if this session wrote files, run this once:

   ```
   git status --short -b; git rev-list --count '@{upstream}..HEAD' 2>/dev/null
   ```

   Quote `'@{upstream}..HEAD'`, or the brace-expansion guard rejects the command. Print the
   line only when something is uncommitted, untracked or unpushed. Skip the probe entirely in a
   read-only session, and omit the line when the tree is clean. A branch with no upstream is
   normal, so report it as nothing at all rather than as a snag.
5. One line per entry, five entries per section. Beyond five, keep the most recent and append
   `+N more`. Never truncate silently.
6. A session with nothing in it gets the three mandatory lines and nothing else. Do not invent
   a mission.

## The footer

One penguin and one sign-off, paired, chosen from the actual state:

```
<(o_o)>  Analysis complete.            all clear
<(O_O)>  We have a situation.          something is blocked on the user
<(-_-)>  Just smile and wave, boys.    nothing left to do
<(x_x)>  You didn't see anything.      something failed
```

The header, the penguin and the sign-off are the entire joke budget. Never let the bit soften,
inflate or reword a fact. If the session went badly, say so plainly and let the penguin look
worried.
