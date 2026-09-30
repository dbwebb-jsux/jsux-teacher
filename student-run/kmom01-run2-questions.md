# kmom01 student run 2 — questions and friction

Run date: 2026-09-30
Persona: capable student. Source: `kmom01.mdx` as updated (written setup steps, `reflections/kmomNN.md`).
Work is done in `webshop-efostud` on branch `kmom01-run2` (branch `kmom01` and PR #1 from run 1 are left as they are).
API key reused from run 1 (products already copied, `GET /v2/products` returns 12).
Issues already logged in run 1 (`kmom01-questions.md`, TODO #15-#19) are not repeated here unless they behave differently.

## Setup notes for this run
- The fork's workflows are now enabled (four workflows: Agent policy check, Report grade to Canvas, Node.js CI, Deploy static content to Pages). GitHub Pages was still not configured (Pages API 404) at the start of the run.
- The fork's `main` had no `reflections/` folder; synced by fast-forward from upstream `main` before branching (a student forking after the change gets it automatically; an existing fork needs "Sync fork").
- Branch is called `kmom01-run2` (PR #1 already uses `kmom01`). Names starting with `kmom` still match the deploy rule and the Canvas workflow filter.

## Open questions

### R1. Deploy job fails with a cryptic error if Pages is not enabled
- **Where:** `kmom01.mdx` fork steps ("Slå på GitHub Pages"), hand-in video, `.github/workflows/static-deploy.yml`
- **Type:** tooling
- **What I wondered / got stuck on:** The PR checks show green for "Node.js CI" and "Agent policy check", but "Deploy static content to Pages" fails with `Get Pages site failed. Please verify that the repository has Pages enabled and configured to build using GitHub Actions`. This happens when step 3 (Settings > Pages > Source: GitHub Actions) was skipped. A student sees a red cross on their first push and on the PR and may not connect it to the setup step. The failure also appears on every push to `main`.
- **What I did about it:** Left it failing (needs the teacher to set Pages in the UI). Web page cannot be verified live (see R5).
- **Suggested fix:** Add to the setup text: "Efter första pushen ska alla checkar vara gröna. Om 'Deploy static content to Pages' är röd, gå tillbaka till steg 3." Also mention in the hand-in section which checks should be green.

### R2. Existing forks do not get changes made to the starter repo
- **Where:** starter repo `webshop`, `kmom01.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** The new `reflections/` folder is only in forks created after the change. My fork from run 1 lacked it, and I had to fast-forward `main` from upstream. If the starter or a template is fixed during the course (which is likely, e.g. lint fixes), students have no instructions for pulling those updates.
- **What I did about it:** Fast-forwarded the fork's `main` from upstream and branched from it.
- **Suggested fix:** Add a short "Uppdatera din fork" section (GitHub's "Sync fork" button, then `git pull` on `main` and merge/rebase into the kmom branch), and decide who announces template changes.

### R3. Proprietary fonts on the chosen shop
- **Where:** `uppgifter/typsnitt_och_farg.mdx`, `uppgifter/webbshoppen_del_1.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** IKEA uses its own font ("Noto IKEA"), and other shops use paid Typekit/Adobe fonts (I saw one in run 1 too). The assignment says to be inspired by the shop's typography, but nothing says what to do when the font is not available: pick a similar free Google Font? How to add it (no page shows the `<link>` to Google Fonts or a local `@font-face`)?
- **What I did about it:** Documented the real font in the report and used Noto Sans from Google Fonts in the shop.
- **Suggested fix:** Add a short section on web fonts (Google Fonts `<link>` or self-hosted, and the sustainability trade-off). Ties naturally to the sustainability theme.

### R4. Product cards get uneven heights with the exercise CSS
- **Where:** `kunskap/forstasidan_for_en_webbshop.mdx`, "Produktlistning"
- **Type:** other
- **What I wondered / got stuck on:** The album covers have different proportions, so a card grid with `img { width: 100% }` gives cards of different heights. The page's own layout (image left of text, 20% width) hides this, but any student who makes a grid will hit it.
- **What I did about it:** Used `aspect-ratio: 1` and `object-fit: cover` on `.product img`.
- **Suggested fix:** Mention `aspect-ratio`/`object-fit` for images in a list (also good for layout shift and performance).

### R5. How can a student check the finished page and the async product list?
- **Where:** my own tooling, but relevant for the text
- **Type:** other
- **What I wondered / got stuck on:** A headless Firefox screenshot is taken before `fetch` returns, so the product list was empty in my screenshot. I verified the JS by running `main.js` against a stub `document` in Node (12 products rendered) and the layout by screenshotting a static copy. Students will just open the page in their browser, so this is only relevant for teacher automation.
- **What I did about it:** See above.
- **Suggested fix:** None for the course.

## Reflections folder check (TODO #20)
- Templates exist for kmom01-06 and kmom10 in the starter; the kmom01 template has the three questions from the page.
- Page and template agree on the location. The "Obs" note under the hand-in video explains the difference from the video. I found no other place in kmom01 or the uppgifter specs that still mentions the PR description.
- `reflections/kmom01.md` lint/CI: not linted (only html, css, js), so a student cannot get feedback on empty templates. A student who forgets to fill in a placeholder ("Skriv ditt svar här.") still passes all checks.
  - **Suggested fix (optional):** A small CI check that fails or warns if a reflection still contains the placeholder text.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
|---|---|---|
| En plats att koda på | 1 h | fork already existed; sync with upstream took 2 min |
| Öva + Uppgifter | 12 h | about 40 min (second time, lint-clean code written directly; different shop) |
| Reflektera | 1 h | 10 min |

## Things that worked well
- `npm test` passes on the first try when the code follows the repo's style; no need to consult the page snippets a second time.
- The reflections template makes the hand-in clearer: three files (`reflections/kmom01.md`, `reports/kmom01.md`, the site) and one PR.
- CI ran for the fork now (Node.js CI, Agent policy check green).
