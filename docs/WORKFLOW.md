# SCANERATOR — How We Work Together

This file lives at `docs/WORKFLOW.md`. To start a Claude session: **"read docs/WORKFLOW.md and start a session"**.

---

## Starting a session

1. Mention this file and the current branch/feature you want to work on
2. Reference `docs/Bugs.md` — bugs to fix take priority over new features
3. Reference `docs/BACKLOG.md` for the pending `[ ]` items backlog
4. Reference the plan file (`~/.claude/plans/`) for large features in progress, if one exists
5. State what you want to accomplish — Claude will ask clarifying questions if needed before starting
6. **Claude creates a feature branch before touching any code** — no exceptions (see Branch strategy below)

---

## Ending a session

Claude follows this checklist at the end of every working session, in order:

1. **Ask the user to test** — open `index.html` in Chrome/Edge/Firefox and smoke-test the change
2. **Wait for approval** — do not proceed until the user confirms ("ok", "good", etc.) or requests fixes
3. **Update all docs** once approved:
   - `docs/BACKLOG.md` — delete completed items (move to `docs/DONE.md` with `— OK`)
   - `README.md` — any user-visible change
   - `docs/MAINTAINER.md` — any architecture / integration / gotcha change
   - `WORKFLOW.md` — add a dated dev log entry for the session
