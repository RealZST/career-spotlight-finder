---
name: career-spotlight-finder
version: 1.0.0
description: Use when wanting to discover hidden strengths, industry buzzwords, and career narratives from past projects, articles, or code — for resumes, self-introductions, and personal branding
metadata:
  openclaw:
    requires:
      anyBins: [pandoc]
    homepage: https://github.com/RealZST/career-spotlight-finder
allowed-tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - WebFetch
  - AskUserQuestion
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

1. Verify subdirectories `analyses/`, `copies/`, `history/` exist. Recreate any that are missing.
2. Glob `~/.career-spotlight/analyses/*.md` and count the files.
3. Tell the user: "Found N existing project analyses. New projects will be analyzed incrementally."

## Step 1 — Input Collection + Project Analysis

1. Ask the user for one or more project sources. Accepted input types:
   - **Local path** — directory or file (e.g., `~/projects/my-app`, `~/papers/paper.pdf`)
   - **URL** — web page or online article (e.g., `https://example.com/blog/my-post`). Fetched with the WebFetch tool.
   - **docx file** — Word document (e.g., `~/docs/report.docx`). Converted to text via `pandoc` (preferred) or `python3 -c "import docx; ..."` as fallback.
2. If the user provides no sources, prompt again — at least one source is required.
3. Validate each source:
   - **Local path:** resolve to an **absolute canonical path** (expand `~`, resolve `..`, follow symlinks). If the resolved path does not exist, warn the user and skip it.
   - **URL:** verify the URL scheme is `http://` or `https://`. Fetch the page with WebFetch. If the fetch fails (network error, 404, etc.), warn the user and skip it.
   - **docx file:** resolve the path, confirm it exists and ends with `.docx`. Convert to markdown/text: first try `pandoc -f docx -t markdown "$path"` via Bash; if pandoc is not installed, fall back to `python3 -c "import sys; from docx import Document; d=Document(sys.argv[1]); print('\n'.join(p.text for p in d.paragraphs))" "$path"`. If both fail, warn the user (suggest `brew install pandoc` or `pip install python-docx`) and skip it.
   - If ALL sources are invalid, tell the user and return to step 1 (re-prompt for sources).
4. **Directory expansion** — for each validated local directory, scan its top-level contents to determine whether it is:
   - **(a) A single project** (e.g., a code repo with `README.md`, `src/`, `package.json`) — keep as one project.
   - **(b) A collection of independent documents** (e.g., a folder of PDFs, `.tex` files, `.docx` files, or papers with no shared code structure) — expand into individual sources, one per document.

   **Detection heuristic:** If the directory has no code-project markers (`README.md`, `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Makefile`, `src/`, `lib/`, `.git/`) AND contains 2+ document files (`.pdf`, `.tex`, `.docx`, `.md`, `.txt`) at the top level or in flat subdirectories, treat it as a collection.

   When a collection is detected, tell the user: "This directory looks like a collection of N independent documents rather than a single project. I'll analyze each one separately." The user can override this if they want a combined analysis.
5. **After validating and expanding sources, ask the user to set project priorities for the final source list:**
   - Which projects are **highlights** (重点) — these will receive the most narrative weight and prominence in all outputs.
   - Which projects are **supporting** (次要) — these add breadth but are not the user's main story.
   - Do NOT assume the most recent project is the most important. The user decides.
   - Record each project's priority as `highlight` or `supporting` — this is stored in the analysis frontmatter and used throughout Steps 3-4.
6. If more than 8 projects are queued for analysis, gently recommend focusing on 5-8 projects that best represent the target direction. Explain that narrative quality is highest in this range — too many projects can dilute themes and stretch context. Suggest setting less central projects to `supporting` priority, or running the skill again later with a different subset. Do NOT enforce a hard limit; proceed if the user wants to continue with all projects.
7. For each valid source, gather content:
   - **Local directory:** Glob to scan contents. **Skip:** binary files, `node_modules/`, `.git/`, `__pycache__/`, `venv/`, `.env`. If a project has >50 files, warn the user and ask whether to proceed or narrow scope.
   - **Local file (non-docx):** Read the file directly.
   - **docx file:** Use the converted text from step 3.
   - **URL:** Use the fetched content from step 3.
