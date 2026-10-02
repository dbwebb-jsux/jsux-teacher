# kmom03 student run — questions and friction

Run date: 2026-10-02
Persona: capable student. Source: `kmom03.mdx`, `kunskap/en_varukorg_med_css_och_javascript.mdx`, `uppgifter/javascript_pa_webben.mdx`, `uppgifter/webbshoppen_del_3.mdx`.
Work is done in `webshop-efostud` on branch `kmom03`, branched from `kmom02` (the kmom01/kmom02 PRs are not merged; the kmom02 PR could not be created because of SAML enforcement, see kmom02 Q17).
Videos read via Swedish auto-captions (3 videos).

## Open questions

### Q1. Exercise assumes a `models/products.js` with `fetchProduct`, but never defines it
- **Where:** `kunskap/en_varukorg_med_css_och_javascript.mdx`, final `cart.js` listing
- **Type:** missing
- **What I wondered / got stuck on:** The listing imports `products from "../models/products.js"` and calls `products.fetchProduct(product)`, using `productInfo.data.image_url`. The text says "Filen `models/products.js` kan ni skapa själv". Nothing documents the single-product endpoint (`GET /v2/products/:id?api_key=...`, returns `{data: {...}}`) in `kunskap/lager_apit.mdx`. A student must guess it or discover it by trial.
- **What I did about it:** Tested `GET /v2/products/<id>` (works, needs `api_key`), wrote `models/products.js` with `fetchProduct` and `fetchAll`.
- **Suggested fix:** Show a 10-line `models/products.js` in the exercise (or document the endpoint in `lager_apit`).

### Q2. File layout is inconsistent again (`main.js` / `views/`)
- **Where:** same article, `import { instantiateCartButtons } from "./cart.js"` in `main.js` vs. `import products from "../models/products.js"` in `cart.js`
- **Type:** contradiction
- **What I wondered / got stuck on:** `../models/` from `cart.js` implies `cart.js` is in a subfolder (`views/`, as in kmom02), while `./cart.js` from `main.js` implies both are in the same folder. Whether `main.js` is in `views/` or in the root depends on kmom02 (see kmom02 Q2). Also the earlier text says to "lägga till [cart.css och cart.js] som de andra filerna i `head`" but `cart.js` is imported by `main.js`, so adding it as a second `<script>` is wrong (double-load, though harmless for modules).
- **What I did about it:** `main.js` stays in the root, `views/cart.js`, import `./views/cart.js`. Did not add `cart.js` to `<head>`.
- **Suggested fix:** Show the final folder tree. Remove "lägg till i head" for `cart.js`.

### Q3. Product markup in the exercise does not match the student's page
- **Where:** same, "Knappar för att lägga till i varukorgen" and "Visa varukorgen"
- **Type:** unclear
- **What I wondered / got stuck on:** The exercise replaces the product card with its own markup (`<p>` name, `.bottom` wrapper) and the header with `inner-header`, `right`, `h1 > a`, classes that have no CSS in the exercise. A student with their own kmom01/02 markup must merge by hand. It's not said that only the `span.add-to-cart` is essential.
- **What I did about it:** Kept my markup, added only the button, the cart icon and cart panel.
- **Suggested fix:** Say "lägg till knappen i din egen markup, resten är exempel".

### Q4. Unfinished sentence
- **Where:** same, "Själva varukorgshanteringen"
- **Type:** other
- **What I wondered / got stuck on:** "Vi avslutar med med att göra objektet till en sträng igen och sedan" ends mid-sentence (and has "med med").
- **What I did about it:** Read it as "... sparar det i LocalStorage".
- **Suggested fix:** Finish the sentence.

### Q5. `id="${product.id}"` with numeric ids, and behaviour of `addToCart`
- **Where:** same, `addToCart` and product markup
- **Type:** other
- **What I wondered / got stuck on:** Using the numeric product id as the DOM `id` works in HTML5 but is fragile (CSS selectors on it need escaping; ids like `50577` clash if the cart also renders the same ids). `data-product-id` is the conventional way. Also `addToCart` reads `event.target.id`; if the button contains an element (icon) the target is the child. And the final `addToCart` calls `toggleCart()`, which closes the cart if it is already open and does not refresh it (the first listing without that call differs from the last).
- **What I did about it:** Used `data-product-id` and `event.currentTarget`. Re-rendered the cart on change.
- **Suggested fix:** Use `data-` attributes in the exercise, and explain `currentTarget` vs `target`.

### Q6. Cart overlay positioned with `position: absolute` and `scrollY` hack
- **Where:** same, `cart.css` and `toggleCart`
- **Type:** other
- **What I wondered / got stuck on:** The panel is `position: absolute` and `top` is recalculated from `window.scrollY` in JS, plus `body.style.overflow = "hidden"`. `position: fixed; top: 1vh` achieves the same without JS. The page also teaches "use class toggling" while setting inline styles for `body` overflow. Student can wonder why not `fixed`. Also `width: 40vw` is too narrow on phones, and `100vh`-based height misbehaves with mobile browser toolbars (the kmom06/10 pages talk about responsiveness).
- **What I did about it:** Used `position: fixed`, `width: min(40rem, 100vw)`, and a `.cart-open` class on `body` instead of inline styles.
- **Suggested fix:** Either explain why `absolute`, or switch the exercise to `fixed`.

