# kmom01 student run — questions and friction

Run date: 2026-09-30
Persona: capable student. Source: `dbwebb-jsux.github.io/src/content/docs/kmom01.mdx` and linked pages.
Work is done in `webshop-efostud` on branch `kmom01`.

## Open questions

### Q1. Reading (6 study hours) cannot be done by me
- **Where:** `kmom01.mdx`, "Läsa & titta" (Designing Interfaces ch. 1 and 2, e-book via library)
- **Type:** other
- **What I wondered / got stuck on:** The literature and the reflection question about "Designing for People" depend on reading the book.
- **What I did about it:** Skipped. Reflection text will be written from the course page material and marked as simulated.
- **Suggested fix:** None needed for real students. Maybe state how the 6 hours split between reading, videos and lectures.

### Q2. Weekly video (genomgång) looks recorded for the first ever run
- **Where:** `kmom01.mdx` video `oBAUUkz5XG4` (captions)
- **Type:** outdated
- **What I wondered / got stuck on:** The video says the course is "helt ny", "aldrig gått förut", "delar av materialet är inte redo", mentions "sju kursmoment" and a specific Tuesday/Wednesday/Thursday schedule with Mattias and Kenneth. The site has kmom01-06 + kmom10, and the schedule for Nov 2026 may differ.
- **What I did about it:** Nothing, just noted.
- **Suggested fix:** Re-record for 2026 (or add a note above the video that it is from the first run). Ties to TODO #2/#3 (audio).

### Q3. Lager API page: Postman POST body only explained in video
- **Where:** `kunskap/lager_apit.mdx`, "Kopiera produkter"
- **Type:** missing
- **What I wondered / got stuck on:** The text says to send a POST to `/v2/copier/products` with Postman but not what the body must contain. From the video captions: body type `x-www-form-urlencoded`, key `api_key`, value = your key, expect `201 Created` and 12 albums. Without the video a text-first student cannot do it. Also nothing about how to verify (GET `/v2/products?api_key=...`).
- **What I did about it:** Used the video captions and will use `curl` instead of installing Postman (a GUI install and possibly a Postman account is a lot for one POST).
- **Suggested fix:** Write the request out as text (and as a `curl` example) with the expected response. Fits TODO #5.

### Q4. Lager API page: two different places to get the key, and email is sent to an external service
- **Where:** `kunskap/lager_apit.mdx` "En API-nyckel" vs video `fAA9hp5Zjjc`
- **Type:** unclear
- **What I wondered / got stuck on:** Page links `https://lager.emilfolino.se/v2/auth/api_key`; video goes via the docs page "API keys" then "request a personal API key". The video says an email is required so the teacher can contact you about abnormal traffic and recommends the student mail, but the page does not say why the email is needed or which email to use.
- **What I did about it:** Blocked until the teacher tells me which email to use (I will not invent one).
- **Suggested fix:** Add one sentence about why the email is collected and which address is recommended.

