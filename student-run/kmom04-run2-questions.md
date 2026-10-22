# kmom04 student run (run 2) — questions and friction

Run date: 2026-10-22
Persona: capable student. Source: `kmom04.mdx`, `kunskap/integrera_stripe.mdx`, `kunskap/orderformular_for_webbshoppen.mdx`, `uppgifter/resurser_pa_webben.mdx`, `uppgifter/webbshoppen_del_4.mdx`.
Work is done in `efostud/` on `kmom04-run2`, branched from `kmom03-run2`. Videos not read (no `yt-dlp`); demo video `fnpzVAJHA1Y` unverified.
Side effect: I created one test order via `POST /v2/orders` (id 16924, "Test") in the API under the fork's API key while testing the endpoint with `curl`.

## Open questions

### Q1. Stripe payment flow could not be run end to end
- **Type:** tooling
- **What I did about it:** Verified with `curl`: `create-checkout-session` returns a `clientSecret`, `session-status` with a fake id returns 404 JSON (page shows the error text, checked in headless Chrome), `POST /orders` works. The embedded checkout form, a test-card payment and the `complete` branch (form shown, order + order items created, cart cleared) are **untested**. Verify manually with test card 4242 4242 4242 4242.

### Q2. Empty cart on `order.html` gives a broken checkout
- **Where:** `integrera_stripe.mdx`, `buildItems` / `initializeCheckout`
- **Type:** missing
- **What I wondered / got stuck on:** With an empty cart `items` is `[]` and the API answers 400 "items must be a non-empty array", so `clientSecret` is `undefined` and `initEmbeddedCheckout` throws, leaving a blank page. A student who types `order.html` directly or reloads after clearing the cart gets this. The exercise only hides the button for an empty cart.
- **What I did about it:** Followed the exercise (not handled).
- **Suggested fix:** Add a guard ("Din varukorg är tom") before `initializeCheckout`.

### Q3. Reloading `order.html?session_id=...` after the order allows duplicate orders
- **Where:** `orderformular_for_webbshoppen.mdx`
- **Type:** missing
- **What I wondered / got stuck on:** After the cart is cleared the URL still contains the completed `session_id`, so the form is shown again and a second submit creates an order with an empty `cart` and the same `payment_id` (an order without items). There is also no double-click protection on submit.
- **What I did about it:** Hid the form and showed "Tack för din beställning" after success; did not handle reloads.
- **Suggested fix:** Mention it as a known limitation, or redirect to a confirmation view after success.

### Q4. `hidden` on the "Beställ" button does not work with the exercise's CSS
- **Where:** `integrera_stripe.mdx` ("göm den ... när `renderCartProducts()` skriver ut ...") + `orderformular_for_webbshoppen.mdx` (`.button { display: inline-block }`)
- **Type:** unclear
- **What I wondered / got stuck on:** The natural way is `button.hidden = true`, but `.button` sets `display`, which overrides the browser's `[hidden]` rule, so the button stays visible. Also the exercise text still quotes the English "No products in cart." from the old `cart.js`, while my kmom03 version writes something else; it is a string the student can no longer find.
- **What I did about it:** Added `.button[hidden] { display: none }` and hide it in `renderCartProducts`.
- **Suggested fix:** Show a 3-line example of hiding (class `hidden` or `[hidden]` rule) and say to hide it based on the cart contents, not on a text string.

### Q5. `order.html` is a copy of `index.html`, but the snippet shows paths that do not match
- **Where:** `integrera_stripe.mdx` "Ett order-formulär"
- **Type:** unclear
- **What I wondered / got stuck on:** The example uses `style/variables.css`, `views/main.js`, `assets/hero-banner.jpg`, a footer "Här skrivs massa", and says "anpassa". Fine, but it does not mention that `cart.js` needs the cart markup to exist on `order.html` (top-level `getElementById`, see kmom03-run2 Q2), nor that `mode-selector.js` needs the `#mode` select. A student who deletes "everything but the form" gets a JS error.
- **What I did about it:** Copied `index.html` and removed the product list and hero.
- **Suggested fix:** List which elements/ids must remain (`#cart`, `#cart-content`, `#cart-products`, `#close-cart`, `#mode`).

