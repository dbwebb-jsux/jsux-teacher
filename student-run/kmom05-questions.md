# kmom05 student run — questions and friction

Run date: 2026-10-08
Persona: capable student. Source: `kmom05.mdx`, `kunskap/google_lighthouse.mdx`, `kunskap/en_webbkomponent.mdx`, `uppgifter/analys_verktyg.mdx`, `uppgifter/webbshoppen_del_5.mdx`.
Work is done in `webshop-efostud` on branch `kmom05`, branched from `kmom04` (earlier kmom PRs are not merged).
Videos read via Swedish auto-captions (demo, genomgång, Lighthouse, lecture). The IKEA guest-talk video could not be fetched (see Q1).

## Open questions

### Q1. IKEA guest-talk video is private
- **Where:** `kmom05.mdx`, "Oscar och Niklas från IKEA berättar" (`-SbmENFroCk`)
- **Type:** broken-link
- **What I wondered / got stuck on:** `yt-dlp` reports "Private video". A student sees an unavailable embed (I only verified through yt-dlp, not in the browser). The kmom intro and the genomgång lean on this talk.
- **What I did about it:** Skipped it; nothing in the assignments depends on it.
- **Suggested fix:** Make it unlisted/public, or remove the section if the recording may not be shared.

### Q2. Component exercise assumes `views/main.js` and `../components/`; the starter has `main.js` in the root
- **Where:** `kunskap/en_webbkomponent.mdx` ("Produktlistningen", "Webbkomponentens klass")
- **Type:** contradiction
- **What I wondered / got stuck on:** The exercise says "min `views/main.js`" and imports `../components/single-product.js`, i.e. `main.js` is in `views/`. In the starter-derived repo `main.js` is in the root (same recurring layout problem as kmom02 Q2, kmom03 Q2, kmom04 Q3). From the root the import must be `./components/...`. The text also says "Ni bör ha något liknande ... kan ligga i andra filer", so it is partly covered, but the import paths are still hard-coded.
- **What I did about it:** Put components in `components/` and imported `./components/...` from root `main.js`.
- **Suggested fix:** Show the target folder tree, or move `main.js` into `views/` in the starter.

### Q3. Exercise markup and ids do not match the repo from kmom03/04
- **Where:** same, `renderProducts` / `single-product.js`
- **Type:** contradiction
- **What I wondered / got stuck on:** The exercise product markup uses `<span class="add-to-cart" id="${product.id}">+</span>`. My (kmom03) markup uses `<button class="add-to-cart" data-product-id=...>` and `cart.js` reads `dataset.productId` (see kmom03 Q5). The student must translate. The exercise's `instantiateCartButtons` ordering issue is not mentioned: it must run *after* the components have rendered their HTML.
- **What I did about it:** Kept my markup and `data-product-id`; call `instantiateCartButtons()` after the list is rendered.
- **Suggested fix:** Say that the markup is an example and the add-button wiring must still work; state the ordering.

### Q4. `observedAttributes` without `attributeChangedCallback`; `product='${JSON.stringify(...)}'` quoting
- **Where:** same, `SingleProduct` class and `renderProducts`
- **Type:** unclear
- **What I wondered / got stuck on:** `observedAttributes` is declared but no `attributeChangedCallback` exists, so it does nothing; the text says it "definierar vilka attribut vi kunna hämta data ifrån", which is not what it does (it declares which attributes trigger `attributeChangedCallback`). Also the JSON is put inside a single-quoted attribute: any apostrophe in a value (an album called "Ain't ...") would break the HTML. (None of the 12 current product names has one, so it works for now.) `JSON.parse` in a getter runs on every access.
- **What I did about it:** Kept `connectedCallback` rendering as in the exercise, but wrote the JSON into a double-quoted attribute escaped with a small `escapeAttribute()` helper (`components/escape.js`), and did not declare `observedAttributes`.
- **Suggested fix:** Explain what `observedAttributes` is for, or drop it; mention escaping or use `this.dataset`/`setAttribute` from JS.

### Q5. The exercise does the product *list*, the assignment asks for the *cart*; the exercise says nothing about click handling
- **Where:** `en_webbkomponent.mdx` vs. `uppgifter/webbshoppen_del_5.mdx` req. 1-3
- **Type:** unclear
- **What I wondered / got stuck on:** The exercise ends with a product card that renders (`+` button inert; wiring comes from kmom03 code). Req. 3 needs methods in the class for decrease/increase/remove, which the exercise never shows (no event listeners inside the component, no `this.querySelector`, no re-rendering). Req. 1 "produktlistning/orderraderna i varukorgen" is ambiguous: the cart *list* or the cart *lines*? The video says "cart item" component and that the click handling must move into the component. The prose "vi byter ut de enskilda produkterna i produktlistningen" in the exercise intro says the product list on the front page, the assignment says the cart. Whether the front page cards must be components as well is unclear (the exercise: yes; the assignment: only the cart).
- **What I did about it:** Did both: `single-product` (front page, from the exercise) and `cart-item` (cart, the assignment).
- **Suggested fix:** State clearly that the assignment is the cart, and give a small example of an event handler as a class method, plus how a component tells the cart to re-render (e.g. a custom event or an imported function). The video hints at importing the badge function from the cart view.