### Q5. `code .` assumes Visual Studio Code
- **Where:** `kunskap/lager_apit.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** The page never says VS Code is required or installed. Other editors work fine.
- **What I did about it:** Created the file with the shell and used my own editor.
- **Suggested fix:** "Öppna projektet i din editor (t.ex. `code .` för VS Code)".

### Q6. API key ends up committed and published
- **Where:** `kunskap/lager_apit.mdx` `models/auth.js`; `webshop/.gitignore` only ignores `node_modules/` and `.env`
- **Type:** other
- **What I wondered / got stuck on:** `models/auth.js` is not ignored, so the key is committed, pushed and (since the JS is served by GitHub Pages) public. The page says the data is "your own", so it may not be a security problem, but a student learning good habits could reasonably ask whether committing a key is OK. Nothing in the material says it is intentional.
- **What I did about it:** Followed the page (committed `models/auth.js`).
- **Suggested fix:** State explicitly that this key is deliberately low-stakes and why, or mention it in a short "secrets" note. Ties to the later Stripe article.

### Q7. Code snippets conflict with the repo's lint rules
- **Where:** `kunskap/lager_apit.mdx` and `kunskap/forstasidan_for_en_webbshop.mdx` JS snippets vs `webshop/eslint.config.mjs` (`semi: never`, `indent: 2`)
- **Type:** contradiction
- **What I wondered / got stuck on:** `models/auth.js` snippet uses 4-space indentation; the last `main.js` snippet ends with `renderProducts();` (semicolon); the `.map` callback body uses 6 spaces. CI runs `npm test` (eslint) on push/PR, so copy-pasting the page as-is should fail lint. To be confirmed by running `npm test` after pasting.
- **Confirmed:** pasting the page snippets verbatim gives 4 eslint errors (`models/auth.js` indent x2, `main.js` indent and extra semicolon). The `.map` template also has trailing whitespace after `</h3>`.
- **What I did about it:** Fixed to 2 spaces / no semicolons / no trailing spaces.
- **Suggested fix:** Fix the snippets so they pass the repo's lint rules.

### Q8. "Ersätt body-delen" but the snippet contains the whole file, and the CSS `body` rule already exists
- **Where:** `kunskap/forstasidan_for_en_webbshop.mdx` "Grundläggande struktur"
- **Type:** unclear
- **What I wondered / got stuck on:** Says to replace the `body` part of `index.html` but shows the full document. For CSS it shows a full `body { ... }` rule while `style.css` already has a `body` rule; it does not say "replace" or "extend". Adding a second `body` rule may be flagged by stylelint (no-duplicate-selectors). It also never shows the CSS for `.container { flex: 1; }`, only describes it in words.
- **Confirmed:** appending the page's `body` rule to `style.css` gives stylelint errors `no-duplicate-selectors` and `rule-empty-line-before`.
- **What I did about it:** Edited the existing `body` rule instead, and put the generic `p`/`h1`/`h2`/`h3` rules before the component rules because stylelint's `no-descending-specificity` complained when I put `.header p` before `p` (a student customising the page will hit this too).
- **Suggested fix:** Say "ersätt hela filen"/"ändra befintlig regel" explicitly and show the resulting `.container` rule.

### Q9. Hero image: no image provided and `assets/` does not exist
- **Where:** `kunskap/forstasidan_for_en_webbshop.mdx` "En Hero-bild"
- **Type:** missing
- **What I wondered / got stuck on:** The page uses `assets/hero-banner.jpg` from the teacher's own download. The starter repo has no `assets/` directory and no example image. Students must find, license and size an image on their own.
- **What I did about it:** Created my own small SVG (`assets/hero-banner.svg`) because I cannot download or view photos. Also: `css/use-baseline` and the Google Fonts `<link>` for Oswald were needed to reuse the inspiration shop's font; the page never mentions how to add web fonts, which the assignment (typsnitt) invites.
- **Suggested fix:** Provide a default placeholder hero in the starter and state image size/format guidance (also relevant for sustainability, CO2).

### Q10. API response shape is not documented on the page
- **Where:** `kunskap/forstasidan_for_en_webbshop.mdx` "Produktlistning"
- **Type:** unclear
- **What I wondered / got stuck on:** The code uses `result.data` and `product.image_url` and `product.name`. The page says "look at the console" but does not show or link the response shape. To be verified against the real API.
- **Confirmed:** `GET /v2/products?api_key=...` returns `{ data: [...] }` with 12 products; each has `id, article_number, name, description (long markdown), specifiers, stock, location, price, image_url, category`. `result.data`, `image_url` and `name` in the page's code are correct. `price` and `category` are also available but never shown in the exercise.
- **What I did about it:** Used `name` and `image_url` as on the page.
- **Suggested fix:** Show one example product JSON.

### Q11. Where should the webshop inspiration choice be recorded for "Webbshoppen del 1"?
- **Where:** `uppgifter/webbshoppen_del_1.mdx` requirement 1
- **Type:** unclear
- **What I wondered / got stuck on:** "Välj en webbshop ... Samma som i Typsnitt och färg" — but nothing in del 1 says where to show or state the choice. The report `reports/kmom01.md` covers it, presumably.
- **What I did about it:** Will state it in the report only.
- **Suggested fix:** Say "redovisas i rapporten".

### Q12. Requirements of del 1 are open-ended (no definition of done)
- **Where:** `uppgifter/webbshoppen_del_1.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** Four short requirements. How do I know I am done? Product "titel och bild" but the exercise page already produced this, so the assignment is mostly "make it look like your chosen inspiration". How much design work is expected? Nothing on required file list or price display.
- **What I did about it:** Built header (name + tagline), hero image, a responsive grid of products (title + image) and a footer, styled after the analysed shop. Unsure whether that is "enough"; a checklist would tell me.
- **Suggested fix:** TODO #6: add a "definition of done" checklist.

### Q13. Where do the reflection answers go: report file or PR description?
- **Where:** `kmom01.mdx` "Reflektera" vs hand-in video `YD0tE7FW6i0` vs `uppgifter/typsnitt_och_farg.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** The page says "som en del av din inlämning via GitHub svara på frågorna". The hand-in video (captions) copies the questions into the pull request description. But the Typsnitt och färg spec has its own report file `reports/kmom01.md`. A student could reasonably put the TIL in the report.
- **What I did about it:** Following the video: reflection answers go in the PR description. Also mention in the report? (decision pending)
- **Suggested fix:** State the location explicitly on the kmom page. Ties to TODO #9.
- **Resolved 2026-09-30:** reflections now go in `reflections/kmomXX.md` (templates added to the starter, all kmom pages updated). The kmom01 hand-in video still shows the PR description and has an "Obs" note on the page (see TODO #20).

### Q14. Color analysis: tool not specified
- **Where:** `uppgifter/typsnitt_och_farg.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** "Berätta om du använde något särskilt verktyg" but the page gives no suggestion of tools (browser dev tools, color picker extensions, etc.), and how many colors count as "the palette".
- **What I did about it:** Fetched HTML/CSS with `curl` and counted hex codes (Bengans; several other record shops returned 403 to automated requests). A student would use dev tools or a color picker instead.
- **Suggested fix:** Suggest 1-2 tools and a range (e.g. 4-6 colors) to reduce anxiety.