### Q6. The assignment says nothing about validation requirements beyond "passande typ"
- **Where:** `webbshoppen_del_4.mdx` req. 3 and 4.1
- **Type:** unclear
- **What I wondered / got stuck on:** Req. 4.1 wants "information om kunden från ditt formulär", but the exercise's form has only first name and e-mail (and the e-mail field is read-only and fetched from Stripe), so there is hardly any field where input type matters. The API accepts address, zip, city, country. Which fields are expected is open; the exercise teaches `tel`, `date`, zip with `pattern`.
- **What I did about it:** Kept the exercise form (name + email) only. A student aiming for req. 3 would add address, zip, phone.
- **Suggested fix:** List the required fields in the spec (e.g. name, address, zip, city, phone).

### Q7. Resource-analysis assignment can't be done without a browser, and "three tables" are not explained
- **Where:** `uppgifter/resurser_pa_webben.mdx`
- **Type:** tooling / unclear
- **What I wondered / got stuck on:** Requirement 1 says "ytterligare 2 webbshoppar" and the template has three tables (Sida 1-3), where the headers are "Sida 1/2/3" although the rows are pages and the three tables seem to be three webshops. Rename the headers to the shops. It is also unclear whether the student's own webshop counts as one of them. Req. 3 wants a product page and an order page, which many shops hide behind cart state or login. Also: "Stå i kursrepot" (see kmom02 Q11).
- **What I did about it:** An agent without devtools: measured only a lower bound for IKEA's front page via `curl`; other shops (Clas Ohlson, Rusta) were bot-blocked/404. The report says it is incomplete.
- **Suggested fix:** Name the table headers after shops; say if your own webshop counts; add "Stå i ditt repo".

### Q8. CO2 formula versus article
- **Where:** `resurser_pa_webben.mdx` req. 6
- **Type:** unclear
- **What I wondered / got stuck on:** The text says "0,5 kWh per GB × 295 g CO2 per kWh. Alltså cirka 0,15 g CO2 per MB". 0.5 × 295 = 147.5 g/GB = 0.1475 g/MB, consistent. Fine. Not checked against the article (not fetched).

### Q9. Reading assignment
- **Where:** `kmom04.mdx`
- **Type:** other
- **What I did about it:** Skipped the book chapter 10, the NN/g article and the CO2 articles (reflection Q1 and TIL are marked as simulated, and Q1 says the article was not read).

### Q10. Study time
- **Where:** "Läsa & titta" says 8 h, other sections 6 h each (not 6 as in earlier kmom)
- **Type:** other
- **What I wondered:** The stated 8 h for reading differs from the 6 h used in kmom02 and 03, for a chapter plus three articles; probably intentional, noting it.

### Q11. PR not created
- **Type:** tooling
- **What I did about it:** `gh` not installed; branch pushed. Compare URL: `https://github.com/efostud/webshop/pull/new/kmom04-run2`.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
| --- | --- | --- |
| Läsa & titta | 8 h | Not done |
| Öva | 6 h | ~45 min of coding, mostly copying the exercise code; no payment run |
| Uppgifter | 6 h | Report cannot be done without a browser; coding ~15 min |
| Reflektera | 1 h | Simulated |

## Things that worked well
- The exercises now contain complete, working code and call out pitfalls (`readonly` vs `disabled`, number vs text for zip, `:user-invalid`, top-level `await`, `type="module"`).
- The Stripe test-server design is nice: `create-checkout-session` and `session-status` respond with clear JSON errors.
- Adding the "clear cart + update counter" requirement links kmom03 and kmom04 properly.
