End the session.

1. Run `make check`. If it fails, say so and do not mark the task done.
2. Run `git status`. List anything uncommitted and why.
3. Grep the diff for anything that looks like a real secret, MAC, IP, Entra GUID, or tenant name. Report hits and stop if any are not obviously synthetic.
4. Rewrite docs/STATE.md (do not append). Keep it under 60 lines:
   - Current task: the next unfinished task.
   - Tasks: the remaining ordered list.
   - Done: one line per finished task.
   - Open decisions and known issues, current only. Drop resolved ones.
5. Append any architecture decision made this session to docs/DECISIONS.md, one line with the reason.
6. If this session touched the approval page, list exactly what I should open and check in the browser, including a phone-width check.
7. Commit the doc changes with `docs: update state`. Do not push.
8. Tell me in two lines what was done and what is next, then stop.
