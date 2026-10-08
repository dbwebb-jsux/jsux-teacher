# kmom04 student run — questions and friction

Run date: 2026-10-08
Persona: capable student. Source: `kmom04.mdx`, `kunskap/integrera_stripe.mdx`, `kunskap/orderformular_for_webbshoppen.mdx`, `uppgifter/resurser_pa_webben.mdx`, `uppgifter/webbshoppen_del_4.mdx`.
Work is done in `webshop-efostud` on branch `kmom04`, branched from `kmom03` (earlier kmom PRs are not merged).
Videos read via Swedish auto-captions (3 videos: demo, genomgång, föreläsning).

## Open questions

### Q1. Exercise code uses `productData.data.name`, but the student's `fetchProduct` (from kmom03) already unwraps `data`
- **Where:** `kunskap/integrera_stripe.mdx`, `buildItems`
- **Type:** contradiction
- **What I wondered / got stuck on:** The listing reads `productData.data.name`, i.e. `fetchProduct` returns the whole response body. In kmom03 the exercise (see kmom03 Q1) never defined `models/products.js`, so what `fetchProduct` returns depends on what each student wrote. In my repo it returns `result.data`, so `.data.name` would be `undefined`. The listing also uses `products` without importing it.
- **What I did about it:** Used `product.name` and imported `products`.
- **Suggested fix:** Provide `models/products.js` in kmom03 and use the same shape in both exercises; add the missing `import products` line.

### Q2. `unit_amount: 10000` is hardcoded; the product price is ignored
- **Where:** `kunskap/integrera_stripe.mdx`, `buildItems`
- **Type:** unclear
- **What I wondered / got stuck on:** Every product costs 100 kr in the exercise (the demo video says "200 spänn" for two albums). The API products have a `price` field (the first product has `price: 1`). Stripe takes the amount in öre. A student will wonder if `price` should be used, and whether `1` means 1 kr or 1 öre (Stripe minimum is about 3 kr for SEK, so `1 * 100` would likely fail).
- **What I did about it:** Kept 10000 as in the exercise (a flat 100 kr), noted it in code.
- **Suggested fix:** State that the price is fixed on purpose, or explain `price` and the ×100 conversion.

### Q3. Paths and file layout in `order.html` do not match the student's repo
- **Where:** `kunskap/integrera_stripe.mdx`, `order.html` listing
- **Type:** contradiction
- **What I wondered / got stuck on:** The listing uses `style/variables.css`, `style/cart.css`, `style/forms.css`, `style/style.css` and `views/mode-selector.js`, `views/cart.js`. The starter repo has all CSS in the root (`variables.css`, `style.css`, `cart.css`) and only JS in `views/`. Same layout problem as kmom02 Q2 / kmom03 Q2. Also "copy `index.html`" copies the product list container and `main.js` script; the text says `main.js` was removed but not the `product-list` div.
- **What I did about it:** Adapted paths to my own repo, put `forms.css` in the root.
- **Suggested fix:** Show the folder tree, or tell students to adapt paths to their own structure.

### Q3b. `forms.css` is mentioned but never provided
- **Where:** `kunskap/integrera_stripe.mdx` ("en `forms.css` för att göra stilen för formulären") and `orderformular_for_webbshoppen.mdx` ("Jag har i CSS-filen valt att dölja formuläret ...")
- **Type:** missing
- **What I wondered / got stuck on:** The CSS to hide the form until `session.status == 'complete'` is described but not shown. Also `.input`, `.button` (used on the "Beställ" link) have no CSS in the exercises; `.button` was never defined in kmom03.
- **What I did about it:** Wrote `forms.css` with `.input`, `.input:valid/:invalid`, `.order-form { display: none }` and `.order-form.visible`.
- **Suggested fix:** Show the 3-4 lines that hide/show the form, and a `.button` rule.

### Q4. Top-level `await` and module script not explained
- **Where:** `kunskap/integrera_stripe.mdx`, last listing in "Ta hand om betalningen"
- **Type:** unclear
- **What I wondered / got stuck on:** `await fetch(...)` is used at top level, which only works because `order.js` is loaded with `type="module"`. The text does not say so. Also the earlier listing already calls `initializeCheckout()` at the bottom; the last listing is meant to replace that, but it is shown as a loose snippet and the reader must work out how to merge them. ESLint config may also complain about the `Stripe` global (the `/* global Stripe */` comment handles it, good).
- **What I did about it:** Merged by hand into one `order.js`.
- **Suggested fix:** Show the full final `order.js` at the end of each exercise.

