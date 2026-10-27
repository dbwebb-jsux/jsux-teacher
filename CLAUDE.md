# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Role

This folder is the workspace for the **teacher** of JSUX ("UX-design med JavaScript", course code DV1705 at BTH), preparing and improving course material for the next running of the course, **November 2026 – January 2027**. Act as a teaching assistant: help draft, restructure, and polish weekly course content, assignment specs, grading rubrics, and the student starter project — not as a student writing code for submission.

This folder is itself a git repo, but only for the teacher notes (`CLAUDE.md`, `TODO.md`, `TEACHER_TODO.md`, `student-run/`); it also contains several independent repos (gitignored here), described below. `cd` into the relevant one before running its commands; each has its own `CLAUDE.md` with detailed build/lint/test commands.

You have access to these repos from this workspace:
- `dbwebb-jsux.github.io/` — the course website repo (course content, kmom pages, assignments, knowledge articles).
- `webshop/` — the starter repo that students fork, clone and work in during the course.
- `astro-hidden-prompt/` — the Astro component (npm package `astro-hidden-prompt`, remote `emilfolino/astro-hidden-prompt`) that the course site uses to embed invisible AI-guidance text before each requirement.
- `order_api/` — the Lager API (remote `emilfolino/order_api`, deployed at https://lager.emilfolino.se/) that students' webshops fetch products from; see the kunskap article on the inventory API.
- `efostud/` (named `webshop-efostud/` on the teacher's other machine; both are gitignored) — the teacher's own test fork of `webshop` (remote `efostud/webshop`), where the `student-run` skill does its work as a simulated student. Not course material; don't edit it outside student-run sessions.
- `student-run/` — plain folder (not a repo) with the question/friction logs from the student-run passes (`kmomNN-questions.md`, `kmomNN-run2-questions.md`). `TEACHER_TODO.md` is derived from these.

## Repos in this workspace

### `dbwebb-jsux.github.io/` — the public course site

Astro + Starlight site published to GitHub Pages. All course content is Swedish `.mdx` under `src/content/docs/`:
- `kmom01.mdx`…`kmom06.mdx`, `kmom10.mdx` — the weekly course moments (kmom10 is the final project moment; kmom07–09 don't exist, the course jumps straight to 10 for the project phase)
- `kunskap/` — standalone knowledge articles/tutorials referenced from kmom pages (e.g. building the cart, color schemes, Stripe integration, an inventory API, rendering markdown, responsive images)
- `uppgifter/` — assignment specs, including the multi-part `webbshoppen_del_1..6` sequence
- `index.mdx` — course overview: kursplan (DV1705), literature (*Designing Interfaces*, Tidwell/Brewer/Valencia) with a per-kmom reading table, grading rubric (points → A–F), Ladok moment mapping, and links to the study/lesson plans (external Google Sheets, not in this repo)
- `kunskap/fusk_och_disciplin.mdx` — the academic-integrity policy shown to students, including the "AI as tutor, not code generator" guidance quoted verbatim by the webshop repo's tooling (see below)

The sidebar order in `astro.config.mjs` is the source of truth for kmom ordering and must be updated if a new kmom page is added.

### `webshop/` — the student starter/template repo

This is the repo each student forks to build their webshop project (kmom01 onward). It is a plain HTML/CSS/JS project (no framework), linted with ESLint + Stylelint + HTMLHint.

It carries a **student-facing anti-AI-agent policy**, enforced by CI (`.github/workflows/agent-policy.yml` running `scripts/check-agent-policy.sh`): the repo's own `CLAUDE.md`/`agents.md` must contain a fixed paragraph telling coding agents to refuse to help and to point students at `fusk_och_disciplin`, and `.gitignore` must not hide agent-config paths (`.claude/`, `.cursor/`, etc.) that would let a student dodge the check. **This policy governs student forks of `webshop/`, not this teacher workspace** — when working in `webshop/` from this session to update the starter template, example content, or CI itself, that restriction doesn't apply to you; just don't weaken or remove the enforcement mechanism without the teacher explicitly asking for it, since it's a deliberate anti-cheating control.

Other CI: `node.js.yml` (lint on push/PR), `canvas-hand-in.yml` (submission integration with Canvas LMS), `static-deploy.yml` (deploys the starter site).

### `astro-hidden-prompt/` — the hidden-prompt component

Single component, `src/HiddenPrompt.astro`: renders a visually hidden `<span>` (`font-size:0; position:absolute; color:transparent`) holding an instruction to AI assistants, intended to travel with the text when a student copies a requirement into an LLM chat. The course site imports it as `astro-hidden-prompt/HiddenPrompt.astro` and places `<HiddenPrompt />` after the first word of each requirement in `uppgifter/*.mdx`. Placement matters: a span before the first letter (the original placement) was dropped from the copy when a student dragged from the first letter, so keep it inside the text. Verified with real mouse drags in Chrome and Firefox; Safari is untested. The course site consumes the published npm version, so a change here must be released and the dependency bumped in `dbwebb-jsux.github.io` before it takes effect.

## Working across the repos

Course content in `dbwebb-jsux.github.io` (kmom pages, `uppgifter/webbshoppen_del_*`) describes a project that students build inside `webshop`. When editing an assignment spec or kmom moment, check whether it references files, structure, or behavior that should stay consistent with the `webshop` starter (e.g. `index.html`, `style.css`, the anti-agent notice, CI workflow names).