### Q6. Who updates the cart badge and the total after a component changes the cart?
- **Where:** `webbshoppen_del_5.mdx` req. 3; genomgång video ("importera indikatorn")
- **Type:** missing
- **What I wondered / got stuck on:** Once buttons live inside the component, the badge (`updateCartCount`) and the cart view must be refreshed from the component. `updateCartCount` is private in `views/cart.js`, and a component importing `cart.js` while `cart.js` imports the component is a circular import. The video says "ni behöver inte hantera allt i dubbla funktioner", i.e. import the shared function, but the spec does not mention it.
- **What I did about it:** Moved cart storage helpers into `models/cart.js` (get/save/change/remove, dispatching a `cart-changed` event), the badge and cart list listen to that event.
- **Suggested fix:** Hint at a shared `models/cart.js` or custom events as the way to avoid circular imports.

### Q7. Analysverktyg: "samma 3 webbshoppar som förra veckan", but the spec contradicts itself with the kmom04 spec and has leftover text
- **Where:** `uppgifter/analys_verktyg.mdx`
- **Type:** other
- **What I wondered / got stuck on:** The intro starts with "i kommer igenom kursen..." (typo, missing "Vi") and says "I detta kursmoment tar vi ett lite mer tekniskt perspektiv", copied from kmom04. Req. 1 says use the same three shops; kmom04 itself had the "2 more" vs "three" ambiguity (kmom04 Q15). The report template says "tre tabeller" and each has the same three pages as in kmom04, i.e. 9 Lighthouse runs. The spec says only "Desktop" but nothing about mobile; scores vary between runs and with extensions/login state, which the spec does not mention (use an incognito window?). "Ordersida" had to be an empty cart last week (kmom04 Q15).
- **What I did about it:** Same three shops and the same nine URLs as in kmom04, Desktop mode.
- **Suggested fix:** Fix the typo and the copied sentence; recommend incognito and a note that scores fluctuate; say whether the student's own shop is included (video says "gärna").

### Q8. Lighthouse video and exercise: Chrome menu path is outdated, no instructions for CLI/headless; "Best Practices" etc. not explained
- **Where:** `kunskap/google_lighthouse.mdx` (video only)
- **Type:** outdated
- **What I wondered / got stuck on:** The whole "exercise" is a 2-minute video: "tryck på de tre plupparna ... mod tools ... developer tools". Text instructions are missing, so the page has no way to copy settings (Desktop vs Mobile, categories) and nothing explains the four categories or what a "good" score is. Lighthouse Desktop mode is required by the spec but the video runs the default (mobile). No link to the official docs.
- **What I did about it:** Used the Lighthouse CLI (see below) with `--preset=desktop`, which matches DevTools Desktop mode.
- **Suggested fix:** Add 5 lines of text: how to open it, select Desktop, the four categories (with a link to docs), and incognito.

### Q9. Reading: chapter 7 "Lists of Things" is not accessible; MDN articles are fine
- **Where:** `kmom05.mdx` "Läsa & titta"
- **Type:** other
- **What I wondered / got stuck on:** The book is behind the library login. The reflection question 1 ("Cards-uppdelningen för listor") depends on the chapter; I cannot answer it from first-hand reading. The MDN pages are available.
- **What I did about it:** The reflection for question 1 is **simulated** and written from what I actually know and can see (the course demo and my own work), and says so by being generic.
- **Suggested fix:** None needed, but a one-line pointer to the chapter's "Cards" pattern would help students find it.

### Q10. Videos date themselves; "veckan efter Black Friday", dates in December
- **Where:** genomgång and lecture videos (captions)
- **Type:** outdated
- **What I wondered / got stuck on:** The genomgång talks about "veckan efter Black Week", the lecture about the project start, a trip to a conference and that "ungefär hälften av alla studenter försvann i trevägsuppropet". For the November 2026 run these details are wrong/confusing. The page text says "Måndag förmiddag", the lecture "Onsdag eftermiddag".
- **What I did about it:** Ignored.
- **Suggested fix:** Re-record or add a note that dates refer to the 2025 run.

### Q11. Exercise code is indented with 4 spaces and fails the starter's ESLint (`indent: 2`)
- **Where:** `kunskap/en_webbkomponent.mdx`, the class listings
- **Type:** tooling
- **What I wondered / got stuck on:** Pasting the `SingleProduct` listing gives 9 `indent` errors in ESLint (verified by linting the pasted code). The starter's `CLAUDE.md` says 2 spaces. The "Node.js CI" workflow would fail for any student who copies the code as is.
- **What I did about it:** Wrote 2-space code.
- **Suggested fix:** Reformat the listings to 2 spaces.

