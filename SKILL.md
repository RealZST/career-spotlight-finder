---
name: career-spotlight-finder
description: Use when wanting to discover hidden strengths, industry buzzwords, and career narratives from past projects, articles, or code — for resumes, self-introductions, and personal branding
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - AskUserQuestion
argument-hint: "[target-domain]"
---

# Career Spotlight Finder

Discover hidden strengths and career narratives from your past projects. Solves three problems: "I don't know where my spotlights are", "I can't articulate my value", and "My work looks scattered."

**Pipeline overview:**

```
[Step 0] Init → [Step 1] Analyze → [Step 2] Position → [Step 3] Synthesize → [Step 4] Write copy → [Step 5] Review
```

---

## Step 0 — Initialization

**If `~/.career-spotlight/` does NOT exist:**

1. Tell the user: "This skill stores your career profile at `~/.career-spotlight/`. This includes per-project analyses, an aggregated report, and ready-to-use copy. The directory is local to your machine and not synced anywhere."
2. Wait for user confirmation before proceeding.
3. Create directories: `~/.career-spotlight/analyses`, `~/.career-spotlight/copies`, `~/.career-spotlight/history`.
4. Write a short README.md to `~/.career-spotlight/README.md` explaining the directory structure.
5. If directory creation fails, report the error, suggest checking permissions, and do NOT proceed.

**If `~/.career-spotlight/` DOES exist:**

1. Glob `~/.career-spotlight/analyses/*.md` and count the files.
2. Tell the user: "Found N existing project analyses. New projects will be analyzed incrementally."

**Target domain:**

- If `$ARGUMENTS` was provided, record it as the target domain for Step 2.
- If not provided, mark target domain as unset; it will be inferred in Step 2.

---

## Step 1 — Input Collection + Project Analysis

1. Ask the user for one or more project source paths (directories or files).
2. If the user provides no paths, prompt again — at least one path is required.
3. For each path: if it does not exist, warn the user, skip it, and continue with valid paths.
4. Glob each valid path to scan its contents. **Skip:** binary files, `node_modules/`, `.git/`, `__pycache__/`, `venv/`, `.env`. If a project has >50 files, warn the user and ask whether to proceed or narrow scope.
5. Check existing analyses in `~/.career-spotlight/analyses/` by matching the `source_path` field in their frontmatter. Only analyze projects that are new.
6. If ALL provided projects are already analyzed, offer the user a choice: re-analyze them or skip ahead to Steps 2-4.
7. For each new project, read `guides/project-analysis-guide.md` and follow its methodology.
8. Write each analysis to `~/.career-spotlight/analyses/[slugified-name].md` using `templates/project-analysis.md` as the template.

**Naming rules:**

- Directory path → slugify the directory name.
- Single file → slugify as `parentdir-filename`.
- On collision → append `-2`, `-3`, etc.

---

## Step 2 — Domain Positioning

**If target domain was set via `$ARGUMENTS`:**

1. Check for `references/industry-terms-[domain].md`. Load it if it exists.
2. Proceed directly to Step 3.

**If target domain is unset:**

1. Read `guides/domain-positioning-guide.md` and follow its methodology.
2. Present 2-3 candidate positioning directions to the user.
3. Wait for the user to select one.
4. Check for a matching `references/industry-terms-[domain].md` and load it if available.

---

## Step 3 — Narrative Synthesis

1. Read all analysis files from `~/.career-spotlight/analyses/`.
2. Read `guides/narrative-synthesis-guide.md` and follow its methodology.
3. If `~/.career-spotlight/report.md` already exists, archive it to `~/.career-spotlight/history/report-YYYY-MM-DDTHH-MM-SS.md` (use current timestamp).
4. Write the new synthesized report to `~/.career-spotlight/report.md` using `templates/aggregated-report.md` as the template.

---

## Step 4 — Copywriting

1. Read `~/.career-spotlight/report.md`.
2. Read `guides/copywriting-guide.md` and follow its methodology.
3. If any files exist in `~/.career-spotlight/copies/`, archive each to `~/.career-spotlight/history/` with a `YYYY-MM-DDTHH-MM-SS` timestamp prefix.
4. Write four files to `~/.career-spotlight/copies/` using `templates/copywriting-variants.md` as the template:
   - `resume-bullets.md`
   - `elevator-pitch.md`
   - `linkedin-summary.md`
   - `casual-intro.md`

---

## Step 5 — User Review

Present a summary to the user:

- One-sentence positioning statement
- Number of theme lines identified
- Top 3 hidden capabilities discovered
- Top blind spots flagged

Then ask:

> "Would you like to:
> 1. Add more projects (back to Step 1)
> 2. Change domain direction (back to Step 2)
> 3. Adjust narrative emphasis (back to Step 3)
> 4. Regenerate specific copy variants (back to Step 4)
> 5. Accept and finish"

- If the user picks 1-4, return to the corresponding step.
- If the user picks 5, confirm completion and remind them where their files are stored (`~/.career-spotlight/`).
