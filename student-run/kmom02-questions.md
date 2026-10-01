# kmom02 student run — questions and friction

Run date: 2026-10-01
Persona: capable student. Source: `kmom02.mdx`, `kunskap/ett_fargschema_for_var_webbshop.mdx`, `kunskap/rendera_markdown_i_javascript.mdx`, `uppgifter/typsnitt_och_farg_del_2.mdx`, `uppgifter/webbshoppen_del_2.mdx`.
Work is done in `webshop-efostud` on branch `kmom02`, branched from `kmom01-run2` (see Q1).
API key reused from earlier runs. Videos: the three kmom02 videos were read via Swedish auto-captions.

## Open questions

### Q1. Which branch to start kmom02 from when the kmom01 PR is not merged?
- **Where:** `kmom02.mdx`, "En branch för kmom02" (`git checkout -b kmom02`, "stå i ditt repo")
- **Type:** unclear
- **What I wondered / got stuck on:** The page only says to create `kmom02`. The kmom01 PR is open (the teacher grades it, merge happens later), so `main` does not contain the kmom01 work (`index.html`, `main.js`, `reports/kmom01.md`), but both the exercise ("ta en kopia av vår förstasida") and the report spec (`cp reports/kmom01.md reports/kmom02.md`) need it. A student on `main` gets a repo without `index.html` content.
- **What I did about it:** Branched `kmom02` from the kmom01 branch. Then the kmom02 PR against `main` also contains all kmom01 commits.
- **Suggested fix:** Say explicitly: "Skapa kmom02 från din kmom01-branch (`git checkout kmom01 && git checkout -b kmom02`)" and explain that the PR will show kmom01 changes until kmom01 is merged, or that students should wait for/merge kmom01 first. Decide which one is intended.

### Q2. Exercise moves `main.js` to `views/` but nothing else is updated
- **Where:** `kunskap/ett_fargschema_for_var_webbshop.mdx`, "Tema-väljare"
- **Type:** contradiction
- **What I wondered / got stuck on:** The text says to create `views/` and put `main.js` "från förra veckans uppgift" in it, and `category.html` loads `views/main.js`. But (a) `index.html` still loads `main.js` from the root, so moving the file breaks the front page; (b) `main.js` imports `./models/auth.js`, which becomes wrong from `views/` (`../models/auth.js`); (c) `main.js` renders into `#product-list` and fetches all products, which is not what `category.html` needs. It is unclear if the file should be moved, copied or if a new file is meant.
- **What I did about it:** Kept `main.js` where it is for `index.html`, and created `views/category.js` for the category page, importing `../models/auth.js`. Did not move `main.js`.
- **Suggested fix:** Decide on a structure and write it out: e.g. "skapa `views/category.js`, `main.js` ligger kvar", or "flytta och uppdatera `index.html` och importvägen". Remove "lägger `main.js` från förra veckans uppgift i den katalogen".

### Q3. `localStorage` snippet uses a different id than the HTML
- **Where:** same, "Tema-väljare", second `mode-selector.js` listing
- **Type:** contradiction
- **What I wondered / got stuck on:** The HTML gets `id="variables-style"` and the first JS listing uses `getElementById("variables-style")`, but the second listing (the final one with `localStorage`) uses `getElementById("variables")`, which returns `null` and throws on first use.
- **What I did about it:** Used `variables-style`.
- **Suggested fix:** Change to `"variables-style"` in the second listing.

### Q4. Dark theme file duplicates all variables; page flashes light theme on load
- **Where:** same, "Tema-väljare"
- **Type:** other
- **What I wondered / got stuck on:** `variables-dark.css` is a full copy of `variables.css`, so any palette change must be made in two files. The mode is also applied by a `defer`red module script after first paint, so a user with dark saved sees a flash of the light theme on every page load. The course repeats the DRY argument for variables just before.
- **What I did about it:** Kept it as in the exercise (two files) but moved only the theme-specific variables (text/background) into the files that differ, with the palette shared in `variables.css`... (see commit). Noted the flash.
- **Suggested fix:** Mention that only the differing variables need to be in the dark file (a second `<link>` or `[data-theme]` selector), and optionally mention the flash and how to avoid it (small blocking inline script). At least name it as a known limitation.

