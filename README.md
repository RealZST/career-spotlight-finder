# Career Spotlight Finder

Discover hidden strengths, market-facing positioning, and a coherent career narrative from your past projects.

## The Problem

- **"I don't know where my spotlights are"** — you've used methods and frameworks without knowing their industry names.
- **"I can't articulate my value"** — you sense you're experienced but struggle to turn your work into a clear story.
- **"My work looks scattered"** — your projects seem unrelated on the surface but are often connected by deeper patterns.
- **"I fit several directions, but none cleanly"** — your real value may live in the combination, not a single narrow job-title label.

## Usage

Mention the skill by name in a normal prompt and describe what you want analyzed.

Examples:

- `Use the career-spotlight-finder skill to analyze these projects and help me find my career positioning.`
- `Use career-spotlight-finder on these papers and repos, then generate resume bullets and a LinkedIn summary.`

You can also invoke it directly in different coding agents:

- **Codex**: `$career-spotlight-finder`
- **Claude Code**: `/career-spotlight-finder`

## What It Does

A six-step pipeline that turns projects, papers, and docs into career positioning outputs:

1. **Init** — Sets up `~/.career-spotlight/` on first run and reuses it incrementally after that.
2. **Analyze** — Reads each project with a five-dimension framework tailored to repos, papers, or documents.
3. **Position** — Infers 2–3 plausible directions, recommends one expert-facing center, and keeps adjacent directions as alternative wrappers.
4. **Synthesize** — Finds the strongest thread across projects, builds narrative arcs, and writes a distinctiveness thesis.
5. **Write copy** — Generates resume bullets, elevator pitch, LinkedIn summary, and casual intro from the same underlying story.
6. **Review** — Lets the user add projects, change direction, adjust emphasis, or regenerate outputs.

## Current Strengths

- **Recommendation-first positioning** — the skill recommends a credible expert center instead of handing the identity problem back to the user.
- **Bridge-profile support** — when the user's edge comes from adjacent strengths together, it can use a bridge framing instead of forcing a narrow label.
- **Distinctiveness thesis** — it explains not just what bucket the user fits into, but why this person is more memorable than a standard candidate in that area.
- **Priority-aware storytelling** — users can mark projects as `highlight` or `supporting`, and those priorities shape synthesis and copywriting.
- **Thread discovery across different project types** — it can connect papers, repos, and side products into one believable career story when the connective logic is real.
- **Incremental reuse** — existing analyses are preserved, and project priority changes can be reflected without full re-analysis.

## Positioning Philosophy

- The skill's job is **not** to ask the user to solve their own classification problem.
- It should recommend a credible expert entry point, then explain what makes the user special inside that frame.
- When the user's story spans adjacent areas, the main line should stay clear without erasing the second strength that gives the profile its edge.

## Where Output Is Stored

```text
~/.career-spotlight/
├── analyses/    # per-project analyses
├── report.md    # aggregated career brand report
├── copies/      # ready-to-use copy variants
└── history/     # archived reports and prior copy
```

All output is local only, not synced anywhere.

## Incremental Updates

Run the skill again with new projects. Only new projects are analyzed. The report and copies regenerate from the full analysis set, and existing project priorities can be updated without full re-analysis.

## Adding Custom Term References

Add `industry-terms-[your-domain].md` to the `references/` directory inside this skill. Follow the format of existing reference files. It will be auto-detected during domain positioning and can be loaded alongside bundled references for hybrid directions.

## Supported Inputs

- **Code repositories** — README, structure, key implementation files, selected configs, and important docs
- **Research papers** — PDF / LaTeX-style paper analysis with research-trajectory extraction
- **Documents** — design docs, reports, specs, and other narrative materials
- **Local files and directories** — common text/code formats plus PDF

## Main Outputs

- **Per-project analyses** with term mappings, hidden capabilities, and transferable tags
- **One aggregated report** with:
  - one-sentence positioning
  - expert center
  - distinctiveness thesis
  - theme lines or thread chapters
  - cross-theme capabilities
- **Four copy variants**:
  - `resume-bullets.md`
  - `elevator-pitch.md`
  - `linkedin-summary.md`
  - `casual-intro.md`
