# TODO — JSUX course improvements (from DV1705 H25 lp2 course evaluation)

Source: `Kursvärdering - Rapport med fritextsvar - DV1705 H25 lp2 UX-design med JavaScript.pdf`
(22/72 responses, 30.6% response rate, kursindex 3.8/4). Overall reception was very positive — these
are concrete, sourced improvement items pulled from the free-text answers, for the Nov 2026–Jan 2027 run.

1. **Frame the "why" before the "how" in coding demos.** A student asked for a short conceptual intro
   before diving into code — e.g. "we're fetching product data, that's usually done via an API, an API
   is X, there are several kinds of calls..." — rather than jumping straight to syntax. Apply when
   scripting kmom lecture segments, especially in kmom03–kmom05 where new APIs/concepts are introduced.

2. **Fix lecture recording audio.** Mic placement picks up desk/surface vibration, causing unwanted
   noise. Check mic mounting/isolation before recording this year's videos.

3. **Keep the vocal-intensity variation in recordings (praised as a deliberate engagement technique),
   but don't let volume dip too low at key instructional moments** — one respondent said quieter
   passages made instructions hard to fully hear.

4. **Increase feedback frequency across kmom submissions.** Multiple comments noted real feedback
   only came on the first and last hand-in ("Förutom första och sjätte inlämningen, har egentlig
   feedback inte förekommit"). Add a lightweight feedback checkpoint (even short) for the in-between
   kmoms, not just kmom01 and kmom10.

5. **Replace some "how-to" videos with written step-by-step text for mechanical setup tasks**
   (explicitly called out: connecting the repo to GitHub). Video makes it easy to mis-click among many
   options and hard to double-check a step later; text is easier to follow precisely and re-check.

6. **Add an explicit "definition of done" checklist per uppgift.** A student wanted something like
   "when finished you should have 2 HTML files, 1 CSS file, 2 JS files" to build confidence that
   they're on the right track. Add this to `uppgifter/*.mdx` specs, especially early ones
   (`webbshoppen_del_1..3`) for students newer to programming.

7. **Add more optional JavaScript fundamentals material** for students who find the language itself
   hard, plus **more live/recorded coding sessions** showing an experienced developer's actual
   problem-solving process (requested by more than one respondent, and already praised where it
   exists — expand rather than introduce).

8. **Consider adding a data-persistence/SQL segment.** One respondent wanted more coverage of storing
   and retrieving data from a real database in a larger project, beyond the current in-browser/API
   handling (`kunskap/lager_apit.mdx` is the closest existing article — evaluate whether to extend it
   or add a new kunskap page).

9. **Reduce/streamline the written reflection ("TIL"-style) reporting load, and encourage more
   screenshots/visual evidence of work instead of prose-only reports** — international student
   feedback specifically. Check `reports/` expectations and the reflection-text grading criteria in
   `index.mdx`.

10. **Tighten scope wording on open-ended tasks like responsiveness**, e.g. specify a small fixed set
    of breakpoints to test/name (e.g. "test and name up to 3 breakpoints") instead of "all sizes" —
    without removing the general creative freedom students explicitly praised elsewhere.

11. **Address the "AI / Claude Code" elephant in the room.** A student explicitly asked whether the
    skills taught here stay relevant given AI coding agents (compared it to "a typing course in the
    90s"). Consider a short discussion/reflection segment — ties naturally into kmom06 (AI moment) and
    `kunskap/fusk_och_disciplin.mdx` — about what durable skills the course builds vs. tool use.

12. **Add a short presentation/pitch exercise**, in-person or recorded, so students get practice
    presenting their work to others (explicitly requested, ties into UX communication skills the
    course already values).

13. **Keep and consider expanding alumni guest visits** ("besök av tidigare studerande") — praised,
    with a direct ask for more of it.

14. **Make live Q&A pauses actually substantive.** Feedback described post-lecture "any questions?"
    moments as too rushed (a ~3 second pause before moving on). Build in real pause time and consider
    asking for questions more than once per session.

## Student run findings (kmom01, 2026-09-30)

Source: `student-run/kmom01-questions.md` (run 1, 19 entries; the log has details and suggested fixes).

15. **Fix lint conflicts in code snippets.** Pasting the snippets from `kunskap/lager_apit.mdx` and
    `kunskap/forstasidan_for_en_webbshop.mdx` verbatim fails the starter's own lint (4-space indents,
    a trailing semicolon, trailing whitespace). Appending the page's `body` rule to `style.css` also
    triggers `no-duplicate-selectors`. Make the snippets lint-clean and say "replace" vs "add" explicitly.

16. **Write the Postman/POST step out as text.** `kunskap/lager_apit.mdx` never says what the POST body
    contains (`x-www-form-urlencoded`, key `api_key`, expect `201 Created` and 12 albums); it only
    works if you watch the video. Add the request as text, with a `curl` example. Fits #5.

17. **Definition of done and a default hero image for "Webbshoppen del 1".** Four open requirements, no
    checklist, and the starter has no `assets/` or placeholder hero image. Ties to #6.

18. **Re-record or annotate the weekly genomgång video** (`oBAUUkz5XG4` in `kmom01.mdx`). It is from the
    first run: "helt ny kurs", "sju kursmoment", and a specific schedule. Ties to #2/#3.

19. **Explain that the API key is committed and published on purpose.** `models/auth.js` is not
    git-ignored and the key ends up in public JS on GitHub Pages. Say why that is acceptable here.

20. **Move reflections to a `reflections/` folder in the `webshop` starter, one `kmomXX.md` per kmom.**
    `reports/` is used for the analysis reports in kmom01–05 and kmom10, and
    the kmom pages, hand-in video and kmom10 disagree on where the reflection goes (report file vs PR
    description). Add template files `reflections/kmom01.md`…`kmom06.md` and `kmom10.md` in the starter, and
    update all kmom pages ("Reflektera"/"Redovisning") to say where to write. The kmom01 hand-in video
    (`YD0tE7FW6i0`) still shows pasting the answers into the PR description and needs re-recording or a note.

Run 2 (kmom01 again, after the reflections change), source: `student-run/kmom01-run2-questions.md`:

21. **Tell students what green looks like after the first push.** If GitHub Pages is not set to "GitHub
    Actions" (fork setup step 3), "Deploy static content to Pages" fails with the cryptic "Get Pages site
    failed" error, on the PR and on every push. Add "after the first push all checks should be green; if
    'Deploy static content to Pages' is red, go back to step 3" to the setup text and the hand-in section.

22. **Add an "Uppdatera din fork" section.** Changes to the `webshop` starter (like the new `reflections/`
    folder) only reach forks created afterwards. Explain "Sync fork" on GitHub, then `git pull` on `main`
    and merge/rebase into the kmom branch, and decide how template changes are announced.

23. **Cover web fonts and proprietary fonts.** Typsnitt och färg / Webbshoppen del 1 ask students to take
    inspiration from a shop's typography, but shops often use their own or paid fonts (IKEA, Typekit). Say
    what to do (pick a similar free Google Font), show how to add one (`<link>` or `@font-face`), and
    mention the sustainability trade-off.

24. **Handle uneven image heights in the product list.** Album covers have different proportions, so a card
    grid gets uneven card heights. Mention `aspect-ratio` and `object-fit: cover` in
    `kunskap/forstasidan_for_en_webbshop.mdx`.

25. **Optional: CI check for unfilled reflection templates.** A `reflections/kmomXX.md` that still contains
    "Skriv ditt svar här." passes every check. Consider a check in the starter's CI that warns or fails for
    the current `kmom*` branch. Ties to #20.

26. **Remove all walkthrough and lecture videos.** Remove the "Veckans genomgång" and "Veckans föreläsning"
    `<YouTube>` embeds and their sections from the kmom pages (kmom01–06, kmom10), since they are
    outdated (last year's dates, sick-leave notes, plans for other kmoms, flows that no longer match the
    exercises). Check for content only found in the videos (e.g. the validation/CI and custom events
    lecture in kmom03, the kmom02 `localStorage` demo) and move what is still needed into the written
    material. Keep the "done" demo videos at the top of each kmom unless decided otherwise. Note that the
    "Keep" list below praises coding sessions in lectures; that refers to live lectures, not the recordings.

## Keep — don't regress these while making the above changes

- The whole-course single project that builds up kmom-by-kmom (webshop), praised repeatedly.
- GitHub-based submission workflow — explicitly called "reassuring" and easy to work with.
- Fast Discord-based support/feedback turnaround — praised multiple times, preferred over
  scheduled office-hours-only models.
- Coding sessions/live problem-solving embedded in lectures.
- The blend of theory + practice, and the real-world-feel of tasks (e.g. the Ikea example).
- Creative freedom in how students implement solutions.
- Sustainability/CO₂ content tie-in — called out as a valued, relevant addition.
