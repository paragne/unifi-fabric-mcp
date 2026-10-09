Start a session.

1. Read docs/STATE.md (already imported by CLAUDE.md) and the last 10 lines of docs/DECISIONS.md.
2. Run `git status` and `git log --oneline -5`. If the tree is dirty, stop and tell me what is uncommitted. Do not commit, stash, or restore it.
3. Run `make check`. If it fails, stop and report. Do not fix anything yet. (Before task 1 lands there is no Makefile; skip this step and say so.)
4. State the current task in one sentence, the files you expect to touch, and the tests you will add.
5. If the task touches auth, redaction, secrets, tenants, the change pipeline, or the approval page, list the CLAUDE.md security rules that apply to it.
6. Wait for my go before editing.
