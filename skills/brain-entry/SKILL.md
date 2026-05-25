---
name: brain-entry
description: Formats new entries into a company brain (markdown vault) with correct frontmatter, breadcrumb chain, concise body, lowercase-kebab-case slug, and matching MOC update. Uses the current working directory when it is the brain root, otherwise uses the explicit brain path from Claude config. Use when user invokes /brain-entry, says "log decision", "log this", "add to brain", "capture this", "save to brain", "log a process", "add team member", "add customer", or makes any explicit request to capture something into their company brain.
---

# Brain Entry

Formats new entries into a company brain markdown vault. The caller (Claude in conversation) provides the content and target folder; this skill enforces structure so every entry follows the brain's best practice.

## Quick start

User: `/brain-entry log a decision: switching to weekly standups, daily was killing focus`

Result:
- New file: `decisions/switching-to-weekly-standups.md`
- Frontmatter (`created`, `type: decision`), breadcrumb chain, H1, concise body
- `decisions.md` updated with a wikilink under `## Contents`
- Confirmation message with a clickable wikilink

## Rules on every entry

1. **Concise body.** Trim filler ("I was thinking", "basically", "kind of"). Capture WHAT (the fact/decision/event), WHY if non-obvious (the reason), plus any concrete numbers, dates, names, or outcomes. Don't write what wasn't said.

2. **Frontmatter** on every new file:
   ```yaml
   ---
   created: YYYY-MM-DD
   type: decision|process|reference|team|customer|moc
   ---
   ```

3. **Brain root and root MOC.** Before writing, identify BRAIN_ROOT and ROOT_MOC_FILE:
   - If the current working directory contains `CLAUDE.md` and a root-level `.md` file whose H1 contains "Company Brain", use the current working directory as BRAIN_ROOT.
   - Otherwise, read the explicit brain reference from the nearest available Claude config, including `./CLAUDE.md` or `~/.claude/CLAUDE.md`, when it says `read /absolute/path/<name>.md for business context`.
   - Use the parent folder of that referenced file as BRAIN_ROOT and the referenced filename as ROOT_MOC_FILE.
   - If no explicit reference exists, fall back to the current working directory and `company-brain.md`.
   Use the filename without `.md` as ROOT_MOC_BASENAME in breadcrumbs.

4. **Breadcrumb on the first line after frontmatter.** This is a standalone vault — use short-form wikilinks, not path-prefixed ones.
   - Subfolder note: `up:: [[ROOT_MOC_BASENAME|ROOT_MOC_BASENAME]] / [[<subfolder>|<subfolder>]]`
   - Root-level note: `up:: [[ROOT_MOC_BASENAME|ROOT_MOC_BASENAME]]`

   `[[ROOT_MOC_BASENAME|ROOT_MOC_BASENAME]]` resolves to the root MOC at the vault root. `[[decisions|decisions]]` resolves to `decisions.md` at the vault root. Do NOT use path-prefixed links like `[[company-brain/decisions|decisions]]` — this would look for a nested subfolder inside the vault.

5. **Slug.** Lowercase-kebab-case. Always. Never `Proper Case`, `snake_case`, or `camelCase`.

6. **Update the matching folder MOC.** Write files under BRAIN_ROOT. Add `- [[<slug>|<slug>]] — <one-line description>` under `## Contents` in `BRAIN_ROOT/<folder>.md`. Insert alphabetically to keep the MOC tidy.

7. **Confirm.** Tell the user where the file landed, with a clickable wikilink.

## Routing

The caller (Claude in conversation) decides which folder. Default scaffold folders: `operations`, `marketing`, `strategy`, `team`, `processes`, `customers` (or `clients`), `decisions`. Custom folders are fine — do not hardcode the list.

If the target is ambiguous, ask ONE clarifying question. Never silently misfile.

| Input | Folder | Notes |
|---|---|---|
| "log decision: switching standups" | `decisions/` | Clear decision |
| "Sarah from Acme is now our point of contact" | `customers/` | Append to `acme.md` if it exists |
| "new SOP for sales call followups" | `processes/` | |
| "team decision about marketing budget cut" | ambiguous | Ask: `decisions/` or `marketing/`? |
| "customer complaint that changed strategy" | ambiguous | Ask: `customers/` or `strategy/`? |

## Collisions

- **Slug already exists in target folder** → append to the existing file using the date-stamped pattern:
  `**[YYYY-MM-DD]** <new insight> (from [[source]])` *(source wikilink optional)*
  Do NOT overwrite. Do NOT create suffixed variants like `foo-2.md`.
- **MOC entry already exists** → silently skip (idempotent).
- **Reserved filenames — never overwrite:** `CLAUDE.md`, `README.md`, the root MOC, and any root-level MOC (`operations.md`, `marketing.md`, `strategy.md`, `team.md`, `processes.md`, `customers.md`/`clients.md`, `decisions.md`).

## Safety

Never use `rm -rf`, recursive deletes, force deletes, or permanent delete commands against the brain. Use Finder/file manager Trash or Recycle Bin for removal, and use `mv` only for safe renames or moving known installer files to trash after verification.
