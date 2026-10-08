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
- **What I did about it:** Used `attributeChangedCallback`-free `connectedCallback` rendering as in the exercise, but escaped the JSON with `&apos;`/`&quot;` via `escapeHTML`... (see code), and noted the misleading text.
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

