---
name: secret-check
description: Scan for keys and secrets before any push or before making a repo public. Use before every push, when the owner says "secret check", "is it safe to push", "make it public".
---

# secret-check

**When:** before every push; before flipping a repo public; whenever a file that holds credentials is touched.

**Patterns** (portable extended regex, matched case-insensitively):
`PAT='sk-[A-Za-z0-9_-]{20,}|[sr]k_(live|test)_[A-Za-z0-9]{16,}|AKIA[0-9A-Z]{16}|gh[pousr]_[A-Za-z0-9]{20,}|xox[bp]-[A-Za-z0-9-]{10,}|-----BEGIN [A-Z ]*PRIVATE KEY|(password|passwd|secret([_-]?key)?|api[_-]?key|token|private[_-]?key)[^[:alnum:][:space:]]?[[:space:]]*[=:][[:space:]]*[^[:alnum:][:space:]/<]?[A-Za-z0-9!@#$%^&*+=_~-][^[:space:]",]{7,}'` and `X=(':(exclude)scripts/memory/memory.test.mjs' ':(exclude)scripts/memory/memory-conserve.mjs' ':(exclude)scripts/memory/memory-lock.mjs' ':(exclude)scripts/memory/memory-selftest-cases.mjs')`: the memory kit's own test code, whose fixtures match; never add your own files to this list.

**Steps** (each prints a location and the first four characters only, never a whole line; a `fatal:` or other error line means that check did not run: stop, never PASS)
1. Working tree, tracked files (force-added ignored ones too) and untracked ones not ignored: `{ git grep -n -o -i -I -E "$PAT" -- . "${X[@]}"; git grep --untracked -n -o -i -I -E "$PAT" -- . "${X[@]}"; } | sed -E 's/^(([^:]+:){2})(.{4}).*/\1\3.../' | sort -u`
2. Staged snapshot: `git grep --cached -n -o -i -I -E "$PAT" -- . "${X[@]}" | sed -E 's/^(([^:]+:){2})(.{4}).*/\1\3.../'`
3. Outgoing history: `revs=($(git rev-list @{u}..HEAD 2>/dev/null)) || revs=($(git rev-list --all))` then `[ "${#revs[@]}" -gt 0 ] && git grep -n -o -i -I -E "$PAT" "${revs[@]}" -- . "${X[@]}" | sed -E 's/^(([^:]+:){3})(.{4}).*/\1\3.../' | cut -d: -f2- | sort -u`. Before going public use `revs=($(git rev-list --all))`.
4. Tracked env files: `git ls-files | grep -E "(^|/)\.env(\..+)?$" | grep -v "\.env\.example$"`; any line is a finding.
5. Ignore rules, one path at a time: `for p in .env .env.local .env.production; do git check-ignore -q "$p" || echo "FINDING: .gitignore does not cover $p"; done`
6. Classify each hit: a placeholder (`replace-me`, `your-key-here`, `xxx`, an example in docs) is noise; say so. A real-looking value is a finding.

**Report:** `PASS` with the five checks named, or one line per finding: file, line, which check, the first four characters. A finding in the working tree or staged snapshot stops the push until the value moves to `.env`. A finding in history stops the push and names two steps: the key is compromised, rotate it now; the history must be rewritten before any push (the owner decides how; never rewrite it yourself).

**Never:** print a secret or a whole matching line; push with a finding open; call a value a test key without the owner saying so; skip the history check because the tree is clean.
