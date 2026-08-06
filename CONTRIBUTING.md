# Contributing to acceler-presales / acceler-post-sales

This repo **is** the Claude Code marketplace (`.claude-plugin/marketplace.json` at the root) and hosts **two plugin sources**: `acceler-presales` (repo root) and `acceler-post-sales` (`post-sales/` subdirectory, its own `.claude-plugin/plugin.json`). It is the single source of truth for both — edit here, not in any local `…/APR - Pre-Sales Product/acceler-presales-plugin` copy.

## Install (teammates)

```
/plugin marketplace add voldemortuk/acceler-presales-plugin
/plugin install acceler-presales@acceler-local      # pre-sales
/plugin install acceler-post-sales@acceler-local    # delivery / post-sales, if you need it too
```

Public repo — no invite needed to install, just authenticate git for cloning with `gh auth login` (or SSH). To get updates later:

```
/plugin marketplace update acceler-local
```

## Make a change (contributors)

1. Get added as a **collaborator** on the repo (ask Utkarsh), then branch + PR.
2. Edit the relevant files: pre-sales commands/skills live at `commands/*.md` and `skills/*/SKILL.md`; post-sales/delivery ones live at `post-sales/commands/*.md` and `post-sales/skills/*/SKILL.md`. `knowledge/*` is pre-sales-only (the KG).
3. **Bump the version** in the plugin you actually changed: `.claude-plugin/plugin.json` for `acceler-presales`, `post-sales/.claude-plugin/plugin.json` for `acceler-post-sales` (e.g. `0.5.0` → `0.6.0`). Add a `CHANGELOG.md` entry labeled with the plugin name. *If you don't bump the version, teammates keep the cached copy and your change won't ship.* The two plugins version independently — don't bump one for a change to the other.
4. Validate before pushing: `claude plugin validate .`
5. Open a PR. On merge, teammates pick it up with `/plugin marketplace update acceler-local`.

## Conventions

- Keep the marketplace name `acceler-local` stable — it's the `@suffix` in everyone's install id (`acceler-presales@acceler-local`). Renaming it breaks existing installs.
- **This repo is public.** `knowledge/` contains client names, instructor LinkedIn/PII, and pricing data that is now openly readable and clonable by anyone — that was a deliberate call to allow install without collaborator access. Don't add anything more sensitive than what's already there without checking with Utkarsh first, and don't treat "it's already public" as license to add more PII than the engagement actually requires.
- `knowledge/instructor_delivery_flags.json` records who is pre-sales-only vs deliverable — keep it current as engagements land or fall through.
