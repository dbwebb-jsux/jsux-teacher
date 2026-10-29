# TEACHER_TODO — actions only the teacher can take

Sources: `TODO.md` (course evaluation + kmom01 runs) and the student-run logs `student-run/kmom01..06,10`
(run 2026-09-30 … 2026-10-08). Items that the agent can do in the repos are *not* here; they are listed
in the chat summary. Cross-references: `T#` = `TODO.md` number, `kmomNN Qn` = student-run log entry.

Legend: **[decide]** needs a decision before the text can be fixed · **[do]** external action, no way for the agent to do it.

## 1. Recording and live teaching

- [ ] **[do] Fix mic/desk vibration before any new recording** (T2).
- [ ] **[do] Keep the vocal variation, avoid volume dips at instruction moments** (T3).
- [ ] **[do] Decide which "done" demo videos to keep, re-record those that no longer match the spec.** T26 keeps
      them, but the runs found mismatches:
  - kmom02 `caw-OVXaaB8`: hero image, gradient, theme selector on the front page, not in the spec (kmom02 Q14).
  - kmom03 demo: analyses another shop, has quantity controls (kmom03 Q11). Probably fine.
  - kmom04 demo: form has address/zip/city/country, the exercise form has only name + e-mail (kmom04 Q8).
  - kmom06 demo: bot that doesn't answer / doesn't know the albums (kmom06 Q5, Q10).
  - Alternative to re-recording: change the spec to match the video (see section 2).
