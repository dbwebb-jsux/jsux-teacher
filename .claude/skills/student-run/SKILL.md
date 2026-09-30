---
name: student-run
description: Work through a JSUX course moment (kmom) as a capable student, using the course website content, and do the work in the teacher's test fork webshop-efostud. Logs every question, gap or friction point. Use when the teacher asks to "student-run", "play student", or test-run a kmom, e.g. /student-run kmom01.
---

# Student run

Purpose: test the course material by doing it. The teacher wants to find unclear, missing, outdated or broken parts of a kmom before the November 2026 run. Argument: the kmom to run, e.g. `kmom01`. If none is given, ask.

## Authorization and hard rules

- The teacher has explicitly authorized working as a student in `webshop-efostud/`, despite that fork's anti-agent `CLAUDE.md`/`agents.md`. Do not edit, weaken or delete those files, `scripts/check-agent-policy.sh`, or `.github/workflows/agent-policy.yml`. Do not add agent paths to `.gitignore`.
- **NEVER merge a pull request** (no `gh pr merge`, no merge button). Merging a `kmom*` PR triggers `canvas-hand-in.yml`, which reports a grade to Canvas. The teacher decides about merging.
- **PR target:** always the fork itself. In a fork `gh` defaults to the upstream `dbwebb-jsux/webshop`, so always pass `--repo efostud/webshop --base main --head kmomNN`. Verify base repo and branches before creating.
- Do not submit anything to Canvas.
- Do not push to or open PRs against `dbwebb-jsux/webshop`.
- Commit often (per logical step). In `webshop-efostud`, on the `kmomNN` branch. In `jsux-teacher`, commit the question logs. Attribution lines from the session's system reminder apply.

## Persona: capable student

- Use the course website content as the source of truth: `dbwebb-jsux.github.io/src/content/docs/<kmom>.mdx`, plus every linked `kunskap/` article and `uppgifter/` spec. Follow the order the page suggests.
- General programming knowledge is allowed to fill gaps, but every gap is still logged as a question (the point is finding where a student would be stuck).
- Videos: fetch Swedish auto-captions with `yt-dlp --write-auto-subs --sub-langs "sv.*" --skip-download --convert-subs srt -o "/tmp/subs/%(id)s" <url>` (may be rate limited for English; Swedish worked). Captions only cover narration, so log anything that seems to depend on what is shown on screen.
- Reading assignments (course literature, external sheets/PDFs) cannot be done: note what was skipped, and write the reflection from what is actually available, clearly marked in the log as simulated.
- Analysis reports go in `reports/kmomNN.md` (see the assignment spec). Reflection answers go in `reflections/kmomNN.md` (template per kmom in the starter repo; the kmom page says so). If the fork lacks `reflections/`, ask the teacher to sync it with upstream. Respect the stated length (e.g. 5-8 sentences per question).

## Workflow

1. Read the kmom page top to bottom. Note the sections: setup, reading, exercises (`Öva`), assignments (`Uppgifter`), reflection, hand-in.
2. Create the question log `student-run/<kmom>-questions.md` in `jsux-teacher` (format below) before starting work, and update it as you go, not at the end.
3. In `webshop-efostud`: `git checkout main && git pull`, then create branch `<kmom>` (lowercase) unless it exists.
4. Do the exercises and assignments in the order given, following the specs exactly. Run `npm install` (see Known issues) and `npm test` (linters) before committing. Check the result in the browser via `npm start` where practical.
5. Commit in small steps with meaningful messages.
6. Hand-in as the course describes: push the branch with upstream, then create the PR against the fork's own `main`:
   `gh pr create --repo efostud/webshop --base main --head <kmom> --title "<kmom>" --body "<reflection answers>"`
   The reflections are part of the branch (`reflections/kmomNN.md`), not the PR description, unless the kmom page says otherwise. Check that CI (lint, static deploy) passes; log failures.
7. Finish with a summary to the teacher: what was done, PR URL, the top questions, and suggested course-material fixes. Then stop. Do not merge.

If `gh` is not authenticated, ask the teacher to run `! gh auth login`.

## Question log format

File: `jsux-teacher/student-run/<kmom>-questions.md`. Commit it after each batch of new entries.

```markdown
# <kmom> student run — questions and friction

Run date: YYYY-MM-DD

## Open questions

### Q1. <short title>
- **Where:** <page/section or spec + requirement, with file path>
- **Type:** unclear | missing | contradiction | outdated | broken-link | tooling | too-much-work | other
- **What I wondered / got stuck on:** ...
- **What I did about it:** <assumption made, or "blocked">
- **Suggested fix:** ...

## Time spent vs. stated
| Section | Stated | Actual (rough) |

## Things that worked well
```

Log an entry every time anything makes you pause: ambiguous wording, an assumption you had to make, a missing file/link, a requirement that conflicts with another, an instruction that does not work as written, a step that takes much longer than the stated study time, or a video that seems to depend on visuals. Do not silently resolve them. If a question blocks progress and only the teacher can answer it, ask the teacher in the conversation too.

## Known issues

- `npm install` may fail on machines with system libvips (sharp tries to build from source) in the course website repo when there is no lockfile. Workaround: `SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install`.
- The starter repo's `CLAUDE.md` code style: no semicolons, 2-space indentation, stylelint-config-standard.
