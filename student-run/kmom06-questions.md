# kmom06 student run — questions and friction

Run date: 2026-10-08
Persona: capable student. Source: `kmom06.mdx`, `kunskap/azure_openai_api.mdx`, `uppgifter/chilla_lite.mdx`, `uppgifter/webbshoppen_del_6.mdx`.
Work is done in `webshop-efostud` on branch `kmom06`, branched from `kmom05` (earlier kmom PRs are not merged).
Videos read via Swedish auto-captions (demo, genomgång, API-key video, lecture). The lecture was only skimmed (about 32 000 characters of captions).

## Open questions

### Q1. The API key must be ordered by the student in BTH's TopDesk portal; I cannot do that
- **Where:** `kunskap/azure_openai_api.mdx`, "En API-nyckel till Azure OpenAI's API"
- **Type:** other
- **What I wondered / got stuck on:** The key arrives by e-mail "efter någon timme" (video) after an order in `bth.topdesk.net` with student SSO. A student who starts the exercise without having ordered the key in advance is blocked for hours. The endpoint answers `401 Access denied due to missing subscription key` without a key (verified with curl), so nothing can be tested live without it. The genomgång says "ni får jättegärna göra det nu", but the kmom page only mentions the key inside the exercise, and not at the top of `kmom06.mdx`. A student who works from the kmom page order (reading, then exercise) loses a day.
- **What I did about it:** Built the chat against the documented request/response shape and tested with a stubbed response in the browser. Real API not tested. Asked the teacher in the summary for a key if a live test is wanted.
- **Suggested fix:** Put "Beställ API-nyckeln redan nu" at the top of `kmom06.mdx` (or even in kmom05), with the link.

### Q2. A personal API key is told to be stored in a committed file that is deployed publicly
- **Where:** `kunskap/azure_openai_api.mdx` ("spara undan den i `models/auth.js`")
- **Type:** other
- **What I wondered / got stuck on:** The key is personal and tied to a quota, and `models/auth.js` is committed to the fork and published by the static-deploy workflow to GitHub Pages, so it can be read by anyone (and by GitHub secret scanning, which may revoke it). Unlike the Lager and Stripe *public* keys (shared test keys), this is a secret. The course discusses in kmom04 why a secret key must not live in the frontend (genomgång: "den secret key får inte ligga på fronten"), but here it does exactly that, without a comment.
- **What I did about it:** Put an empty placeholder in `models/auth.js` (`openai_api_key: ""`) and did not commit a real key.
- **Suggested fix:** Acknowledge the contradiction (it is a classroom compromise, the key is rate limited), or offer a proxy endpoint like the Stripe one in the Lager API; or tell students to use a `.gitignore`d `auth.local.js`.

### Q3. The exercise's code listings contradict each other and do not run as written
- **Where:** `kunskap/azure_openai_api.mdx`
- **Type:** contradiction
- **What I wondered / got stuck on:**
  - `auth` is used (`auth.openai_api_key`) but never imported.
  - Listing 2 adds `<p class="user-message">`, listing 3 adds a plain `<p>` for the same thing.
  - `document.getElementById("new-message")` is used inside a component; `this.querySelector` is the usual way, and getElementById collides if two components exist.
  - The system prompt is the placeholder "...webbshoppen URL" (the text says "här väljer jag att lägga in URL'n till min webbshop"): a student can paste it literally.
  - The code uses `keyup` for Enter; with IME composition / holding Enter this double-sends, and the `<input>` has no `<form>`, so mobile "Go" button does nothing. The page tells to use `keyup` but kmom04 taught forms.
  - Each `sendMessage` sends only the system prompt and the latest message, so the bot has no memory of the conversation; a "chat" that forgets the previous turn is surprising and not mentioned.
  - 4-space indentation again (lint `indent: 2`, see kmom05 Q11).
  - `class="input new-message"`: `.input` was defined in kmom04's `forms.css` only if the student wrote it.
- **What I did about it:** Wrote my own version: form + submit, `this.querySelector`, a messages array kept in the component, `auth` imported.
- **Suggested fix:** Make one complete listing for the whole component, with imports; mention history.

