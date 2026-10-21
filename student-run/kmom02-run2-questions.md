# kmom02 student run (run 2) — questions and friction

Run date: 2026-10-21
Persona: capable student. Source: `kmom02.mdx`, `kunskap/ett_fargschema_for_var_webbshop.mdx`, `kunskap/rendera_markdown_i_javascript.mdx`, `uppgifter/typsnitt_och_farg_del_2.mdx`, `uppgifter/webbshoppen_del_2.mdx`.
Work is done in `efostud/` (not `webshop-efostud/`, see Q1) on branch `kmom02-run2`, branched from `origin/kmom01-run2`.
Compared with run 1 (`kmom02-questions.md`): the material now fixes the old Q3 (id), Q5/Q6 (markdown example, pinned `@18`, security note), Q7/Q8 (category values and filter hint), Q9, Q10, Q11, Q12 (devtools hint) and the known-limitations note for Q4. Those are not repeated.

## Open questions

### Q1. Skill assumes `webshop-efostud/`, the fork lives in `efostud/` and has only `main` locally
- **Where:** `.claude/skills/student-run`
- **Type:** tooling
- **What I did about it:** Asked the teacher; used `efostud/`, fetched, branched from `origin/kmom01-run2`.
- **Suggested fix:** Update the skill's path, or state where the fork should be.

### Q2. `yt-dlp` and `gh` are not installed
- **Where:** skill (videos, hand-in)
- **Type:** tooling
- **What I did about it:** Videos were **not** read this time (the "done" demo, `caw-OVXaaB8`), so nothing about on-screen content is verified. The branch is pushed; PR creation needs `gh` (teacher to install) or the compare URL `https://github.com/efostud/webshop/pull/new/kmom02-run2`.

### Q3. Which branch to start kmom02 from is still not stated (was run 1 Q1)
- **Where:** `kmom02.mdx`, "En branch för kmom02"
- **Type:** unclear
- **What I wondered / got stuck on:** `main` of the fork lacks the kmom01 work (no `index.html` content), but `cp index.html category.html` and `cp reports/kmom01.md ...` need it. The page only says `git checkout -b kmom02` "stå i ditt repo".
- **What I did about it:** Branched from the kmom01 branch; the PR will also show kmom01 commits.
- **Suggested fix:** Say which branch to stand on, and how the PR diff is expected to look until kmom01 is merged.

### Q4. Exercise still says to move `main.js` into `views/` (was run 1 Q2)
- **Where:** `kunskap/ett_fargschema_for_var_webbshop.mdx`, "Tema-väljare", and the `<script src="views/main.js">` in the `category.html` listing
- **Type:** contradiction
- **What I wondered / got stuck on:** Moving `main.js` breaks `index.html` (it loads `./main.js`) and the import `./models/auth.js` (becomes `../models/auth.js`). `main.js` also renders all products into `#product-list`, which `category.html` does not want. The markdown exercise's listing imports `./models/auth.js`, which only works from the root, so it contradicts the `views/` placement.
- **What I did about it:** Kept `main.js` for `index.html`, created `views/category.js` (imports `../models/auth.js`) for the category page.
- **Suggested fix:** Write the intended structure explicitly (e.g. "`main.js` ligger kvar, skapa `views/category.js`"), and use `../models/auth.js` in the markdown listing if it lives in `views/`.

### Q5. `cp index.html category.html` copies a grid and a hero that do not fit the category page
- **Where:** exercise "CSS-variabler" vs. `webbshoppen_del_2.mdx` req. 4
- **Type:** missing
- **What I wondered / got stuck on:** The copy contains the `.product-list` grid (thumbnail cards) and the hero image. Req. 4 wants title, image and the *whole* description for three albums, which is a different layout. First render with the copied grid gave three narrow columns with one-letter-wide text. Needed a new container class and CSS.
- **What I did about it:** `.album-list` + `.album` layout, removed the hero.
- **Suggested fix:** One sentence in the spec that the list layout must be adapted, since descriptions are long.

### Q6. Exercise uses `index.html` where only `category.html` exists
- **Where:** exercise end of "Ett färgschema" ("hämtas in i `index.html` efter `variables.css`")
- **Type:** unclear
- **What I wondered / got stuck on:** The page only edits `category.html`, so `index.html` has no `variables.css` link, and a `var(--...)` in the shared `style.css` is undefined there. The spec does not say whether the theme must also work on the front page, and a saved mode is otherwise ignored there (was run 1 Q14).
- **What I did about it:** Added `variables.css`, the selector and `mode-selector.js` to `index.html` too, and a link to `category.html`.
- **Suggested fix:** State that every page loads `variables.css` (and the selector, or not).

### Q7. Light/dark are defined relative to the old hard-coded colours, no guidance on contrast
- **Where:** `webbshoppen_del_2.mdx` req. 2-3
- **Type:** missing
- **What I wondered / got stuck on:** Spec says "varierar bakgrundsfärg och textfärg" but not about the accent colours. My violet (`#9b59d0`) on white for description headings is a weak contrast (about 4:1). No contrast checking tool or WCAG threshold is mentioned, although the book chapter/kmom is about this.
- **What I did about it:** Used it anyway and noted it in the reflection.
- **Suggested fix:** Mention a contrast checker and the 4.5:1 target in the spec or the exercise.

### Q8. Descriptions in the API: emojis, `hr`, ordered lists
- **Where:** `rendera_markdown_i_javascript.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** The page covers the `###` headings, but descriptions also render `<hr>`, `<ol>` and emojis; with `* { margin: 0 }` from the starter `style.css`, lists have no spacing or markers outside the box until styled (list numbers sat outside the text column).
- **What I did about it:** Styled `.album-description ul`/`li`; left the `ol` as is.
- **Suggested fix:** Mention that the reset in `style.css` removes list padding.

### Q9. Time on the report: measurements cannot be taken by an agent
- **Where:** `typsnitt_och_farg_del_2.mdx` req. 2.3 (Inspect > Computed)
- **Type:** tooling
- **What I did about it:** Reused the values from the stylesheets (as in kmom01 method) and said so in the report; margins not read. A real student can do this in the browser.

### Q10. Reading and Canvas video not possible
- **Where:** `kmom02.mdx`, "Läsa & titta"
- **Type:** other
- **What I did about it:** Skipped chapters 3 and 5 and the sustainability video (login). Reflection Q1 and the TIL are marked as simulated in `reflections/kmom02.md`.

### Q11. PR not created
- **Type:** tooling
- **What I did about it:** See Q2. Not merged, nothing submitted.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
| --- | --- | --- |
| Läsa & titta | 6 h | Not done |
| Öva | 6 h | ~30 min for a capable student, plus ~15 min for adapting layout (Q4, Q5) |
| Uppgifter | 6 h | ~30 min |
| Reflektera | 1 h | Simulated |

## Things that worked well
- The theme switcher code in the exercise now works as written (id fixed) and the known-limitations notes are clear.
- Category values and the `&category=` URL parameter in the spec made the data fetch trivial (verified: returns the three `hiphop` albums).
- `marked@18` from jsDelivr still returns HTTP 200; the example is valid and the `div.album-description` advice is useful.
- Lint (`npm test`) passes; the category page renders correctly in headless Chrome in the light theme (dark theme and persistence not clicked through).