### Q5. `return_url: location.href` loops back to the same page and the flow after payment is unclear
- **Where:** `kunskap/integrera_stripe.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** After payment Stripe sends the browser to `return_url` with `?session_id=...`, and the page code then must show the order form. If the student pays, then reloads, they get the form again with the same session (status still `complete`), and can submit the order twice. Also if the user opens `order.html` with an empty cart, `items` is `[]` and Stripe returns an error that the exercise does not handle (the checkout silently fails to mount). The cart should be cleared only when the order is created (spec 5), so a paid-but-not-ordered state exists.
- **What I did about it:** Show a message when the cart is empty. Did not guard against double submit beyond clearing the cart after the order is posted.
- **Suggested fix:** Mention these edge cases or accept them explicitly ("vi hanterar inte ...").

### Q6. `session` data: where do `payment_id` and `customer_email` live, and "sparat instans"?
- **Where:** `kunskap/orderformular_for_webbshoppen.mdx`, `orderData` listing (`payment_id: // hämta från sparat instans`)
- **Type:** unclear
- **What I wondered / got stuck on:** "hämta från sparat instans" is not understandable. The reader must connect `session.payment_id` and `session.customer_email` from the other exercise, and keep `session` in a module-level variable (it is declared inside an `if` block with `const`, so not in scope in `handleSubmit`). The listing also has a missing comma after `payment_id:`, so it is not valid JS.
- **What I did about it:** Stored `session` in a module-level `let`, used `session.payment_id`.
- **Suggested fix:** Write the concrete code (`session.payment_id`) and where to keep `session`.

### Q7. The e-mail field is `disabled`, so `FormData` does not include it
- **Where:** `kunskap/orderformular_for_webbshoppen.mdx`, `<input ... disabled="disabled" name="email">` plus `formData.get("email")`
- **Type:** broken
- **What I wondered / got stuck on:** Disabled controls are excluded from `FormData`, so `formData.get("email")` returns `null`. The exercise would send `email: null` to the API, and spec req. 4.2 (e-mail from Stripe) would silently fail. Nothing in the exercise fills the field with `session.customer_email` either. Also `required` on a disabled field has no effect.
- **What I did about it:** Used `readonly` and filled the value from `session.customer_email`. (Alternatively read the value from `session` directly.)
- **Suggested fix:** Use `readonly` or read from `session`; show the line that sets `email.value`. This is a real bug for every student following the text.

### Q8. Form has only name and e-mail, but the demo video fills in address, zip, city, country
- **Where:** `orderformular_for_webbshoppen.mdx` (form has `firstname` + `email`), demo video (fills "379 79 Karlskrona, Sverige"), `uppgifter/webbshoppen_del_4.mdx` req. 4.1 ("information om kunden från ditt formulär")
- **Type:** contradiction
- **What I wondered / got stuck on:** The API order has `name, address, zip, city, country`. The video form has those; the exercise has only first name. The spec does not say which fields are required. Req. 3 says "passande typ" but with two fields there is almost nothing to choose from (only `text` and `email`), so the `tel`, `number` etc. types from the article are never practiced in the assignment. Pattern for zip code, `autocomplete` attributes, `<label>` accessibility are not required either, though the kmom reading is all about form usability.
- **What I did about it:** Made a form with name, address, zip (`inputmode="numeric"`, `pattern`), city, country, phone (`tel`) and e-mail (readonly).
- **Suggested fix:** List the required fields in the spec (e.g. at least name, address, zip, city, country) so the input-type requirement is testable.

### Q9. Which input types does the exercise cover vs. the spec? `number` for zip code is a trap
- **Where:** `orderformular_for_webbshoppen.mdx` (input types), `webbshoppen_del_4.mdx` req. 3
- **Type:** unclear
- **What I wondered / got stuck on:** The article says use `number` for digits. A Swedish zip code ("379 79") is not a number (leading zeros, spaces). The article's `pattern` example uses a personnummer. The Tidwell/NN reading says to avoid such traps. Also `type="date"`/`password` are listed but irrelevant for the order form.
- **What I did about it:** `type="text" inputmode="numeric" pattern="[0-9]{3} ?[0-9]{2}"` for the zip code.
- **Suggested fix:** Add a short note about when *not* to use `number`, and tie it to the NN article.

### Q11. Exercise says the order is created "i API:t" but `order_items` / `models/orders.js` is not provided
- **Where:** `orderformular_for_webbshoppen.mdx`, last two listings
- **Type:** missing
- **What I wondered / got stuck on:** `orders.addOrderItem` is used with the comment "jag valde att skapa en models/orders.js". Only the `POST /v2/order_items` call is documented in the Lager API docs (`order_id, product_id, amount, api_key`); the create-order response shape (`result.data.id`) is shown in the API docs but the exercise says "där det finns ett `id`" without `data.`. `newOrderId` is never defined. The order of API calls is OK but the student has to write `createOrder` and `addOrderItem` themselves.
- **What I did about it:** Wrote `models/orders.js` with `createOrder` and `addOrderItem`; verified that the response has `data.id`.
- **Suggested fix:** Show `models/orders.js` (about 20 lines), same as suggested for `products.js`.

