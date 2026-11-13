# Maintaining this repo

Notes for me, Fede. Short on purpose.

## Update a skill

Example: The Office RPG goes from 3.4 to 3.5.

1. **Edit** the files in `skills/the-office-rpg/` (`SKILL.md`, `references/`, `assets/`).
2. **Bump the version** in three places:
   - `SKILL.md`, in the top block: `version: "3.5"`
   - `skills/the-office-rpg/README.md`: the version badge and the "Version 3.4" line at the bottom
3. **Rebuild the zip.** It must hold one folder, `the-office-rpg/`, with `SKILL.md`, `references/` and `assets/` inside. No README. From the repo root:

   ```bash
   cd skills && rm -f the-office-rpg/the-office-rpg.zip && zip -r -X the-office-rpg/the-office-rpg.zip the-office-rpg/SKILL.md the-office-rpg/references the-office-rpg/assets -x "*.DS_Store" && cd ..
   ```

   No terminal? Ask Copilot or Claude: "Rebuild the Office RPG zip as described in MAINTAINING.md."
4. **Commit and push** to `main`. The download link in the article keeps working, it always serves the latest zip.
5. **New release** (optional but nice): on GitHub, go to **Releases > Draft a new release**. Tag: `the-office-rpg-v3.5`. Attach the new zip. Write 3 lines on what changed.

## Add the next skill

1. Make a folder `skills/<skill-name>/` with `SKILL.md`, `references/`, `assets/` and a `README.md`. Copy the Office RPG README as a starting point.
2. Build `skills/<skill-name>/<skill-name>.zip` the same way as above.
3. Add one row to the table in the root `README.md`:

   ```markdown
   | <a href="skills/<skill-name>"><img src="skills/<skill-name>/assets/cover.png" width="160" alt="Short description of the cover"></a> | ✨ **[Skill Name](skills/<skill-name>)** | One line on what it does. | [⬇️ Download](https://github.com/fedepacheco/tbc/raw/main/skills/<skill-name>/<skill-name>.zip) |
   ```

4. The download link for the article is always:
   `https://github.com/fedepacheco/tbc/raw/main/skills/<skill-name>/<skill-name>.zip`

## Writing rules

Plain words, short sentences, no em dashes. Images under 1 MB, always with alt text. Max 3 badges per README. End every README with: "I work at Microsoft. The skills and opinions here are my own."
