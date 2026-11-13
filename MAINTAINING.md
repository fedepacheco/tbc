# Maintaining this repo

Notes for me, Fede. Short on purpose.

## How it's organised

- **The front page (`README.md`) is organised by article.** Each article gets its own section under **Articles**, newest at the top.
- **The files are organised by type**, so download links never break:
  - `skills/<name>/`: Copilot Cowork skills (a folder with `SKILL.md`, plus a `.zip` to download)
  - `prompts/<name>/`: plain prompts to copy and paste (create this folder the first time you need it)
- `assets/brand/`: logo, banner and the social preview image.

One article can point to several skills or prompts. A skill can show up in more than one article.

## Add a new article

1. Add the skill or prompt files (see below).
2. In `README.md`, copy the `### No.04 · The Office RPG` block and paste it **above** it, right under `## Articles`. Then change:
   - the number and title: `### No.05 · <Title>`
   - the cover image: put it in the skill's `assets/` folder as `<name>-cover.jpg`, under 1 MB, and write a short description in `alt="..."`
   - one line on what it's about
   - the table: one row per skill or prompt
   - `📖 The article: coming soon.` becomes `📖 [Read the article](<LinkedIn link>)` once it's live
3. Change the big **⬇️ Download** link near the top to point at the newest skill.

## When an article goes live

Replace `The article: coming soon.` with `[Read the article](<LinkedIn link>)` in two places: the article's section in `README.md` and the skill's own `README.md`.

## Add a skill

1. Make a folder `skills/<name>/` with `SKILL.md`, `references/`, `assets/` and a `README.md`. Copy the Office RPG README as a starting point.
2. Build the zip (see below).
3. The download link is always:
   `https://github.com/fedepacheco/tbc/raw/main/skills/<name>/<name>.zip`

## Update a skill

Example: The Office RPG goes from 3.4 to 3.5.

1. **Edit** the files in `skills/the-office-rpg/`.
2. **Bump the version** in `SKILL.md` (`version: "3.5"`) and in the skill's `README.md` (the version badge and the "Version 3.4" line at the bottom).
3. **Rebuild the zip.** It holds one folder, `the-office-rpg/`, with `SKILL.md`, `references/` and only the assets the skill itself links to (for the Office RPG that's `assets/how-it-works.png`). The README pictures stay out, so the download stays small. From the repo root:

   ```bash
   cd skills && rm -f the-office-rpg/the-office-rpg.zip && zip -r -X the-office-rpg/the-office-rpg.zip the-office-rpg/SKILL.md the-office-rpg/references the-office-rpg/assets/how-it-works.png -x "*.DS_Store" && cd ..
   ```

   No terminal? Ask Copilot or Claude: "Rebuild the Office RPG zip as described in MAINTAINING.md."
4. **Commit and push** to `main`. The download link in the article keeps working. It always serves the latest zip.
5. **New release** (optional but nice): on GitHub, go to **Releases > Draft a new release**. Tag: `the-office-rpg-v3.5`. Attach the new zip. Write 3 lines on what changed.

## Writing rules

Plain words, short sentences, no em dashes. Images under 1 MB, always with alt text. Max 3 badges per README. Brand colours: navy `#00009C`, pink `#FF009C`, yellow `#FCDF00`, cyan `#00C5FF`. End every README with: "I work at Microsoft. The skills and opinions here are my own."