### Q12. Spec req. 5: "indikatorn" and clearing the cart
- **Where:** `uppgifter/webbshoppen_del_4.mdx` req. 5
- **Type:** unclear
- **What I wondered / got stuck on:** Which indicator? Presumably the counter on the cart icon from kmom03. `updateCartCount` is not exported from `cart.js` in my version (nor in the exercise), so after `localStorage.removeItem("cart")` the badge stays until reload. The exercise never says how to update it.
- **What I did about it:** Exported `clearCart()` from `cart.js` that clears storage and updates the badge.
- **Suggested fix:** Mention "räknaren från del 3" and hint that cart.js needs to expose a function.

### Q13. Spec "Gör det möjligt att gå från varukorgen till en order-sida" vs. article text
- **Where:** `webbshoppen_del_4.mdx` req. 1, `integrera_stripe.mdx`
- **Type:** other
- **What I wondered / got stuck on:** The exercise puts `<a class="button" href="order.html">Beställ</a>` in the cart; the cart is generated in JS (`renderCartProducts`) in kmom03, and the empty-cart case also should not show the button. Not said where to add it.
- **What I did about it:** Added the link in the static cart panel markup below the product list, hidden if the cart is empty.
- **Suggested fix:** Say where the button goes.

### Q14. Stripe checkout works, but errors are not handled (bad `session_id`, 504)
- **Where:** `kunskap/integrera_stripe.mdx`, "Ta hand om betalningen"
- **Type:** missing
- **What I wondered / got stuck on:** `https://js.stripe.com/clover/stripe.js` and the embedded checkout mount fine in Chromium (iframe appears). But calling `/stripe/session-status` with an unknown `session_id` made the Lager API answer with an nginx `504 Gateway Time-out` HTML page after ~60 s, so `response.json()` in the exercise code throws and the page shows nothing. A student who edits or reuses an old URL gets a blank page and no hint. I could not complete a real card payment in headless Chromium (cross-origin iframe), so the post-payment path (`session.status == 'complete'` -> form) was only verified in pieces: the order API calls separately (order 16923 + item created and read back), the rest by reading the code.
- **What I did about it:** Left as in the exercise. Logged only.
- **Suggested fix:** Mention that `session_id` must come from a real payment, and wrap the call in `try/catch` with a message. Consider making the API return JSON errors.

### Q14b. Exercise's `:valid`/`:invalid` styling contradicts the NN/g article in the same kmom
- **Where:** `orderformular_for_webbshoppen.mdx` ("Styling av formulär", green/red border) vs. NN/g "Website Forms Usability" (errors not by color alone; avoid placeholders; explain formats; distinguish optional/required) and the `pattern` example with `placeholder="YYMMDD-XXXX"`
- **Type:** contradiction
- **What I wondered / got stuck on:** The exercise teaches green/red borders only, with `:invalid` applying on page load to every empty `required` field (the whole form is red before the student types), and uses a placeholder as the format hint. The assigned reading says the opposite on three points. `:user-invalid` (the fix) is not mentioned. The reflection question 1 asks what you learned from the article, so a student who follows the exercise literally violates it.
- **What I did about it:** Followed the article: hint text above the field, `:user-invalid` with thick border and background (not color only), no placeholder.
- **Suggested fix:** Update the exercise to `:user-invalid`, add a non-color cue and a visible hint, and remove the placeholder example.

### Q15. Time-consuming: Resurser på webben needs 2 new webshops x 3 pages, plus the first from kmom03
- **Where:** `uppgifter/resurser_pa_webben.mdx` req. 1 ("ytterligare 2 webbshoppar") vs. genomgång video ("välja tre webbshoppar") and the three tables
- **Type:** contradiction
- **What I wondered / got stuck on:** The spec says "choose 2 more webshops" (so 3 in total with the one from kmom03?), the genomgång video says "välja tre webbshoppar och tre sidor från varje", "rimligen kan sida 1 vara den ni redan har undersökt i kmom03". The kmom03 assignment analysed one shop's JavaScript only. The student's own webshop is not part of it. Req. 6 asks for a reflection on sustainability but the report template has "Analys" only; the Tidwell chapter and the CO2 article are not tied to the requirements (e.g. no one asks the student to estimate CO2 with the formula from the article). "Laddningstid" is not defined (DOMContentLoaded vs load vs finish).
- **What I did about it:** IKEA (from kmom01-03) + two others; load time = "Finish"/load from Network panel; ran cache-disabled.
- **Suggested fix:** Align the spec with the video ("tre webbshoppar totalt, varav en från kmom03"); define load time; ask for a CO2 estimate using the article.

