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

### Q8. Heading capitalisation differs between the page and the template
- **Where:** `kmom10.mdx` ("Krav 4" heading text "**Krav 4**" vs. "**KRAV 1A**/**KRAV 3**"), `reflections/kmom10.md` (`## KRAV 4`)
- **Type:** contradiction
- **What I wondered / got stuck on:** The page asks for the headings `KRAV 1A`, `KRAV 1B`, `KRAV 2`, `KRAV 3` but writes "Krav 4" in the Krav 4 section. The grading is based on the text and "missing documentation = 0 points", so the exact heading matters for a strict reader. The template has `KRAV 4`.
- **What I did about it:** Used the template's headings and removed the three requirements I did not do.
- **Suggested fix:** Use `KRAV 4` on the page.

### Q9. Grading table: overlapping bands, and a student doing two requirements cannot reach more than D-C
- **Where:** `index.mdx`, "Slutbetyg" and "Projektarbete"
- **Type:** unclear
- **What I wondered / got stuck on:** FX is "54-" and F is "50-", so 50–54 fits both. With the baseline 30 points (kmom01-06 done as instructed) plus up to 10 for "mycket väl utfört" plus 2x15 for two requirements, the maximum is 70 = C; to reach A the student must do all four requirements. The genomgång says this ("30 + 30 = 60, bara D"), but the kmom page says "Välj minst 2 av 4 för att få godkänt" without saying what grade that means. The first table says 30 points for "Kursmomenten är utförda enligt instruktion", i.e. already a baseline, which makes the 55 limit for E reachable only with partial points on the two requirements.
- **What I did about it:** Nothing.
- **Suggested fix:** Add a sentence like "två krav ger som mest betyg C" and fix the overlap.

### Q10. Presentation video: cannot be recorded by me; requirements and hosting are open
- **Where:** `kmom10.mdx`, "Presentation"
- **Type:** other
- **What I wondered / got stuck on:** "Bifoga videon i din inlämning via ett PR på GitHub": GitHub PRs have a file-size limit for attachments (about 10 MB for video in comments, 100 MB in the repo, which bloats the repo) and no length is given. The text does not say whether to link to YouTube or commit the file, nor the maximum length, nor the language. Is the video graded (points)? It is not in the grading table.
- **What I did about it:** Skipped; the teacher/real student must do this step. The PR description says so.
- **Suggested fix:** Give a length (e.g. 5–8 min) and a hosting suggestion, and say whether it affects the grade.

### Q11. The genomgång video is dated and partly contradicts the page
- **Where:** genomgång video `Z_WDdrUBV-U`
- **Type:** outdated
- **What I wondered / got stuck on:** The video says "fyra krav, inte fem som jag försökte hålla upp" (a visual joke that is lost in captions), that the final grade starts at "över 55" with "30 från kursmomenten", and that the category files should be named `hardrock.html` etc.; it also says "se till att lägga tydliga länkar ... visa upp dem i presentationsvideon" which is not on the page. The genomgång lists Krav 3 as including "när vi lägger till en order" as an API failure.
- **What I did about it:** Followed the page.
- **Suggested fix:** Add the video-only hints to the page.

### Q12. The MDN example for Krav 3 does not catch HTTP errors, and nothing says how to test "no internet"
- **Where:** `kmom10.mdx` Krav 3, linked MDN "Using the Fetch API"
- **Type:** unclear
- **What I wondered / got stuck on:** `fetch` only rejects on network failure; a 404/500 resolves normally. The MDN page does cover `response.ok` further down, but the first example (the one the genomgång points to) does not. "Testa genom att inte ha internet uppkoppling efter sidan laddats" is not trivial for students: DevTools "Offline" in the Network tab (or `Network.emulateNetworkConditions`) is the practical way, and is not mentioned. I used the DevTools offline emulation.
- **What I did about it:** `request()` helper checking `response.ok`, status text and JSON parsing; tested offline, malformed JSON and the stock rule in headless Chromium.
- **Suggested fix:** Mention `response.ok` and DevTools "Offline".

