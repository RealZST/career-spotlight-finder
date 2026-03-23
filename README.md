# Career Spotlight Finder

Discover hidden strengths and career narratives from your past projects.

## The Problem

- **"I don't know where my spotlights are"** — you've used methods and frameworks without knowing their industry names.
- **"I can't articulate my value"** — you sense you're experienced but struggle to weave it into a story.
- **"My work looks scattered"** — your projects seem unrelated but are actually connected by deeper themes.

## Usage

```
/career-spotlight-finder                    # auto-infer target domain
/career-spotlight-finder "machine learning" # specify target domain
```

## What It Does

A six-step pipeline that turns your past projects into career positioning copy:

1. **Init** — Sets up `~/.career-spotlight/` directory (first run) or detects existing analyses (incremental).
2. **Analyze** — Reads your projects, extracts hidden capabilities using a five-dimension framework.
3. **Position** — Infers 2-3 possible career directions, or uses the one you specified.
4. **Synthesize** — Clusters projects into theme lines, builds narrative arcs, identifies blind spots.
5. **Write copy** — Generates resume bullets, elevator pitch, LinkedIn summary, and casual intro.
6. **Review** — Presents findings, lets you iterate (add projects, change direction, regenerate).

## Where Output Is Stored

```
~/.career-spotlight/
├── analyses/    — individual project analysis files
├── report.md    — aggregated brand report
├── copies/      — ready-to-use copy (resume, pitch, LinkedIn, casual)
└── history/     — past versions (auto-archived)
```

All output is local only, not synced anywhere.

## Incremental Updates

Run the skill again with new projects. Only new projects are analyzed. The report and copies regenerate from all analyses (old + new).

## Adding Custom Term References

Add `industry-terms-[your-domain].md` to the `references/` directory inside this skill. Follow the format of existing reference files. It will be auto-detected during domain positioning.

## Supported Inputs (Phase 1)

Local files and directories — any text format (`.md`, `.txt`, `.tex`, `.py`, code files), PDF.

Future phases will add `.docx`, repo URLs, and web links.