### Q16. Reading and videos
- **Where:** `kmom04.mdx` "Läsa & titta"
- **Type:** other
- **What I wondered / got stuck on:** Chapter 10 of *Designing Interfaces* is behind the library login and could not be read (skipped). The NN/g and CO2 articles were read from the web; I did not read the referenced MDPI paper. Stated time is 8 h reading + 6 h exercises + 6 h assignments + 1 h reflection = 21 h for the kmom. The lecture video (captions) is mostly about searching research (Open Access tools), form validation/regex and CSS; none of it maps to a requirement in the webbshoppen del 4 spec or the report spec. Both videos contain code/tool demos that depend on what is on screen. The page text says the genomgång is "Tisdag" and the lecture "Onsdag", which is wrong for students watching later. The genomgång mentions "tre webbshoppar" while the spec says "ytterligare 2" (see Q15), and the demo video shows a form with address fields (Q8).
- **What I did about it:** The reflection answers are written from the NN/g article and my own work, **simulated** (no real student experience, no Tidwell chapter, and no real card payment was completed).
- **Suggested fix:** Link the lecture's validation content to the assignment, and reword the day references.

### Q17. Deploy workflow fails in the fork: GitHub Pages is not enabled
- **Where:** `.github/workflows/static-deploy.yml`, CI on `kmom04` push
- **Type:** tooling
- **What I wondered / got stuck on:** "Deploy static content to Pages" fails in "Setup Pages": `Get Pages site failed. Please verify that the repository has Pages enabled`. Lint (Node 20/22) and agent-policy pass. Probably the same on every student fork until Pages (source: GitHub Actions) is enabled; the Stripe `return_url` in the exercise (`location.href`) and the API docs example (`github_user_name.github.io/webshop/order.html`) assume a deployed page, but nothing in kmom04 mentions enabling Pages.
- **What I did about it:** Did not change the fork settings. Logged only.
- **Suggested fix:** Tell students to enable Pages (Settings > Pages > GitHub Actions) in kmom01 if not already there, or make the workflow tolerant (`enablement: true`).

### Q18. PR cannot be created with `gh pr create` (SAML), REST works
- **Where:** hand-in, `gh pr create --repo efostud/webshop ...`
- **Type:** tooling
- **What I wondered / got stuck on:** Same as kmom02 Q17: GraphQL fails with "Resource protected by organization SAML enforcement (repository.parent)" because the fork's parent is in the `dbwebb-jsux` org.
- **What I did about it:** Created the PR against the fork itself via the REST API (`gh api repos/efostud/webshop/pulls`): https://github.com/efostud/webshop/pull/3, base `efostud/webshop:main`, head `kmom04`. Not merged.
- **Suggested fix:** Authorize the token for the org (`gh auth refresh` + SSO), or note in the skill that REST is the workaround.

### Q19. ESLint/Stylelint caught `font-family: inherit` in the new `forms.css`
- **Where:** `forms.css`, `css/font-family-fallbacks` rule
- **Type:** tooling
- **What I wondered / got stuck on:** The exercise's `.input` uses `font-family: var(--default-font)`, which the starter's lint config may flag the same way; the exercise's CSS variables (`--default-font`, `--default-font-size`, `--default-line-height`, `--default-margin-bottom`) do not exist in the starter's `variables.css` either.
- **What I did about it:** Used `"Noto Sans", sans-serif` and my own values.
- **Suggested fix:** Use values the starter defines, or define those variables in the starter.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
|---|---|---|
| Läsa & titta | 8 h | not done fully (book skipped); ~0.3 h for two web articles and caption reading |
| Öva (Stripe + orderformulär) | 6 h | ~1.5 h (a student would need more, debugging Q1, Q6, Q7) |
| Uppgift: Webbshoppen del 4 | 6 h | ~1 h (included in Öva; flow verified only in pieces) |
| Uppgift: Resurser på webben | (part of 6 h) | ~1 h automated; a human doing 9 pages by hand in DevTools: ~1.5-2 h |
| Reflektera | 1 h | ~0.2 h (simulated) |

## Things that worked well
- The demo video clearly shows the finished flow (cart, Stripe checkout, form, order created).
- Using a shared Stripe test backend is a good idea; no setup needed.
- The embedded checkout is genuinely easy to mount; the shared test backend means no account is needed.
- The Lager API order and order_items endpoints behave as documented (`POST /v2/orders` returns `data.id`).