### Q15. Markdown heading style in the report template
- **Where:** `uppgifter/typsnitt_och_farg.mdx` "Rapportstruktur"
- **Type:** other
- **What I wondered / got stuck on:** The template uses underlined (Setext) headings (`====`, `----`) while most students know `#` headings. Works, but is not what markdown linters and most editors default to.
- **What I did about it:** Followed the template.
- **Suggested fix:** Consider `#`-style headings.

### Q16. Fork workflows / Pages not active after following the setup, so hand-in checks and deploy do not run
- **Where:** `kmom01.mdx` "En plats att koda på" (fork video + my new written steps), hand-in video
- **Type:** tooling
- **What I wondered / got stuck on:** After pushing `kmom01` and opening the PR, GitHub shows no workflow runs and no status checks for the PR. `gh api repos/efostud/webshop/actions/workflows` returns zero workflows and the Pages API returns 404, i.e. the fork's workflows were never enabled and Pages is not set to "GitHub Actions". The hand-in video promises lint + static deploy checks on the PR, so a student who skipped the "enable workflows" click would silently get no checks and no deploy and might not notice.
- **What I did about it:** Blocked on the UI steps (they cannot be done through the API the first time). Asked the teacher to do them from the written steps as a test of the new text.
- **Suggested fix:** Add a "verify" step at the end of the setup: "Du ska se tre workflows under Actions" and after the first PR: "Kontrollera att checkarna kör". Consider a check in the hand-in video/page for "no checks running? Then Actions is not enabled".

### Q17. `gh pr create` in a fork fails with SAML SSO error
- **Where:** hand-in (video `YD0tE7FW6i0`), tooling
- **Type:** tooling
- **What I wondered / got stuck on:** `gh pr create --repo efostud/webshop --base main --head kmom01` failed with "Resource protected by organization SAML enforcement ... (repository.parent)" because `gh` queries the parent repo in the `dbwebb-jsux` organisation and the token is not SSO-authorised. Students who use `gh` will meet this, students who use the web UI will not.
- **What I did about it:** Created the PR with `gh api repos/efostud/webshop/pulls -f head=kmom01 -f base=main ...` which only touches the fork. It targeted `efostud/webshop` as intended.
- **Suggested fix:** Mention in the hand-in text that the web UI is the supported way; optionally add a `gh` note.

### Q18. `npm install` modifies the tracked `package-lock.json`
- **Where:** starter repo `webshop`
- **Type:** tooling
- **What I wondered / got stuck on:** Running `npm install` on the untouched starter changed `package-lock.json` (6 lines removed) so `git status` shows a modified file the student did not touch. A student may be unsure whether to commit it.
- **What I did about it:** Reverted it (`git checkout package-lock.json`) and did not commit it.
- **Suggested fix:** Use `npm ci` in the instructions, or regenerate and commit the lockfile in the starter.

### Q19. Could not check the result visually
- **Where:** my own tooling, not the course
- **Type:** other
- **What I wondered / got stuck on:** I could not take a screenshot (headless Chromium hung), so I verified only lint (`npm test`, all green), that the dev server serves the files, and that the API returns what `main.js` expects. The visual result and Oswald loading from Google Fonts are unverified. The teacher should open the page.
- **What I did about it:** Nothing further.
- **Suggested fix:** n/a.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
|---|---|---|
| Läsa & titta | 6 h | not done (book), captions read only |
| En plats att koda på | 1 h | mostly done by the teacher already; fork was pre-made |
| Öva (Lager API + Förstasidan) | 6 h | about 30 min of work for an experienced coder, largely typing the page's snippets |
| Uppgifter (Typsnitt och färg + Webbshoppen del 1) | 6 h | about 45 min incl. analysis and styling |
| Reflektera | 1 h | 10 min |

Note: an experienced student is far below the stated hours; less experienced students probably fit them.

## Things that worked well
- Fork setup steps and `npm install` / `npm test` on the starter repo work out of the box (`npm test`: eslint, stylelint, htmlhint all pass on the untouched starter).
- The finished-example video (`Ku2kLxM3EBU`) gives a very clear picture of the expected outcome.
