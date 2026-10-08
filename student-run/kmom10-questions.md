# kmom10 student run — questions and friction

Run date: 2026-10-08
Persona: capable student. Source: `kmom10.mdx`, `kunskap/responsiva-bilder.mdx`, the grading section of `index.mdx`, the project genomgång video (`Z_WDdrUBV-U`, Swedish captions).
Work is done in `webshop-efostud` on branch `kmom10`, branched from `kmom06`.
Chosen requirements: **Krav 3 (felhantering)** and **Krav 4 (responsivitet)**, i.e. the minimum two. Krav 1 and 2 were read and analysed for clarity but not implemented. The presentation video cannot be recorded (see Q10).

## Open questions

### Q1. Krav 3 sentence is cut off
- **Where:** `kmom10.mdx`, end of "Krav 3: Felhantering": "Beskriv under rubriken **KRAV 3** i din redovisningstext hur du gick tillväga och hur "
- **Type:** other
- **What I wondered / got stuck on:** The sentence ends mid-way. The genomgång captions also end this section with "Här kompletteras men eh en liten text verkar som. Det har inte hunnit skriva klart", so the author knows. A student does not know what must be documented (how you tested? screenshots?).
- **What I did about it:** Documented approach, how I tested offline/malformed data, and the stock rule.
- **Suggested fix:** Finish the sentence (e.g. "... hur du testade felen").

### Q2. Krav 3 "alla API-anrop ... OpenAI-API:t": what about Stripe and the Stripe iframe? And what is "felformaterat data"?
- **Where:** `kmom10.mdx`, Krav 3 bullet 1
- **Type:** unclear
- **What I wondered / got stuck on:** "Alla API-anrop till Lager-API:t och OpenAI-API:t" omits the two Stripe calls (`create-checkout-session`, `session-status`), which are also `fetch` to the Lager API (so they count under "Lager-API:t"), but they sit in the exercise code and are easy to forget. "Skicka felformaterat data" is not explained: malformed request body? malformed response? The genomgång says "alla fetch-anrop i hela systemet ska ha try/catch". It is also not said what "tydligt" means (alert? inline banner?) or whether a retry is expected. The linked MDN page's first example only shows `try { fetch } catch`, which does *not* catch HTTP errors (404/500 resolve normally) — so a student who follows only the example never handles non-2xx responses.
- **What I did about it:** Central `request()` helper that handles offline (TypeError), non-2xx, and invalid JSON, with call sites catching and showing an inline `role="alert"` banner with a retry button.
- **Suggested fix:** List all call sites by name, say that `response.ok` must be checked, and give a concrete example of "felformaterat".

### Q3. Krav 3 stock rule: where must it be enforced, and what about stock changing after the item was added?
- **Where:** `kmom10.mdx`, Krav 3 bullet 2
- **Type:** unclear
- **What I wondered / got stuck on:** "antal produkter får inte överstiga lagersaldot" — in the cart (+ button), on the front page add-button, both? The genomgång says both and suggests disabling the plus button. The order page is not mentioned: stock can change between adding to cart and ordering, and the cart is stored in LocalStorage and can have been edited. Also `stock` is only returned by the products endpoint, so the add-to-cart code has to look it up (the cart only stores ids and counts). The same page that a student built in kmom03-05 (components) needs `stock` passed along, which the kmom05 `single-product` listing explicitly passes only `id, name, image_url`.
- **What I did about it:** Enforced it in front page buttons, in the cart `+` button, and re-checked when the order page loads.
- **Suggested fix:** State the pages explicitly and mention re-validation.

### Q4. Krav 4 is two sentences; what is "bra" and which widths/pages?
- **Where:** `kmom10.mdx`, Krav 4
- **Type:** unclear
- **What I wondered / got stuck on:** No target widths, no list of pages (does the cart panel, chat, order page and Stripe form count?), and nothing on how it will be assessed ("fungerar felfritt" in the grading table). The text says "kunskaper från webtec-kursen", which students outside that programme may not have. The genomgång recommends Flexbox and says the course so far "fokuserat på desktop".
- **What I did about it:** Tested at 320, 375, 768, 1280 px for horizontal overflow on all four pages and screenshots.
- **Suggested fix:** Give the minimum widths to support and the pages that must work.

### Q5. `responsiva-bilder` does not fit the situation: the shop only has one huge image per product
- **Where:** `kunskap/responsiva-bilder.mdx`, Krav 4
- **Type:** unclear
- **What I wondered / got stuck on:** The article shows `srcset`/`<picture>` with several files (`sheep@2x.jpg`), but the shop's product images come from the Lager API (`image_url`, one large PNG each, see kmom05 Q15 on 28 MB), so a student cannot supply smaller versions. The article does not mention `sizes`, `w` descriptors, `loading="lazy"`, `width`/`height` (layout shift) or image-resizing proxies.
- **What I did about it:** See the log below.
- **Suggested fix:** Add a section on what to do when you don't control the images, or add small image variants to the API (e.g. `thumbnail_url`).

