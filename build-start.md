# Company Brain Build — Setup Script

This file contains instructions for Claude to execute during first-time setup. Follow each step in order. Do not skip steps. The extracted kit root contains this file, `skills/`, and a `company-brain/` payload folder. All brain edits happen inside `company-brain/`; installer files stay outside the final brain.

At the start of setup:
- Set PACKAGE_PATH to the folder containing this `build-start.md`.
- Set BRAIN_PATH to `PACKAGE_PATH/company-brain`.
- Track setup progress in `PACKAGE_PATH/CLAUDE.md` until Step 8 moves that file into the final brain folder.

---

## Safety rules for file operations

Use these rules for the entire setup.

- Do not permanently delete folders or notes from the brain. If something should be removed from the final brain, move it to `_examples/` or ask the user to move it to Trash/Recycle Bin.
- Do not use `rm -rf`, `rmdir`, recursive deletes, or force deletes.
- macOS: for user-removable files, prefer Finder/Trash. For scripted cleanup of known installer files only, use `mv <file> ~/.Trash/`.
- Linux: for user-removable files, prefer the file manager Trash. For scripted cleanup of known installer files only, create `~/.local/share/Trash/files/` if needed and `mv <file>` there.
- Windows: do not use `del`, `rmdir`, or PowerShell permanent delete commands. Ask the user to move files to Recycle Bin manually.
- It is safe to rename files and folders with `mv` when the destination does not already exist.
- If a destination path already exists, stop and ask for a different path. Do not merge, overwrite, or delete.
- Always verify that `CLAUDE.md` and the root MOC exist at the final path before clearing the SETUP trigger.

---

## Step 0 — Welcome + Obsidian + Wispr Flow

Tell the user:

> "Welcome to the Company Brain Build. Before the interview, two tools worth grabbing now so they're ready when you need them:
>
> **1. Obsidian** (free — obsidian.md): Install it now. Once setup is complete, Claude will tell you exactly which folder to open as a vault — you'll see all your notes as a connected graph. Don't open it yet, just install it.
>
> **2. Wispr Flow** (optional — wisprflow.ai, free trial): If you'd prefer to talk instead of type, grab this too. Hold a key and dictate straight into Claude Code — great for the interview, even better for daily captures once the brain is live.
>
> Install either, both, or neither — then say the word and we'll start the interview."

Proceed as soon as the user indicates they're ready. Do not wait on tool downloads if they want to skip and start immediately.

Tick: `- [ ] Obsidian + Wispr Flow introduced` in `PACKAGE_PATH/CLAUDE.md`.

---

## Step 1 — Business interview

Ask these questions one at a time. Do not batch them. Wait for each answer before asking the next. There is no length constraint on answers — the more context, the better the brain design.

1. "What's the business called?"
2. "Tell me about it — what do you do, who do you help, and how does it actually work day-to-day?"
3. "How many people are on the team, including you?"
4. "Do you work with individual clients or customers, or do you sell a product to many people at once?"
5. "Your brain folder will be named **`company-brain`** and saved to `~/company-brain/`. Type `default` to use that, or give me a different name or full path."

Store **BRAIN_NAME** and **FINAL_PATH** from question 5:
- If they say "default": BRAIN_NAME = `company-brain`, FINAL_PATH = `~/company-brain/` expanded to the absolute path
- If they type just a name (e.g. `acme-brain`): BRAIN_NAME = `acme-brain`, FINAL_PATH = `~/acme-brain/` expanded to absolute path
- If they type a full path (e.g. `~/Documents/my-brain`): FINAL_PATH = that path expanded to absolute, BRAIN_NAME = the last segment of the path
- Set ROOT_MOC_BASENAME = BRAIN_NAME and ROOT_MOC_FILE = `BRAIN_NAME.md`. The shipped root MOC starts as `company-brain.md`; rename it during Step 4 if BRAIN_NAME is different.

Tick: `- [ ] Business interview complete` in `PACKAGE_PATH/CLAUDE.md`.

---

## Step 2 — Install the /brain-entry skill

Say:

> "I'm going to install a skill at `~/.claude/skills/brain-entry/` so you can run `/brain-entry` in any future Claude Code session to capture things into your brain with the right structure. OK to proceed?"

Set SKILL_INSTALLED based on the user's answer.

If **yes:**
- Create `~/.claude/skills/brain-entry/` if it doesn't exist
- If `~/.claude/skills/brain-entry/SKILL.md` already exists, ask before overwriting: "There's already a brain-entry skill installed. Overwrite it with this version?"
- Copy `PACKAGE_PATH/skills/brain-entry/SKILL.md` to `~/.claude/skills/brain-entry/SKILL.md`
- Set SKILL_INSTALLED = true
- Confirm: "Skill installed."

If **no:**
- Set SKILL_INSTALLED = false
- Skip and note: "No problem — you can capture notes ad-hoc. I'll demonstrate the format in Step 6."

Tick: `- [ ] Skill installed at ~/.claude/skills/brain-entry/ (or skipped)` in `PACKAGE_PATH/CLAUDE.md`.