### Q4. Security: `innerHTML` with user input and LLM output
- **Where:** same, `chatMessages.innerHTML += \`<p>${message}</p>\``
- **Type:** other
- **What I wondered / got stuck on:** Both the user's text and the model's answer are written with `innerHTML`, which is an XSS vector (a user typing `<img src=x onerror=alert(1)>`, or a model that echoes it). The genomgång mentions switching to `document.createElement` "för att det kan uppstå problem med eventlyssnare om man blandar", but not security. The written exercise says nothing.
- **What I did about it:** `createElement` + `textContent`.
- **Suggested fix:** Add the security reason to the exercise and use `textContent` in the listing.

### Q5. UX improvements (loading indicator, auto scroll, overflow) exist only in the genomgång video
- **Where:** genomgång video `1eHu5FMf1io`, second half; not in `azure_openai_api.mdx`
- **Type:** missing
- **What I wondered / got stuck on:** The video says "jag visar två små kodexempel": a "Laddar svar…" message with a `setInterval` dots animation, and auto-scroll. They are not in the text, so they can't be copy-searched; the captions cover only narration and the code is on screen. The request "lättanvänt chattgränssnitt" (spec req. 1) is left to the student, and the video says details such as opening and closing are up to the student. Also, in the video the chat did not answer during the recording (probably quota/key trouble, "nu svarar den inte").
- **What I did about it:** Implemented a loading indicator (animated dots via `setInterval`, cleared in `finally`) and auto scroll (`scrollTop = scrollHeight`) myself.
- **Suggested fix:** Put the two snippets into the exercise as an optional section.