4. **Commit everything** on the feature branch (code + docs in one commit, or docs as a follow-up commit)
5. **Ask the user to merge** — "Ready to merge `feature/xxx` → `main`?"
6. **After merge confirmed**: follow the [Version management](#version-management) steps below.

---

## Branch strategy

> **Rule #1 — enforced at session start**: Claude runs `git checkout -b feature/xxx` as the very first action of every session, before any file edit. If this step is skipped, no code changes are made until it is done.

| Rule | Detail |
|---|---|
| **Never commit to `main` directly** | Always branch first — no exceptions |
| Branch naming | `feature/short-description` (e.g. `feature/dashboard-filters`) |
| One branch per feature set | Group related changes; don't mix unrelated features |
| Merge only when complete | Feature done + docs updated + user smoke test passed |

```bash
# Start of session — always first
git checkout -b feature/my-feature

# End of session — after user approval
git checkout main
git merge feature/my-feature --no-ff
git push
```

---

## Version management

The version is a string in `index.html` only — in both i18n subtitle entries (`subtitle:` in `fr` and `en`).

### Rules

| Rule | Detail |
|---|---|
| **Never bump per branch** | Branches are dev-in-progress; version reflects what is released |
| **Bump at merge to `main`** | One bump per merge, committed right after the merge commit |
| Commit message | `chore: bump version to X.Y.Z` |
| File to update | `index.html` — the `subtitle:` string in both `fr` and `en` i18n objects |

### Increment guide

| Change type | Bump | Example |
|---|---|---|
| Bug fix, UI tweak, wording | `PATCH` | 1.0.0 → 1.0.1 |
| New feature (new capability, new screen) | `MINOR` | 1.0.x → 1.1.0 |
| Breaking / major architectural change | `MAJOR` | 1.x → 2.0.0 |

### How to bump

Edit the two subtitle strings in `index.html`:
```js
// fr
subtitle:'Impression A4 — regroupé par famille - vX.Y'
// en
subtitle:'A4 print — grouped by family - vX.Y'
```

---

## Release process

SCANERATOR is deployed via **GitHub Pages** from the `main` branch. Deployment is automatic on every push to `main` — no manual step required.

```
git push origin main
→ GitHub Pages rebuilds automatically
→ Live at https://libertyben-code.github.io/scanerator/
```

There is no build step, no CI pipeline, no package registry. The single file `index.html` is the release artifact.

---

## Commit discipline

- **One commit per logical change** — not one per file, not one per session
- Format: `type(scope): description` (conventional commits)
  - `feat(rand): add sequential generation mode`
  - `fix(ui): fix middle column overflow on small screens`
  - `docs: update MAINTAINER and WORKFLOW`
  - `chore: bump version to 1.1`
- Always `git push` immediately after each commit — no local-only commits
- Always add `Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>` at the end of commit messages

---

## Before committing — approval flow

**The mandatory flow for any code change:**

1. Claude implements the change and states which files changed and why
2. **User tests** — open `index.html` in browser and approve or request changes
3. Claude commits only after explicit user approval ("ok", "good", "commit it", etc.)

**Exceptions (Claude may commit directly after stating intent, no build test needed):**

- Typo / formatting fixes in docs
- Doc-only commits (README, MAINTAINER.md, BACKLOG/DONE)
- `WORKFLOW.md` updates

**If a tool call is rejected:**
- Do NOT retry the exact same call
- Read the rejection reason — it usually contains the fix
- Adjust approach and confirm before retrying

---

## Documentation — end of session AND before merging

Update docs **at the end of every working session**, not only at merge time:

| File | When to update |
|---|---|
| `README.md` | Any user-visible change |
| `docs/MAINTAINER.md` | Any architecture / integration / gotcha change |
| `docs/BACKLOG.md` / `docs/DONE.md` | Move completed items from BACKLOG → DONE immediately |
| `WORKFLOW.md` | When the collaboration process itself changes |

Before every `git merge feature/* → main`, verify all relevant docs are current.

---

## BACKLOG.md is the living backlog

`BACKLOG.md` contains only pending `[ ]` items. `DONE.md` is the archive.

```
# In BACKLOG.md:
- [ ] pending item

# When done — remove from BACKLOG.md, append to DONE.md:
- [x] done item — OK
```

New items are added by the user after testing. We work through them section by section. Move to `DONE.md` on the commit that closes the item, not before.

---

## Language rules

| Context | Language |
|---|---|
| App UI, labels, modals | French + English (bilingual — full i18n via the `i18n` object in `index.html`) |
| User messages | French or English — Claude responds in kind |
| Code comments | None by default (only add WHY if non-obvious) |
| Commit messages | English (conventional commits) |
| Internal docs (README, MAINTAINER.md) | French or English |

When adding any user-visible string, always add both `fr` and `en` entries to the `i18n` object in `index.html`.

---

## Coding style

- **No comments** unless the WHY is non-obvious (a bug workaround, a hidden constraint, a surprise behavior)
- **No docstrings** — well-named identifiers are self-documenting
- **No backwards-compat shims** — change the code directly
- **No defensive error handling** for impossible/internal cases — only at system boundaries (user input, IPC, external APIs)
- **Prefer editing existing files** over creating new ones
- **No premature abstractions** — a few similar lines is better than a helper for two uses
- **Single file** — all code stays in `index.html`; do not split into separate JS/CSS files unless the user explicitly requests it

---

## Known technical constraints

### 2026-06-23 — charset-toggle pills vs mode-toggle pills

The `.charset-toggle` CSS class is shared by both character-set toggles (A–Z, 0–9, a–z, –) and the Aléatoire/Séquentiel mode pills. The generic click handler in JS skips any `.charset-toggle` that has a `data-mode` attribute:

```js
document.querySelectorAll('.charset-toggle').forEach(el => {
  if (el.dataset.mode) return; // handled separately
  ...
});
```

**Rule:** any new pill-style toggle that must NOT behave like a charset toggle must have `data-mode="something"` on its element and its own dedicated click handler.

---

## Dated development log

### 2026-06-23 — Sequential generation + Suffix field
- Added `Aléatoire` / `Séquentiel` mode pills to the random generator panel (`cs-random`, `cs-sequential`)
- Sequential mode generates `random_chars(len) + zero-padded counter(seqDigits)` — total code length = `len + seqDigits`
- Added `rand-seq-fields` row (Début + Nb chiffres séq.) shown only in sequential mode
- Added Suffixe field (`rand-suffix`) alongside the existing Préfixe field
- Fixed middle column (`.panel-stack`) overflowing on small screens — added `overflow-y: auto`
- Bumped version `v1.0 → v1.1`