8. Check existing analyses in `~/.career-spotlight/analyses/` by matching the `source_path` (or `source_url`) field in their frontmatter against the resolved sources. For each match:
   - **Version check:** if the existing analysis has `analysis_version` older than the current version (1) or the field is missing, inform the user and mark it for re-analysis.
   - **Staleness check (local sources):** detect whether the source has changed since last analysis:
     - **Git repo** (directory contains `.git/`): compare `source_fingerprint` in the frontmatter against the current latest commit hash (`git -C "$path" log -1 --format=%H`). If the hash differs, the repo has changed.
     - **Single file**: compare `source_fingerprint` against the file's current mtime epoch (`stat -f %m "$path"` on macOS or `stat -c %Y "$path"` on Linux).
     - **Non-git directory**: compare `source_fingerprint` against the most recent file mtime in the tree (`find "$path" -type f -not -path '*/.git/*' -exec stat -f %m {} + | sort -rn | head -1`).
     - If the fingerprint differs, inform the user: "Source has been modified since last analysis." and mark it for re-analysis.
   - **Staleness check (URL sources):** compare `analyzed_date` in the frontmatter against today's date. If the analysis is older than 7 days, inform the user: "This URL was last analyzed on [date]. The page may have changed." and offer re-analysis. The user can skip if they know the content hasn't changed.
   - **Priority update:** compare the priority the user set in step 5 against the `user_priority` in the existing frontmatter; if they differ, update the frontmatter field (metadata-only, no re-analysis needed).
   - Only skip full analysis for projects whose version and fingerprint are all current (and URL analyses are within 7 days).
9. If ALL provided projects are already analyzed and up-to-date, offer the user a choice: re-analyze them or skip ahead to Steps 2-4.
10. For each new project, read `guides/project-analysis-guide.md` and follow its methodology. The guide adapts analysis lens based on input type (research paper vs code repo vs document) and sub-field.
11. Write each analysis to `~/.career-spotlight/analyses/[slugified-name].md` using `templates/project-analysis.md` as the template.

**Naming rules:**

- Directory path → slugify the directory name.
- Single file → slugify as `parentdir-filename`.
- URL → slugify the page title or last path segment (e.g., `https://example.com/blog/my-post` → `my-post`).
- On collision → append `-2`, `-3`, etc.

---

## Step 2 — Domain Positioning

1. Read `guides/domain-positioning-guide.md` and follow its full methodology (Sections 2-4).
2. Infer 2-3 candidate positioning directions, then recommend one framing to lead with. This may be a conventional `Primary Expert Framing` or a `Bridge Expert Framing` when adjacent directions are all strongly supported and the combination is itself the user's advantage.
3. Present the recommendation first, including a short `Distinctiveness Thesis` that explains why the user stands out. Keep other directions as `Alternative Wrappers`, not competing identities.
4. Ask the user to confirm the recommended framing before proceeding. If they want a different wrapper or target direction, use that as the confirmed framing. Do NOT force a narrow job-title label when a bridge framing better matches the evidence, but do require an explicit confirmation or override before moving to Step 3.
5. Load references per Section 4 of the guide.

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
3. If any files exist in `~/.career-spotlight/copies/`, archive each to `~/.career-spotlight/history/` with a timestamp suffix: `[original-name]-YYYY-MM-DDTHH-MM-SS.md` (e.g., `resume-bullets-2026-03-24T10-30-00.md`).
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

Then ask:

> "Would you like to:
> 1. Add more projects (back to Step 1)
> 2. Change domain direction (back to Step 2)
> 3. Adjust narrative emphasis (back to Step 3)
> 4. Regenerate specific copy variants (back to Step 4)
> 5. Accept and finish"

- If the user picks 1-4, return to the corresponding step.
- If the user picks 5, confirm completion and remind them where their files are stored (`~/.career-spotlight/`).
