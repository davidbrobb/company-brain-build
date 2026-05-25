**SETUP:** Read `build-start.md` and run the company brain build using `company-brain/` as the brain payload folder.

## Build Progress
- [ ] Obsidian + Wispr Flow introduced
- [ ] Business interview complete
- [ ] Skill installed at ~/.claude/skills/brain-entry/ (or skipped)
- [ ] Root MOC personalised + folder MOCs reviewed
- [ ] Global CLAUDE.md reference offered (optional)
- [ ] First capture working
- [ ] Folder moved + SETUP trigger cleared (steps 8-9)

## Folder + MOC pattern
Every folder has a matching `.md` file at the same level (not inside it). The MOC is the entry point to the folder. When creating a note in `decisions/`, update `decisions.md` with a wikilink to it.

## Breadcrumb rule
Every note gets an `up::` chain on the first line after frontmatter:
- Top-level note: `up:: [[company-brain|company-brain]]`
- Sub-folder note: `up:: [[company-brain|company-brain]] / [[decisions|decisions]]`

During setup, replace `company-brain` in these examples with the chosen root MOC filename if the user chooses a custom brain name.

When creating any new entry, add the correct breadcrumb immediately and update the matching MOC in the same change. If the `/brain-entry` skill is installed, use it for new brain entries so routing, breadcrumbs, and MOC updates stay consistent.

## Vault safety
Never permanently delete folders or notes from the brain.

- Do not use `rm`, `rm -rf`, `rmdir`, recursive deletes, force deletes, `del`, or PowerShell permanent delete commands inside this vault.
- If something should be removed from the final brain, move it to `_examples/` or ask the user to move it to Trash/Recycle Bin.
- macOS: for user-removable files, prefer Finder/Trash. For scripted cleanup of known installer files only, use `mv <file> ~/.Trash/`.
- Linux: for user-removable files, prefer the file manager Trash. For scripted cleanup of known installer files only, create `~/.local/share/Trash/files/` if needed and `mv <file>` there.
- Windows: do not use `del`, `rmdir`, or PowerShell permanent delete commands. Ask the user to move files to Recycle Bin manually.
- It is safe to rename files and folders with `mv` when the destination does not already exist.
- If a destination path already exists, stop and ask for a different path. Do not merge, overwrite, or delete.