### Q6. Krav 1B: category names, file names and which of the 4 categories
- **Where:** `kmom10.mdx` Krav 1B vs. genomgång
- **Type:** unclear
- **What I wondered / got stuck on:** The text says "de tre kategorier ... som du inte valde". The categories are not listed in the text (API: `pop`, `hard rock`, `country`, `hiphop`, 3 albums each). The video suggests file names `hardrock.html`, `country.html`, `hiphop.html` and says the category must be linked clearly and shown in the video; the text does not. A student choosing e.g. `hard rock` has a space in the category value. "Till dels layout" is vague. The "Tips från coachen" refers to kmom02 variables.
- **What I did about it:** Not implemented (read only).
- **Suggested fix:** List the four categories and the link requirement in the text.

### Q7. Krav 2: three sites x three pages x three analyses: heavy for a 15-point requirement, and the template is not specified
- **Where:** `kmom10.mdx` Krav 2
- **Type:** too-much-work
- **What I wondered / got stuck on:** "Samma mall" of which kmom (02, 04, 05 have different templates)? 9 pages x (colour+typography, resources, Lighthouse) is 27 measurements. In kmom04/05 these were separate reports; here one report, 15 points, no length indication. "Färg och typsnitt (kmom02)" — what the kmom02 analysis looked like is not repeated. Also from the experience of kmom04/05: not every shop has a standalone cart page (kmom05 Q13).
- **What I did about it:** Not implemented (read only).
- **Suggested fix:** Link to the three earlier specs and give an expected size.

### Q8. Reflection/grading: "KRAV 1A" heading and the rule "0 points if not documented" are strict; template headings differ from the page
- **Where:** `kmom10.mdx` ("Redovisning"), `reflections/kmom10.md`
- **Type:** contradiction
- **What I wondered / got stuck on:** The page asks for the heading `KRAV 3`, `Krav 4` (inconsistent capitalisation: "KRAV 1A", "KRAV 2", "KRAV 3", but "Krav 4" at Krav 4), the template uses `## KRAV 4`. The template says "Ta bort rubriker för krav du inte har gjort", the page says "ha en rubrik för varje krav du gör" - consistent. The page says reflections "tre delar" in the text but the template has five question areas (1A, 1B, 2, 3, 4) + general + course thoughts. Fine, just the capitalisation.
- **What I did about it:** Used the template's headings, removed 1A, 1B and 2.
- **Suggested fix:** Use one capitalisation.

### Q9. Grading table: overlapping bands and unclear mapping for projects with 2 requirements
- **Where:** `index.mdx`, "Slutbetyg" and "Projektarbete"
- **Type:** unclear
- **What I wondered / got stuck on:** E is "55+", FX "54-", F "50-": FX and F overlap (50–54). The genomgång explains: 30 (kmom01-06) + 30 (two krav) = 60 = D at best, so a student who does two requirements and gets everything right gets D, not E as the page text may suggest ("minst 2 krav för godkänt" suggests the minimum is E). The text says "Välj minst 2 av 4 krav för att få godkänt", but a student with 30 + 2x(less than 12.5) can end on FX. The Ladok table says kmom05–kmom10 is one 2.5 hp "Projekt" with "den sista inlämningen bestämmer slutbetyget".
- **What I did about it:** Nothing.
- **Suggested fix:** Show a worked example (two krav -> max 90? no: 30+10+30 = 70 = C) and fix the band overlap.

### Q10. Presentation video: cannot be recorded by me; requirements and hosting are open
- **Where:** `kmom10.mdx`, "Presentation"
- **Type:** other
- **What I wondered / got stuck on:** "Bifoga videon i din inlämning via ett PR på GitHub": GitHub PRs have a file-size limit for attachments (about 10 MB for video in comments, 100 MB in the repo, which bloats the repo) and no length is given. The text does not say whether to link to YouTube or commit the file, nor the maximum length, nor the language. Is the video graded (points)? It is not in the grading table.
- **What I did about it:** Skipped; the teacher/real student must do this step. The PR description says so.
- **Suggested fix:** Give a length (e.g. 5–8 min) and a hosting suggestion, and say whether it affects the grade.

### Q11. The genomgång video is dated and partly contradicts the page
- **Where:** genomgång video `Z_WDdrUBV-U`
- **Type:** outdated
- **What I wondered / got stuck on:** The video says "fyra krav, inte fem som jag försökte hålla upp" (visual joke, lost in captions), that the final grade starts at "över 55" with "30 från kursmomenten", and that the category files should be named `hardrock.html` etc.; it also says "se till att lägga tydliga länkar ... visa upp dem i presentationsvideon" which is not on the page. The genomgång lists Krav 3 as including "när vi lägger till en order" as an API failure.
- **What I did about it:** Followed the page.
- **Suggested fix:** Add the video-only hints to the page.

