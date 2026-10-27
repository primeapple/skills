---
name: agents-wishlist
description: Record environment and infrastructure needs agents encounter - missing tools, denied site or network access, unavailable resources, or service outages—with timestamp, evidence, and a possible fix in a shared local wish list.
---

# Agents' Wish List

Use when a task is blocked by the machine, sandbox, network policy, or shared infrastructure—not by a defect in the project being changed.

## Run

1. **Confirm the blocker.** Capture the exact command or resource and error. Check project instructions and available documented alternatives first. A typo, expected test failure, or fixable project configuration issue is not an infrastructure blocker.
2. **Try only low-cost workarounds.** Use safe, documented alternatives that need no extra access, installation, policy change, or significant effort. If no easy workaround resolves the blocker, stop the blocked task and ask the user what to do. Continue only after the blocker is resolved or the user directs a different path.
3. **Record the need** in `agents-wishlist/wishlist.md` under the local state directory:
   - Use `$XDG_STATE_HOME/agents-wishlist/wishlist.md` when set; otherwise use `~/.local/state/agents-wishlist/wishlist.md`.
   - Create the state directory and file if needed. Append; do not replace or rewrite prior entries.
   - Before writing, scan for an earlier entry describing the same problem. For a recurrence, reference that entry by timestamp and record only the new timestamp and meaningful changes (such as new impact, evidence, workaround, or potential fix). Do not repeat details already recorded.
   - If the state path is unavailable or unwritable, report that and include the minimal entry in the task response instead.
4. For a new problem, include:
   - **Time:** ISO 8601 UTC timestamp.
   - **Blocker:** concrete command, site, service, or resource and the observed error/status.
   - **Impact:** what task step this prevents.
   - **Evidence/workaround:** relevant checks and low-cost alternatives tried.
   - **Possible fix:** specific next step, labeled as a hypothesis when unverified; name who or what must provide it when known.
5. Keep entries actionable and safe. Never store credentials, tokens, cookies, private keys, or sensitive request data. Redact secrets and sensitive URL parameters. Do not change sandbox policy, install software, request access, or consume extra resources unless the user authorized it.
6. Do not claim a proposed fix is verified until tested.

## Entry format

```markdown
## <UTC timestamp> — <short environment need or blocker>
- Blocker: <command/resource and exact error or status>
- Impact: <task step blocked>
- Evidence/workaround: <checks and low-cost alternatives tried>
- Possible fix: <specific next step; mark unverified proposals>
```

For a recurrence, keep it short:

```markdown
## <UTC timestamp> — Recurrence: <short title> (see entry <earlier UTC timestamp>)
- Update: <new evidence, impact, workaround, or potential fix; omit unchanged details>
```