---

## Step 3 — Customise the folder structure

Based on the business interview, decide which default folders to keep or rename.

**Default set:** `operations/`, `marketing/`, `strategy/`, `team/`, `processes/`, `customers/`, `decisions/`

**Rules:**
- Apply all folder and MOC changes inside `BRAIN_PATH`, not at the package root.
- Keep `customers/` and `decisions/` by default. Do not offer to drop either one.
- If the user says "clients" instead of "customers", rename `BRAIN_PATH/customers` to `BRAIN_PATH/clients` AND `BRAIN_PATH/customers.md` to `BRAIN_PATH/clients.md`.
- After renaming `customers` to `clients`, update `clients.md` H1 from "Customers" to "Clients", update the description language if needed, and update every breadcrumb inside `clients/` from `[[customers|customers]]` to `[[clients|clients]]`.
- For any other rename, apply the same pattern: folder + matching root MOC + H1 + breadcrumbs in notes inside the renamed folder.
- If team size > 1, say: "I can create a starter note for each teammate now, or keep the example team note and you can replace it later. If you want starter notes, send the names." Create teammate stubs only for names the user provides. If they do not want to provide names, keep `team/example-team-member.md`.
- Do not remove placeholder example files during this step. They will be moved into `_examples/` in Step 7 after the first real capture exists.

**Decide the proposed structure first, then show it before touching anything.** Present the full proposed folder list and ask: "Here's your folder structure: [list]. Anything to rename?" Apply their feedback, then confirm, then apply filesystem changes. No `mv` until the user approves.

---

## Step 4 — Personalise the root MOC

If BRAIN_NAME is not `company-brain`, rename `BRAIN_PATH/company-brain.md` to `BRAIN_PATH/ROOT_MOC_FILE` before rewriting it.

Rewrite `BRAIN_PATH/ROOT_MOC_FILE` with:
- H1 title: the business name from question 1 of the interview, or "Company Brain" if they prefer generic
- No frontmatter (root MOC exception)
- Strip the `<!-- Claude will personalise this title... -->` comment entirely
- A `## Contents` section with wikilinks to each surviving subfolder MOC, using the **short-form** format: `- [[<folder>|<folder>]] — <one-line description of what goes here>`
  (e.g. `- [[decisions|decisions]] — The calls you've made and why`)
  Do NOT use path-prefixed links like `[[acme-brain/decisions|decisions]]` — in a standalone vault, these resolve to a non-existent nested subfolder.

For each surviving folder MOC stub in `BRAIN_PATH` (`operations.md`, `customers.md`, etc.) AND each placeholder note inside surviving subfolders (`example-decision.md`, etc.):
- Update the `created:` date to today's date (YYYY-MM-DD format), replacing `PLACEHOLDER-DATE`
- Replace every root breadcrumb `[[company-brain|company-brain]]` with `[[ROOT_MOC_BASENAME|ROOT_MOC_BASENAME]]`, substituting the actual BRAIN_NAME.

In `PACKAGE_PATH/CLAUDE.md`, update the Breadcrumb rule examples to use `[[ROOT_MOC_BASENAME|ROOT_MOC_BASENAME]]` instead of `[[company-brain|company-brain]]`, and replace the setup note under the examples with: `The root wikilink points to ROOT_MOC_BASENAME because that is the filename of the root MOC (ROOT_MOC_FILE), which Obsidian resolves by name.`

Tick: `- [ ] Root MOC personalised + folder MOCs reviewed` in `PACKAGE_PATH/CLAUDE.md`.

---

## Step 5 — Offer to add reference to global Claude config (OPTIONAL)

Say:

> "Want me to add one line to your global Claude config so I always know about your brain when you open Claude Code anywhere? It just adds: `At the start of each session, read [the actual path]/[root MOC file] for business context.` You can say no — the brain works either way."

(Substitute the actual FINAL_PATH value in the displayed text — the founder should see their real path, not a placeholder.)

If **yes:**
- Check if `~/.claude/CLAUDE.md` exists. If not, create it with just the new line.
- Search for any existing line referencing `ROOT_MOC_FILE` or FINAL_PATH. If found, skip (no duplicates). Tell the user it's already there.
- Otherwise, append the line to the end of the file.
- Confirm: "Added to your global Claude config."

If **no:**
- Confirm: "Skipped. You can always add it manually later."

Tick: `- [ ] Global CLAUDE.md reference offered (optional)` in `PACKAGE_PATH/CLAUDE.md`.

---

## Step 6 — First capture (prove it works)

Say:

> "Let's make your first entry so you can see how it works. Tell me one decision you've made this week."

Create the file manually using the correct format — this works regardless of whether the skill was installed:
- File goes in `BRAIN_PATH/decisions/`
- Frontmatter: `created: today's date`, `type: decision`
- Breadcrumb: `up:: [[ROOT_MOC_BASENAME|ROOT_MOC_BASENAME]] / [[decisions|decisions]]`
- H1: a short slug of the decision
- Body: concise — what was decided and why
- Update `BRAIN_PATH/decisions.md` Contents with a wikilink to the new file

