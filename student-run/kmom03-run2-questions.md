# kmom03 student run (run 2) — questions and friction

Run date: 2026-10-21
Persona: capable student. Source: `kmom03.mdx`, `kunskap/en_varukorg_med_css_och_javascript.mdx`, `uppgifter/javascript_pa_webben.mdx`, `uppgifter/webbshoppen_del_3.mdx`.
Work is done in `efostud/` on branch `kmom03-run2`, branched from `kmom02-run2` (PRs for earlier kmom are not merged, see kmom02-run2 Q3).
Compared with run 1: the exercise now includes `models/products.js` (old Q1) and says `cart.js` is not added to `head`. Not repeated. Videos were **not** read (no `yt-dlp`); the demo video `D6u1N5gyGWA` is unverified.

## Open questions

### Q1. `cart.js` location is implied but not stated (was run 1 Q2, partly fixed)
- **Where:** `kunskap/en_varukorg_med_css_och_javascript.mdx`
- **Type:** contradiction
- **What I wondered / got stuck on:** `main.js` imports `./cart.js` (same folder) while `cart.js` imports `../models/products.js` (one folder down, `views/`). With `main.js` in the root (kmom02) both cannot hold; one path must change.
- **What I did about it:** `cart.js` in `views/`, `main.js` imports `./views/cart.js`.
- **Suggested fix:** State the folder for each file, or make the imports consistent.

### Q2. `cart.js` breaks any page without the cart markup
- **Where:** same, `cart.js` top-level `getElementById("cart")` + `addEventListener`
- **Type:** missing
- **What I wondered / got stuck on:** `cart` is `null` on pages without the cart HTML, so the module throws. The exercise only shows the cart in `index.html`, while the theme selector (kmom02) and the category page are separate. Where the cart should live (every page? header on all pages?) is not said, and the spec's counter in the header implies it should be on every page.
- **What I did about it:** Cart only on `index.html`.
- **Suggested fix:** Say whether the cart is on all pages, and give a hint (shared header markup, or guard with `if (cart)`).

### Q3. Exercise's markup snippet assumes a different header than the student's
- **Where:** "Visa varukorgen" `index.html` listing (`inner-header`, `.right`, `.mode`)
- **Type:** unclear
- **What I wondered / got stuck on:** The snippet shows the teacher's own header and says to copy; my header is different. Fine, but classes like `.cart-header` and `.close-cart` have no CSS in the exercise, so the heading and "x" are unstyled (I added flex).
- **What I did about it:** Adapted by hand.
- **Suggested fix:** Note that only the ids matter, or add the missing CSS.

### Q4. Cart rendering fetches products one by one
- **Where:** `renderCartProducts` in the exercise (`await` in a `for` loop, one request per product)
- **Type:** other
- **What I wondered / got stuck on:** Sequential requests make the cart slow with many items, and the spec asks for quantity buttons that re-render on each click.
- **What I did about it:** Used `Promise.all`, and event delegation on `#cart-products`.
- **Suggested fix:** Optionally mention `Promise.all`; it also matters for the reflection about structure.

### Q5. Spec requirements 2 and 3 give no hints; the order of work is unclear
- **Where:** `webbshoppen_del_3.mdx`
- **Type:** missing
- **What I wondered / got stuck on:** Req. 2 (decrease/remove) and 3 (counter) are one line each. The counter must be updated from `addToCart`, from the remove actions and on page load; the exercise does not show a pattern (a single `saveCart` helper). Whether the counter shows number of lines or the total quantity is stated ("totalt antal produkter") but could be misread.
- **What I did about it:** Total quantity; `saveCart` updates the counter.
- **Suggested fix:** Add a sentence on where the counter must be kept up to date.

### Q6. Analysis assignment needs three pages and an order page that the webshop does not have
- **Where:** `uppgifter/javascript_pa_webben.mdx`, req. 2 ("förstasidan, en produktsida och en ordersida/varukorgen") and the table
- **Type:** contradiction
- **What I wondered / got stuck on:** The text says "Använd samma webbshop som i kmom01 och kmom02" and "Utifrån din valda webbshop", which can mean either the analysed IKEA shop or the student's own webshop. The student's own shop has only two pages at this point (no product page, no order page; the cart is a panel). The table has a row "Ordersida".
- **What I did about it:** Analysed my own webshop; two rows measured, order page marked as missing.
- **Suggested fix:** Decide which webshop is meant. If it is the real shop (IKEA), say "den webbshop du analyserade i kmom01-02". If it is the student's, list which pages count.

### Q7. Network-tab measurements cannot be done by an agent
- **Where:** same, req. 3-6
- **Type:** tooling
- **What I did about it:** Measured file sizes with `wc -c`/`curl`; load time not measured; stated in the report's method.

### Q8. Interaction not clicked through
- **Type:** tooling
- **What I did about it:** `npm test` passes and the front page renders (headless screenshot) without visible errors, but add, decrease, remove and the counter were not tested in a browser. Verify manually before trusting.

### Q9. Reading assignment, sustainability reports not done
- **Where:** `kmom03.mdx`, "Läsa & titta"
- **Type:** other
- **What I did about it:** Skipped chapter 8 and the ITU reports. Reflection Q1 and the TIL are marked as simulated.

### Q10. PR not created
- **Type:** tooling
- **What I did about it:** `gh` is not installed; branch pushed. Compare URL: `https://github.com/efostud/webshop/pull/new/kmom03-run2`.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
| --- | --- | --- |
| Läsa & titta | 6 h | Not done |
| Öva | 6 h | ~30 min (adapting the code to my file layout) |
| Uppgifter | 6 h | ~30 min; the report would take more time in a real browser |
| Reflektera | 1 h | Simulated |

## Things that worked well
- The exercise is now complete: `models/products.js` is included and the explanation of `currentTarget` and `body.cart-open` was helpful.
- Spec for the cart is short and the three requirements are testable.
- The CSS for the cart (`position: fixed`, `min(40rem, 100vw)`) worked as written.