### Q13. A student has no way to avoid duplicate orders when an error hits halfway through creating an order
- **Where:** Krav 3 + `orderformular_for_webbshoppen`: order + order items are created in several calls
- **Type:** unclear
- **What I wondered / got stuck on:** If `POST /orders` succeeds and the second `POST /order_items` fails, retrying creates a second order. The Lager API has no transaction, and the spec says "visa tydligt för användaren". The payment is already done at this point, so the error message must say so.
- **What I did about it:** Banner text states the payment is done but the order failed; the form stays filled. No resume logic (documented as a known limitation in the reflection).
- **Suggested fix:** Mention this as a real-world error case, or accept it explicitly.

### Q14. Responsive images with an external resizing proxy: dependency and privacy not covered by the course
- **Where:** Krav 4 / `responsiva-bilder.mdx`
- **Type:** other
- **What I wondered / got stuck on:** The only way to get small images was a third-party resizer (`wsrv.nl`): the page requires `srcset`, but the files come from the API. It cut my front page from ~28 MB to ~301 KiB in Lighthouse (desktop), mobile 450 KiB, LCP 8.0 s to 0.7 s (partly because of `loading="lazy"`). A student may not realise the sustainability/privacy/availability trade-off, and the course (kmom04 report) discusses sustainability. If the proxy goes down, all pictures disappear (no `src` fallback to the original).
- **What I did about it:** Used it, documented the trade-off in the reflection.
- **Suggested fix:** See Q5 (smaller images from the API, or an article section on this).

### Q15. Responsiveness problems found by testing: `vh` units and fixed overlays, `1fr` grid overflow, chat button covering content
- **Where:** earlier kmoms' CSS (kmom03 cart overlay, kmom06 chat) + kmom10 Krav 4
- **Type:** other
- **What I wondered / got stuck on:** With no horizontal overflow at 320-1280 px, the "obvious" check passes, but screenshots at 375 px showed real problems: the chat button covering the add-to-cart button, the cart panel under the chat button, and giant single-column cards. A student who only resizes the desktop window may miss these. While fixing it, `repeat(2, 1fr)` made the grid wider than the viewport (min-content of images), which needs `minmax(0, 1fr)`; also `width`/`height` attributes on `<img>` need `height: auto` in CSS or the aspect ratio breaks.
- **What I did about it:** Fixed these; documented in the reflection.
- **Suggested fix:** Suggest testing on a phone-size emulator in DevTools, with a checklist of common traps (overlays, `vh`, grid `minmax(0, ...)`, images).

### Q16. CI and PR
- **Where:** hand-in
- **Type:** tooling
- **What I wondered / got stuck on:** Same as before: Pages deploy fails in the fork (Pages not enabled), `gh pr create` hits SAML so I used REST. Lint (Node 20/22) and agent-policy pass.
- **What I did about it:** PR https://github.com/efostud/webshop/pull/6, base `efostud/webshop:main`, head `kmom10`. Not merged. The diff includes unmerged earlier kmom commits because the branches are stacked.
- **Suggested fix:** See kmom04.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
|---|---|---|
| Genomgång video | (none) | ~0.2 h (captions) |
| Krav 3 | (none stated; 15 p) | ~1.5 h |
| Krav 4 | (none stated; 15 p) | ~2 h (measuring and screenshots) |
| Krav 1, Krav 2 | (none stated) | not implemented, read only |
| Redovisning | (none) | ~0.5 h |
| Presentation video | (none) | not done (Q10) |

No time is stated for the project at all, in contrast to the other kmom pages (which give hours per section); a student has no idea how much work to expect. Suggest adding hours per requirement.

## Things that worked well
- The four-requirement structure with choice is clear and gives students freedom; the reflection template per requirement is easy to follow.
- Having built components in kmom05 made the stock rule (Krav 3) quick to add.
- Measuring before and after (overflow, Lighthouse) made Krav 4 concrete.
