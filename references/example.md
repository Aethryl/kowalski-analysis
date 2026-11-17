### Kowalski, analysis

**Mission:** Ship per-tenant rate limits in the API and retire the old global throttle.
*Compacted: history before the schema discussion not visible*

**Objectives**
1. Rate-limit middleware: verified, `a41c9e2`
   - limiter, sonnet, `~/Repos/Worktrees/api/rate-limit`: returned
2. Drop the global throttle: returned
   - cleanup, sonnet, `~/Repos/Worktrees/api/drop-throttle`: returned
3. Docs page for the new limits: planned

**In flight:** *nothing*

**Next:** Verify the `drop-throttle` diff and run its tests.

**Needs you**
- Decide: keep the old `X-Throttle` header for one release, or remove it now?
- Review: `~/Repos/Worktrees/api/rate-limit`, ready for your eyes.

🐧❗ *We have a situation.*