Show the created file to the user — read it back to them and confirm the file path so they can see exactly what was written and where it landed.

If SKILL_INSTALLED = true, then say:

> "That's how every entry works. From now on, use `/brain-entry` in any Claude Code session to capture things the same way. If `/brain-entry` isn't available in a new session, it means Claude Code needs a restart to load the skill — just restart and it'll be there."

If SKILL_INSTALLED = false, then say:

> "That's how every entry works. Because we skipped the skill install, capture manually by asking me to log something to the brain. I'll use this same structure."

Tick: `- [ ] First capture working` in `PACKAGE_PATH/CLAUDE.md`.

---

## Step 7 — Move examples out of the working brain

After the first real capture exists, move placeholder example notes into `_examples/` so the live folders do not look unfinished.

- Create `BRAIN_PATH/_examples/`.
- Move each `example-*.md` file from surviving subfolders into `_examples/`.
- If a folder was renamed, keep the example's updated breadcrumbs from Step 3.
- Do not delete example files.

This MUST happen before the final move in Step 8.

---

## Step 8 — Move the folder safely (commit point)

Move FIRST. Clear the SETUP trigger only AFTER the move is verified.

Before moving the brain folder, move the single project `CLAUDE.md` into the brain payload:
- Source: `PACKAGE_PATH/CLAUDE.md`
- Destination: `BRAIN_PATH/CLAUDE.md`
- Verify `BRAIN_PATH/CLAUDE.md` does not already exist before moving it. If it does exist, stop and ask the user before overwriting.
- Use a safe rename/move only. Do not copy it and leave a duplicate behind.

**8a.** Use the brain payload folder as the source:
```bash
SOURCE_PATH="$BRAIN_PATH"
```

Do NOT move the whole extracted kit folder. Move only the `company-brain/` payload folder. The package root, `build-start.md`, and `skills/` are installer materials and should stay outside the final brain.

**8b.** If `SOURCE_PATH` is already `FINAL_PATH`, do not move anything. Continue to verification.

**8c.** If FINAL_PATH exists and is not SOURCE_PATH:
- Stop and ask the user for a different destination, even if the folder is empty. Do not merge, overwrite, or delete.

**8d.** If FINAL_PATH does not exist:
- Ask the user to move the folder manually using Finder or file manager:
  > "Please move the `company-brain` folder to [FINAL_PATH] using Finder or your file manager. When it's there, come back here and say done."

**8e.** Verify `"$FINAL_PATH/CLAUDE.md"` exists.

**8f.** Verify `"$FINAL_PATH/ROOT_MOC_FILE"` exists.

**8g.** If 8e or 8f fails: do NOT clear the SETUP trigger and do NOT delete anything. Tell the user what's missing and ask them to move the `company-brain` folder manually, then say done.

**8h.** Verify installer materials are not inside the final brain:
- `"$FINAL_PATH/build-start.md"` should not exist.
- `"$FINAL_PATH/skills"` should not exist.
- If either exists, move it to Trash/Recycle Bin using the safety rules above. Do not delete permanently.

Never use `rm`, `rm -rf`, `del`, or permanent delete commands here.

---

## Step 9 — Clear SETUP trigger + write brain reference (final step)

Only run this step after Step 8 has fully succeeded and installer materials have been confirmed absent from the final brain.

In `FINAL_PATH/CLAUDE.md`:

1. Find the line that starts with `**SETUP:**` (exact match on the first line — do not search broadly).
2. Replace that single line with: `At the start of each session, read FINAL_PATH/ROOT_MOC_FILE for business context.` (substitute the actual absolute path and root MOC filename).
3. Replace every `- [ ]` with `- [x]` (tick all checkboxes, including `- [ ] Folder moved + SETUP trigger cleared (steps 8-9)`).
4. **Preserve verbatim** the Folder + MOC pattern, Breadcrumb rule, and Vault safety sections — do not reword, reorder, or restructure them. Ticking checkboxes is the only change permitted to those sections.

Confirm to the user (substitute actual FINAL_PATH value in the message):

> "Your brain is live at [FINAL_PATH].
>
> **Two things to do now:**
>
> **1. Open in Obsidian.** Open the Obsidian app → click **Open folder as vault** → navigate to [FINAL_PATH] → click Open. You'll see your brain as a connected graph — the folders, MOCs, and that first decision you just captured.
>
> **2. Use it from Claude Code.** You can keep opening Claude Code the way you normally do. The global reference means I can find the brain from any session. If you skipped the global reference, open Claude Code from [FINAL_PATH] when you want brain-aware work."

If SKILL_INSTALLED = true, add:

> "For captures, say `/brain-entry` followed by anything you want to save and I'll route it into the right folder with the right format."

If SKILL_INSTALLED = false, add:

> "For captures, ask me to log something to the brain and I'll use the same structure manually."