### Q12. `remove()` as a method name overrides `Element.remove()`
- **Where:** `uppgifter/webbshoppen_del_5.mdx` req. 3 ("minskning, ökning och borttagning ... i metoder i klassen")
- **Type:** unclear
- **What I wondered / got stuck on:** The natural names `increase`, `decrease`, `remove`: the last one shadows the built-in `Element.remove()`, which removes the element from the DOM. Harmless here but a trap.
- **What I did about it:** Named it `remove_()` (ugly).
- **Suggested fix:** Suggest `removeProduct()` in the spec/exercise.

### Q13. Clas Ohlson has no standalone cart page; last week's "order" row was a 404 page
- **Where:** `uppgifter/analys_verktyg.mdx` req. 3, and `uppgifter/resurser_pa_webben.mdx` (kmom04)
- **Type:** unclear
- **What I wondered / got stuck on:** Lighthouse got 404 (`ERRORED_DOCUMENT_REQUEST`) on `/se/cart`, `/se/checkout` and `/se/kassa` at clasohlson.com; the real cart is a drawer. My kmom04 measurement of `/se/cart` therefore measured an error page, and I only found out in kmom05. I corrected the kmom04 report (pushed to the open PR). Students will hit the same with any shop whose cart is a drawer or whose checkout needs items in the cart.
- **What I did about it:** Left that row empty and said so in the report.
- **Suggested fix:** Say "välj en webbshop som har en egen varukorgs-/kassasida, eller skriv att den saknas", and warn that the page must be checked to be a real page, not a 404.

### Q14. Lighthouse scores vary between runs; terminology in the genomgång is outdated
- **Where:** `uppgifter/analys_verktyg.mdx`, `kunskap/google_lighthouse.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** Performance is noisy (throttling, extensions, headless vs headed); students will compare numbers and get different results. Not mentioned. Lighthouse 13 reports "insights" (e.g. `image-delivery-insight`), not "Opportunities".
- **What I did about it:** Documented version, preset and one run per page.
- **Suggested fix:** Add a sentence about noise, recommend incognito, and update the vocabulary.

### Q15. Own webshop weighs about 28 MB: product images from the API are huge
- **Where:** the student's own shop + Lager API `image_url`
- **Type:** other
- **What I wondered / got stuck on:** Lighthouse on my own front page shows ~28 MB total, of which ~27.7 MB is estimated saveable via image delivery, LCP 8 s locally. The kmom04 sustainability report is about resource usage, yet the student's own shop is heavier than all measured commercial shops. `kunskap/responsiva-bilder.mdx` exists but kmom05 never mentions it. In the kmom04 report I had written "min webbshop är liten" without measuring; the numbers contradicted it (corrected). Good teaching material if the student measures their own shop.
- **What I did about it:** Reported it in the kmom05 report; did not change the shop (out of scope for del 5).
- **Suggested fix:** Make "kör Lighthouse på din egen webbshop" an explicit requirement in `analys_verktyg` and link `responsiva-bilder`; consider smaller images in the Lager API.

### Q16. Deploy workflow still fails in the fork (Pages not enabled)
- **Where:** `.github/workflows/static-deploy.yml`
- **Type:** tooling
- **What I wondered / got stuck on:** Same as kmom04 Q17. Lint (Node 20/22) and agent-policy pass on `kmom05`.
- **What I did about it:** Nothing.
- **Suggested fix:** See kmom04 Q17.

### Q17. PR via REST again
- **Where:** hand-in
- **Type:** tooling
- **What I wondered / got stuck on:** `gh pr create` fails with the SAML error (kmom02 Q17, kmom04 Q18), so I did not retry it.
- **What I did about it:** Created https://github.com/efostud/webshop/pull/4 via `gh api repos/efostud/webshop/pulls` with base `efostud/webshop:main`, head `kmom05`. Not merged. Because the branch is cut from `kmom04`, the PR diff also contains the unmerged kmom03/kmom04 commits.
- **Suggested fix:** See kmom04 Q18.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
|---|---|---|
| Läsa & titta | 8 h | not done (book and IKEA talk unavailable); ~0.3 h captions |
| Öva (Lighthouse + webbkomponent) | 6 h | ~0.5 h (video only) + ~1 h |
| Uppgift: Webbshoppen del 5 | 6 h (shared with report) | ~1 h incl. shared cart model and escaping |
| Uppgift: Analysverktyg | (part of 6 h) | ~1.5 h, most of it on the Clas Ohlson 404 and my own shop |
| Reflektera | 1 h | ~0.2 h (question 1 simulated) |

## Things that worked well
- The component idea is easy to grasp from the exercise; the finished product list rendered after small changes.
- The Lighthouse CLI with `--preset=desktop` gives a reproducible alternative to DevTools.
- Measuring the student's own shop turned out to be a useful surprise (Q15).