- [x] Kmom01 hand-in video replaced with the new recording `6fuj61ZiUkE` (T20), commit `c9810fd` in `dbwebb-jsux.github.io`. The old "Obs" note about the PR description was removed with it.
- [ ] **[do] kmom05: IKEA guest talk `-SbmENFroCk` is a private video** (kmom05 Q1). Make it unlisted/public
      (check the speakers' consent) or tell me to remove the section.
- [x] T26 done (committed, not pushed): "Veckans genomgång"/"Veckans föreläsning" (kmom01–06) and "Genomgång av projektet" (kmom10) are removed. Kept: demo videos, kmom01 setup/hand-in/feedback videos, sustainability videos, the IKEA talk, the API-key and Lighthouse videos.
- [ ] **[decide] Video-only content that was not carried over** (no way to fetch captions in this session, so only what the logs identified was moved):
  - kmom03 lecture: testing/validation, CI failures on Node 20/22, custom events (kmom03 Q11, Q12).
  - kmom04 lecture: research search/Open Access tools, form validation/regex beyond the article (kmom04 Q16).
  - kmom10 genomgång: grading explanation ("30 + 30 = 60, bara D"); depends on your grading-table decision.
  Tell me which you still want, as text or as new short videos.
- [ ] **[do] Live sessions, in the course run** (T1, T7, T11, T12, T13, T14):
  - "why before how" intro in lectures, esp. kmom03–05.
  - More live/recorded coding sessions and optional JS-fundamentals material.
  - AI-relevance discussion (kmom06 + `fusk_och_disciplin`). I can draft talking points.
  - Short pitch/presentation exercise (T12). Note kmom10 already has a presentation video, see Q10 below.
  - Alumni visits (T13). The kmom05 IKEA talk is one; book new ones.
  - Real pause for questions, more than once per lecture (T14).
- [ ] **[do] Feedback cadence** (T4): decide the lightweight checkpoint for kmom02–06 and who gives it.
      Related: the runs show the PRs are never merged before the next kmom (see Q1 below), which affects how feedback is given.

## 2. Decisions that block text fixes

Give me a one-line answer per item and I will make the edits.

### Workflow / structure
- [ ] **[decide] Branching model** (kmom02 Q1, kmom03–10 stacked PRs). Options: (a) each kmom branch from the
      previous kmom branch, PR shows earlier commits; (b) you merge each PR before the next kmom; (c) students merge their own
      PR after feedback. Needed to write the "Skapa kmom0X" instructions and T22 ("Uppdatera din fork").
- [ ] **[decide] Final folder layout** (kmom02 Q2, kmom03 Q2, kmom04 Q3, kmom05 Q2). Recurring problem: articles assume
      `views/main.js`, `style/*.css`, `../components/`; starter has `main.js` and CSS in the root.
      Option A: change the articles to the starter's layout. Option B: change the starter (move to `views/`, `style/`).
- [ ] **[decide] Should `models/products.js` and `models/orders.js` be given** in the articles (kmom03 Q1, kmom04 Q1/Q11)
      or kept as "write it yourself"? Recommended: show them (10–20 lines each).
- [ ] **[decide] `models/auth.js` with a key committed on purpose** (T19, kmom06 Q2): accept as classroom compromise and explain,
      or offer an alternative (proxy, `auth.local.js`)? The Azure OpenAI key is *personal* and could be revoked by secret scanning.

### Spec / grading
- [ ] **[decide] Theme selector on every page, or only the category page?** (kmom02 Q14)
- [ ] **[decide] Hard-coded ids accepted for the three category albums, or must filter on `category`?** (kmom02 Q8)
- [ ] **[decide] Cart required on `category.html` too?** (kmom03 Q7). Counter = sum of quantities or distinct products? Quantity 0 removes? (kmom03 Q8)
- [ ] **[decide] Required order-form fields** (kmom04 Q8): name, address, zip, city, country? Otherwise the "input type" requirement is untestable.
- [ ] **[decide] Fixed 100 kr price in the Stripe exercise, or use `price`?** (kmom04 Q2) — Depends on the Lager API, see section 3.
- [ ] **[decide] How many webshops in Resurser på webben** — spec says "2 more", video says "three in total" (kmom04 Q15). Does your own shop
      count, and must students run Lighthouse on their own shop (kmom05 Q15)? Recommended: yes, it was the most instructive result of the run.
- [ ] **[decide] Which shop pages count when a shop has no cart page** (kmom04/05 Q13): "choose another shop" or "write that it is missing"?
- [ ] **[decide] Edge cases accepted or not** (kmom04 Q5, kmom10 Q13): double order submit, empty cart on `order.html`, payment done but order failed.
- [ ] **[decide] Chilla lite** (kmom06 Q7): explicitly "no hand-in, no report", or remove it from the list? Does it count for Ladok?
- [ ] **[decide] Intentional openness in del 6 and kmom10 Krav 4** (kmom06 Q6, kmom10 Q4): keep it open and say so, or add
      concrete minimums (e.g. widths 320/768/1280 and which pages)? Same for T10 (breakpoints).
- [ ] **[decide] kmom10 presentation video** (kmom10 Q10): max length, language, hosting (YouTube link vs file in PR), does it affect the grade?
- [ ] **[decide] kmom10 Krav 2 scope** (kmom10 Q7): 27 measurements for 15 p. Reduce, or give expected size and the template to use?
- [ ] **[decide] Study-time hours for kmom06 reading and for kmom10** (kmom06 Q8, kmom10 time table).
- [ ] **[decide] Grading table** (kmom10 Q9): fix FX/F overlap (50–54), and do you want to say "two requirements gives at most C"?
- [ ] **[decide] Reflection/report load** (T9, T20): is "5–8 meningar per fråga" and the report volume OK? Screenshots instead of prose?
- [ ] **[decide] CI check for unfilled reflection templates** (T25): warn or fail? Policy is yours.
- [ ] **[decide] SQL/persistence segment** (T8): new kunskap page or extend `lager_apit.mdx`? Skip for this run?
- [ ] **[decide] Fonts** (T23): OK to tell students to use a free Google Font and mention the CO₂ trade-off?

## 3. External systems and accounts (outside the repos)

- [x] Lager API changes deployed (PR #46 `student-run-fixes` merged to `master`, deployed 2026-10-29; thumbnails, `thumbnail_url`, `?category=`, Stripe JSON errors). Verified live 2026-10-29 with a key for efo@bth.se: `?category=` filters, `thumbnail_url` is set and thumbnails/images load from jsDelivr. (The 6× duplicated albums seen in that test are from earlier copies already stored under that key, not a seed problem.) The course pages that use them were edited but **uncommitted** in `dbwebb-jsux.github.io` at the time; check whether they are published now.
  - Not planned: duplicate-order protection (kmom10 Q13), accept as a documented limitation.
  - **[decide]** Category values `hard rock`/`hiphop`: rename (breaks existing students' code and the demo videos) or keep and document? Default: keep and document.
  - **[decide]** `price`: seed stores 100 (öre), API returns `price / 100` = 1. Stripe exercise uses 10000 öre. Default: keep and explain in the article; or seed real prices (e.g. 10000 → API returns 100) so the exercise can use `price * 100`.
  - Note: images are served from jsDelivr (`cdn.jsdelivr.net/gh/emilfolino/order_api@master/public/img/...`), so new thumbnails only go live after they are pushed to `master` on GitHub (jsDelivr caches; purge if needed).
  - Not planned: duplicate-order protection (kmom10 Q13), accept as a documented limitation.
- [ ] **[do] Azure OpenAI key flow** (kmom06 Q1, Q10): the key arrives hours after ordering in TopDesk. Confirm the process still works in
      Nov 2026, and say whether you can/will remind students in kmom05. Check quotas ("kalibrera quotas") and tell me the official contact route (Discord?).
      Decide whether to provide a proxy endpoint like the Stripe one.
- [ ] **[do] Give the agent a real key for a live test** of `azure_openai_api` (kmom06 Q1) — the answer so far is from a stub.
- [ ] **[do] SAML for the test fork** (kmom02 Q17, kmom04 Q18): authorize the `gh` token for the `dbwebb-jsux` org
      (`gh auth refresh -h github.com -s repo` + SSO authorize). Otherwise student-run PRs need the REST workaround.
      Students are not affected.
- [ ] **[do] Merge or close the stacked test PRs** on `efostud/webshop` (#3–#6) when you have looked at them.
- [ ] **[do] Library/Canvas access** (kmom02–06 Q16): book chapters 3, 5, 7, 8, 10, 11 and the Canvas sustainability video were not accessible to the agent, so the
      reflection questions about them were never tested. Check the links still work for students and that the reading-list
      page numbers are right. Nothing for me to do.
- [ ] **[do] Check the Ladok mapping and kursplan in `index.mdx`** if kmom06 "Chilla lite" or kmom10 points change.
- [ ] **[do] Announce template changes to existing forks** (T22) once the fork-update section exists: channel and wording are yours (Discord?).
- [ ] **[do] Study/lesson plan Google Sheets** (external, not in repo): update dates and kmom text (e.g. "Tisdag", "Onsdag", dates in the old videos) to the Nov 2026 run.
- [ ] **[do] Enable GitHub Pages in the teacher's own test fork** if you want a green CI in `webshop-efostud` (kmom04 Q17). Student forks need the setup step, see agent list.