### Q6. Spec for webbshoppen del 6 is two loose requirements; where must the chat appear and what counts as "lättanvänt"?
- **Where:** `uppgifter/webbshoppen_del_6.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** Which pages must have the chat? Open/close behaviour? Language? The genomgång explains that loose requirements are deliberate ("krav är inte alltid detaljerade i verkligheten"), which is a reasonable choice, but the page itself does not say so and a student cannot know when it is "done". There are no grading hints.
- **What I did about it:** Made a floating button that opens/closes a panel, on index, category and order pages.
- **Suggested fix:** Add a short note about the intentional openness and some examples of what a student may add.

### Q7. "Chilla lite" has no requirements; the intro text contradicts the course timeline
- **Where:** `uppgifter/chilla_lite.mdx`, `kmom06.mdx`
- **Type:** unclear
- **What I wondered / got stuck on:** The spec is a GIF and "sista veckan innan vi påbörjar projektet chillar vi lite." It is listed under "Uppgifter" with "lämna in enligt nedan" so there is nothing to hand in; the genomgång says there is no report this week. The text "Gör uppgiften ... och lämna in" is therefore confusing; it also counts for Ladok? (the course page's mapping not checked). The genomgång also says the project is already released ("släpptes igår"), while kmom10 must be done after, so the "sista veckan innan projektet" is wrong for students who start the project in parallel.
- **What I did about it:** Did nothing for the assignment (nothing to do), no report file created.
- **Suggested fix:** Say explicitly "ingen inlämning, ingen rapport", or remove it from the list.

### Q8. Kmom page: no study-time line for "Läsa & titta"; reading chapter 11 unavailable
- **Where:** `kmom06.mdx`
- **Type:** missing
- **What I wondered / got stuck on:** The other kmom pages have `<p class="time">` under "Läsa & titta", this one doesn't. The chapter "11. User Interface Systems and Atomic Design" is behind the library login, not readable by me, so reflection question 1 (Design Systems / Atomic Design) is based on general knowledge and my own component work, i.e. **simulated**. The genomgång describes the chapter as "ett steg upp i abstraktion".
- **What I did about it:** See above.
- **Suggested fix:** Add the time line; one sentence about what to look for in the chapter.

### Q9. Reflection asks for ITCIL, which is not in the kmom text of other weeks; template check
- **Where:** `kmom06.mdx` reflection section, `reflections/kmom06.md`
- **Type:** other
- **What I wondered / got stuck on:** The section mentions "Upplever du API sättet ... som ett sätt som öppnar möjligheter" etc. The template in the starter has all four questions, matching the page. The ITCIL ("In This Course I Learned") covers the whole course and is a good question, but "5-8 meningar per fråga" for a course-wide summary is short.
- **What I did about it:** Wrote 5-8 sentences each.
- **Suggested fix:** None.

### Q10. Videos date themselves again; the genomgång was recorded late because of a meeting
- **Where:** genomgång video (captions)
- **Type:** outdated
- **What I wondered / got stuck on:** "vår chef kallar in till möte", "projektet släpptes igår", "retreat på tisdag" in the lecture, quota discussions for "den här veckan" ("vi håller på att kalibrera quotas"). The quota remark is relevant for students: if they get a quota error they should ask in Discord, but this is only in the video.
- **What I did about it:** Ignored; noted the quota advice in Q11.
- **Suggested fix:** Re-record or note it applies to the 2025 run; move the quota/contact info to the text.

### Q11. Error handling: what does a student see when the quota is reached or the key is wrong?
- **Where:** `kunskap/azure_openai_api.mdx` last listing (`result.choices[0].message.content`)
- **Type:** missing
- **What I wondered / got stuck on:** With a bad key or exhausted quota the API returns an error JSON (401/429 with no `choices`), and `result.choices[0]` throws "Cannot read properties of undefined". The video says quota problems should be reported on Discord, but nothing in the text shows how to detect them.
- **What I did about it:** Check `response.ok` and `result.choices`, show a friendly error message in the chat.
- **Suggested fix:** Show an `if (!response.ok)` branch and the 429 message in the exercise.

### Q12. `.input` and `.button` come from the kmom04 `forms.css`; the exercise's classes have no styles of their own
- **Where:** `kunskap/azure_openai_api.mdx` (`class="input new-message"`), `uppgifter/webbshoppen_del_6.mdx` req. 1
- **Type:** missing
- **What I wondered / got stuck on:** No CSS for the chat is provided (message bubbles, scroll area, open/close). "Lättanvänt chattgränssnitt" needs about 100 lines of CSS; the exercise shows none and the video's finished chat is only a screen recording. `.input`/`.button` only exist if the student created `forms.css` in kmom04 (kmom04 Q3b), and `index.html` did not load it.
- **What I did about it:** Wrote `chat.css` and loaded `forms.css` on all pages.
- **Suggested fix:** Give a starter `chat.css` or say CSS design is part of the task.

### Q13. Deploy and PR tooling
- **Where:** hand-in
- **Type:** tooling
- **What I wondered / got stuck on:** Same as kmom04 Q17/Q18: Pages deploy fails in the fork (Pages not enabled), `gh pr create` hits SAML. Lint (Node 20/22) and agent-policy pass. Note: the reflection and the PR say the chat was tested with a stub because there was no key.
- **What I did about it:** PR via REST: https://github.com/efostud/webshop/pull/5, base `efostud/webshop:main`, head `kmom06`. Not merged. The diff also contains unmerged kmom03-05 commits.
- **Suggested fix:** See kmom04.

## Time spent vs. stated
| Section | Stated | Actual (rough) |
|---|---|---|
| Läsa & titta | none stated (Q8) | not done (book); ~0.3 h captions |
| Öva (Azure OpenAI API) | 6 h | key ordering not possible (Q1, normally hours of waiting); ~1 h coding |
| Uppgift: Chilla lite | (part of 6 h) | 0 |
| Uppgift: Webbshoppen del 6 | (part of 6 h) | ~1.5 h incl. CSS and tests with a stubbed API |
| Reflektera | 1 h | ~0.3 h (question 1 simulated; question 4 written from the simulated student runs) |

## Things that worked well
- The system/user message structure is easy to grasp and the exercise's skeleton gets the component going.
- The kmom's idea of making the chat a web component builds nicely on kmom05.
- Sending the product list in the system prompt makes the bot much more useful (the demo video's bot did not know the albums).