### Q5. Invalid JavaScript in the markdown exercise
- **Where:** `kunskap/rendera_markdown_i_javascript.mdx`, second code block
- **Type:** broken-link / other (broken example)
- **What I wondered / got stuck on:** `let descriptionElement.innerHTML = ...` is a syntax error (`let` with a property access). Also the template wraps the result in `<p>...</p>` but `marked.parse` returns block-level HTML (`<h3>`, `<ul>`, own `<p>`), so the nesting is invalid and the browser/HTMLHint will complain.
- **What I did about it:** Wrote `descriptionElement.innerHTML = marked.parse(product.description)` without the `<p>`.
- **Suggested fix:** Fix the example; show the whole flow (fetch, loop, `marked.parse`), and remove the `<p>` wrapper.

### Q6. Unpinned CDN import and no word about security
- **Where:** same
- **Type:** unclear
- **What I wondered / got stuck on:** `https://cdn.jsdelivr.net/npm/marked/lib/marked.esm.js` has no version, so a future major release may change the path/API and break all students at once. The page also does not mention that `innerHTML` with markdown from an API is an XSS risk (marked does not sanitize) — relevant to a UX/web course with a sustainability and security angle.
- **What I did about it:** Used the URL as given (still returns 200 on 2026-10-01).
- **Suggested fix:** Pin a version (`marked@NN`) and add one sentence on trusting the source of the markdown (here: the teacher's own API) vs. sanitizing (DOMPurify).

### Q7. API category names differ from the spec
- **Where:** `uppgifter/webbshoppen_del_2.mdx`, "Kategorier i Lager-API:t"
- **Type:** contradiction
- **What I wondered / got stuck on:** The spec lists "Pop, Hard rock, Hip-Hop och Country", the example JSON shows `"category": "pop"`. The real values in the API are `pop`, `hard rock`, `country`, `hiphop` (the Hip-Hop one has no hyphen, hard rock has a space). A student filtering on `"hip-hop"` gets an empty page.
- **What I did about it:** Logged the actual values from `GET /v2/products`; chose `pop`.
- **Suggested fix:** List the exact `category` values in the spec.

### Q8. The spec does not say how to get the products of one category
- **Where:** `uppgifter/webbshoppen_del_2.mdx`, requirement 4
- **Type:** missing
- **What I wondered / got stuck on:** "de tre albummen som finns för din valda kategori" — the API has no documented category filter (and none is mentioned). A student must know about `Array.prototype.filter` on the `data` array, which is only implied. Not hard for the capable student but a beginner may fetch the three by id instead (hard-coded).
- **What I did about it:** Fetched all products and used `.filter(p => p.category === "pop")`.
- **Suggested fix:** Add a hint ("Hämta alla produkter och filtrera på `category`") and say whether hard-coding ids is accepted.

### Q9. Color scheme terminology not provided before it is required
- **Where:** `uppgifter/typsnitt_och_farg_del_2.mdx`, requirement 2 ("Notera även vilket sort färgschema som används"), `kmom02.mdx`
- **Type:** missing
- **What I wondered / got stuck on:** The exercise only uses the word "komplementärt". The kinds of schemes (monochromatic, analogous, complementary, triadic, split-complementary…) are not listed on the page, and the book chapter 5 is about visual style, not formal color theory (the page claims "färgteorin presenterat under veckan"). The only place it is presented is the Color Wheel tool and the walkthrough video. A student analysing IKEA must classify blue/yellow without a vocabulary.
- **What I did about it:** Used general knowledge (complementary = opposite on the wheel) and noted it in the report.
- **Suggested fix:** Add a short list of the standard schemes with one sentence each, or link a page, to the kmom page or the exercise.

### Q10. "Länka till webbshoppen" is ambiguous
- **Where:** `uppgifter/typsnitt_och_farg_del_2.mdx`, requirement 2.1
- **Type:** unclear
- **What I wondered / got stuck on:** Is it the analysed website (IKEA) or the student's own webshop? Since the report is a redo of kmom01 where "webbplatsen" is the analysed site, it is probably the analysed site, but the course also says "webbshoppen" for the project. In the walkthrough video the teacher analyses a low-fi/real site ("lowfree"?) and the analysis is of the analysed site, so I assume analysed site.
- **What I did about it:** Linked the analysed site (IKEA).
- **Suggested fix:** Write "Länka till webbplatsen du analyserar".

### Q11. `cp reports/kmom01.md reports/kmom02.md` says "Stå i kursrepot"
- **Where:** `uppgifter/typsnitt_och_farg_del_2.mdx`, "Skapa rapportfilen"
- **Type:** unclear
- **What I wondered / got stuck on:** "kursrepot" does not exist as a term elsewhere; the setup says "ditt repo". Minor.
- **What I did about it:** Ran it in my webshop repo.
- **Suggested fix:** "Stå i ditt repo".

### Q12. Requirement to find line-height and margins needs devtools but method is not explained
- **Where:** `uppgifter/typsnitt_och_farg_del_2.mdx`, requirement 2.3
- **Type:** missing
- **What I wondered / got stuck on:** "Notera ner radhöjd och marginal efter stycken och rubriker" – the kmom01 report method says that the student used some tool. Reading computed styles from the browser devtools is the obvious way, but it is not mentioned and computed values (px vs unitless line-height, `em` margins) can be reported in many ways.
- **What I did about it:** Used computed values from the stylesheets/devtools-equivalent (`curl`ed CSS), and reported unit as found.
- **Suggested fix:** Add one line: "Använd webbläsarens utvecklarverktyg (Inspect > Computed)".

### Q13. Stale text on the kmom page
- **Where:** `kmom02.mdx`, "Veckans genomgång" and "Veckans föreläsning"
- **Type:** outdated
- **What I wondered / got stuck on:** The recorded walkthrough is from a week when the teacher was ill (Kenneth and another teacher present it) and "Denna veckan utgick föreläsning pga sjukdom" is shown. The first video states "Emil har genomgång tisdag" but the video says Emil is absent. Also the walkthrough video's description of the repo (e.g. `main.js` in `views/`) may differ from the current exercise. In the "done" demo video the reference site is a different site ("lowfree") and the demo uses a gradient and a hero image, which is not in the spec.
- **What I did about it:** Ignored; followed the written spec.
- **Suggested fix:** Replace the sick-leave text before the November run; re-record or note which videos are from an earlier year.

### Q14. The "done" demo video and the spec do not match
- **Where:** `kmom02.mdx`, first video (`caw-OVXaaB8`)
- **Type:** unclear
- **What I wondered / got stuck on:** In the video the category page has a hero image with the genre, a gradient, a link at the top of the index page, and the theme selector also on the index page ("temaväljaren här uppe" on the front page). The spec only requires the selector on the category page (exercise puts it in `category.html`). The video also shows a monochromatic palette for the analysed site; since the captions only cover narration, I cannot verify the visuals.
- **What I did about it:** Followed the written spec and put the selector on both pages (small extra).
- **Suggested fix:** Decide whether the selector must be on every page; if so say it, since the persisted `localStorage` mode would otherwise be ignored on `index.html`.

### Q15. Time estimate
- **Where:** `kmom02.mdx`, all `<p class="time">`
- **Type:** too-much-work (to be filled at the end)
- **What I did about it:** See table at the end.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
| --- | --- | --- |
| Läsa & titta | 6 h | Not done (book chapters, Canvas video unavailable) |
| Öva | 6 h | (fill) |
| Uppgifter | 6 h | (fill) |
| Reflektera | 1 h | (fill) |

## Things that worked well
- The kmom page links to all pieces in a clear order and the 4 requirements of the assignment are short and testable.
- Reflection template with the three questions is already in the fork.