### Q7. Cart is only available on `index.html`; the spec says nothing about `category.html`
- **Where:** `uppgifter/webbshoppen_del_3.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** The category page from kmom02 has the three albums, but there is no requirement for cart there. Shared header code (theme selector, cart, counter) is duplicated in each HTML file, which leads to a copy-paste of the cart `<div>`s. The kmom03 demo video only shows the front page.
- **What I did about it:** Cart on the front page only.
- **Suggested fix:** State which pages must have the cart; consider a note that duplicated header markup motivates the web components in kmom05.

### Q8. Spec for counter and remove is thin
- **Where:** `uppgifter/webbshoppen_del_3.mdx`, req. 2-3
- **Type:** unclear
- **What I wondered / got stuck on:** "minska antal ... eller helt ta bort" — are two controls needed (− and remove)? What happens when quantity reaches 0 (remove automatically)? Does the counter show the sum of quantities or the number of distinct products ("totalt antal produkter")? Should the counter be visible when 0? Should it update on page load without opening the cart (needs code outside `toggleCart`)? Also nothing about an increase button in the cart (the video shows "öka" and "sänka" and "ta bort").
- **What I did about it:** +/−/remove buttons; quantity 0 removes the item; counter = sum of quantities, hidden when 0, updated on load.
- **Suggested fix:** Add one line each to the spec.

### Q9. Report asks for three page types that a student's own shop cannot be
- **Where:** `uppgifter/javascript_pa_webben.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** "Utifrån din valda webbshop" and "förstasidan, en produktsida och en ordersida/varukorgen". Ordersida: many shops need a logged-in cart or have no separate page (IKEA's `/shoppingcart/` is public, but it is not obvious). The table header "Ordersida" vs. text "ordersida/varukorgen". It is also not said how to treat lazy-loaded scripts (counts grow if you wait), or to wait for network idle, or whether to include inline scripts. Results fluctuate between runs (A/B tests, consent banners); the cookie banner may need accepting first.
- **What I did about it:** Measured with headless Chromium (a one-off node script over the DevTools protocol; a student would simply use the Network tab) through the DevTools protocol (cache disabled, waited 15 s, counted requests of type Script, transferred bytes via `encodedDataLength`, load time from `loadEventEnd`), no cookie consent. Documented in the report.
- **Suggested fix:** Specify: wait until the page has finished loading, same state (consent) for all pages, "Transferred" vs "Resources" size, and note that numbers vary.

### Q10. "Rapportfil" paths and wording
- **Where:** `uppgifter/javascript_pa_webben.mdx`, "Skapa rapportfilen"
- **Type:** unclear
- **What I wondered / got stuck on:** "Stå i kursrepot" again (kmom02 Q11). The `touch reports/kmom03.md` gives an empty file, whereas kmom02 copied the previous report. Fine, but the report asks to keep "Urval/Metod" from earlier, and the text says "Vi kommer igenom kursen ta ett analytiskt perspektiv" without saying that the same webshop (IKEA in my case) should be used as in kmom01-02; the demo video analyses the same shop.
- **What I did about it:** Used IKEA again.
- **Suggested fix:** "Använd samma webbshop som i kmom01 och kmom02."

### Q11. Stale information in the videos / page
- **Where:** `kmom03.mdx`, "Veckans genomgång" video (`TNbecMUHidw`), lecture (`6_oBAUzPRCU`)
- **Type:** outdated
- **What I wondered / got stuck on:** The walkthrough mentions the "tre veckors upprop" with dates 28 November and 12 December and says kmom04 ("Kman F"?) is already released, kmom05 guests (two alumni from IKEA), kmom06 AI chatbot. These are last year's dates and plans, and could confuse the November 2026 run. The lecture teaches testing/validation, custom events and Flexbox — custom events are not part of the kmom03 spec (not needed for the cart). The demo video analyses a different shop (lowfre) and its webshop has a quantity control and remove.
- **What I did about it:** Ignored, used the written spec.
- **Suggested fix:** Re-record or add a text warning ("videon är från 2025, datum gäller inte"), and decide whether the lecture on custom events is a recommended extra.

### Q12. Where do the lecture's `GitHub Actions` problems show up for the student?
- **Where:** lecture video (`6_oBAUzPRCU`)
- **Type:** other
- **What I wondered / got stuck on:** The lecture says some students' tests fail on Node 20 and 22 after `npm install` while locally passing. Related to my own run: `npm install` rewrites `package-lock.json` (removes 6 lines) on this machine, leaving the tree dirty (`git status` shows `M package-lock.json`) after a clean checkout. A student doing `git add -A` commits it.
- **What I did about it:** Reverted the lockfile with `git checkout package-lock.json`.
- **Suggested fix:** Check which npm version the lockfile was made with; consider `npm ci` in the instructions.

### Q13. PR creation blocked by SAML enforcement (again)
- **Where:** hand-in, `gh pr create --repo efostud/webshop`
- **Type:** tooling
- **What I did about it:** Branch `kmom03` is pushed; PR could not be created (same as kmom02 Q17). Blocked, needs the teacher to authorize the token for the `dbwebb-jsux` org or create the PRs in the web UI.
- **Suggested fix:** None for students.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
| --- | --- | --- |
| Läsa & titta | 6 h | Not done (book chapter 8, ITU reports); videos read as captions only |
| Öva | 6 h | Not timed; the cart exercise itself is about 1-2 h for a capable student once Q1-Q3 are worked around |
| Uppgifter | 6 h | Not timed; the webshop part (−/remove/counter) is small, the report needs manual measuring of three pages |
| Reflektera | 1 h | Simulated |

## Things that worked well
- Cart logic tested in headless Chromium: add (3 clicks → counter 3, `localStorage` `{id: qty}`), decrease, remove, increase, no console errors. `npm test` clean.
- The exercise builds on the kmom01/02 structure and the cart logic is small and easy to follow.
