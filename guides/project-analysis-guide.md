# Project Analysis Guide

## Overview

This guide teaches you how to extract hidden professional value from a single project by examining it through five dimensions: Problem & Motivation, Methods & Technology, Scale & Impact, Hidden Capabilities, and Transferable Patterns. Most people drastically undervalue their own work because they see only what they consciously chose to build, not the engineering practices, cross-cutting concerns, and meta-skills embedded in how they built it. Your job is to surface what users don't know they know -- to find the career-relevant signal buried in code structure, tooling choices, documentation habits, and architectural decisions that the user would never think to mention on a resume.

---

## Input Reading Strategy

Adapt your reading approach to the type of source material. The goal is to build a comprehensive mental model of the project with minimal file reads (target 15-20 files maximum for code repos).

### Code Repository

Read files in this order:

1. **README** (README.md, README.rst, README.txt) -- understand stated purpose, setup instructions, and any feature descriptions.
2. **Entry points** -- main.py, index.ts, index.js, app.py, main.go, Main.java, lib.rs, or whatever the language convention is. These reveal architecture and primary logic.
3. **Config files** -- package.json, pyproject.toml, Cargo.toml, go.mod, Makefile, docker-compose.yml, Dockerfile, .github/workflows/*.yml, tsconfig.json, webpack.config.js. These are gold mines for hidden capabilities.
4. **Directory structure** -- use Glob to scan the tree. Directory names reveal architecture (e.g., `src/middleware/`, `tests/integration/`, `locales/`, `k8s/`).
5. **Key source files** -- based on what you learned from steps 1-4, read 5-10 source files that seem most important (models, core logic, utilities, middleware).
6. **Test files** -- scan 1-2 test files. Testing patterns reveal quality engineering practices.

### Paper (LaTeX/PDF)

1. Read the abstract, introduction, methodology/methods, results, and conclusion sections.
2. For PDF files: if extraction quality appears low (garbled text, missing sections, broken formatting), inform the user and suggest providing the .tex source files instead.
3. For LaTeX: read the main .tex file first, then any \input or \include files for methodology and results.
4. Check for supplementary materials, appendices, or code repositories referenced in the paper.

### Document / Report

1. If the document is short (under ~200 lines), read it in full.
2. For longer documents, prioritize: executive summary, methodology/approach, results/outcomes, conclusion/recommendations.
3. Look for any appendices containing technical details, metrics, or tooling descriptions.

### Mixed Directory

1. Start with README and any top-level documentation files.
2. Read top-level config and metadata files.
3. Drill into subdirectories based on what looks most substantive.
4. If the directory contains both code and documents, treat the code portion as a code repo and the documents separately.

### File Skip List

Always skip these -- do not attempt to read them:

- **Directories:** `node_modules/`, `.git/`, `__pycache__/`, `venv/`, `.venv/`, `env/`, `.env/`, `dist/`, `build/`, `.next/`, `.cache/`, `target/` (Rust/Java), `vendor/` (Go/PHP)
- **Binary files by extension:** .png, .jpg, .jpeg, .gif, .svg, .ico, .bmp, .webp, .zip, .tar, .gz, .bz2, .7z, .rar, .exe, .bin, .so, .dylib, .dll, .o, .a, .lib, .pyc, .pyo, .class, .wasm, .pdf (unless the project IS a paper/document), .mp3, .mp4, .wav, .mov, .avi, .ttf, .woff, .woff2, .eot
- **Lock files:** package-lock.json, yarn.lock, pnpm-lock.yaml, Cargo.lock, poetry.lock, Pipfile.lock, composer.lock (note their existence for dependency management evidence, but do not read them)
- **Generated files:** .min.js, .min.css, .map files, .d.ts declaration files

Detect binary files by extension before attempting to read. If you accidentally read a binary file (garbled output), skip it and move on.

### Version Compatibility

When you encounter an existing analysis file in `~/.career-spotlight/analyses/`:

1. Check the `analysis_version` field in its frontmatter.
2. The current analysis version is **1**.
3. If the existing file has an `analysis_version` older than 1 (or the field is missing), inform the user: "This project was analyzed with an older version of the methodology. Would you like to re-analyze it for more comprehensive results?"
4. If the user declines re-analysis, use the existing file as-is for downstream steps.

---

## Five-Dimension Extraction

For each project, extract findings along all five dimensions. This is where the real value lives -- you must go beyond what the user would self-report.

### Dimension 1: Problem & Motivation

**What to look for:**
- README problem statements, "About" sections, or opening paragraphs
- Paper abstracts and introduction sections
- Code comments that explain "why" (not "what") -- especially at the top of files or above complex logic
- Git commit messages (if accessible) that reference issues or motivations
- Issue trackers or TODO files that reveal what problems were being addressed
- The gap between "what existed before" and "what this project provides"

**Example extractions:**
- A CLI tool that wraps complex API calls -> "Reduced developer onboarding friction by abstracting complex API interactions into a streamlined command-line interface"
- A data processing script that cleans CSVs -> "Built a data quality assurance pipeline to ensure downstream analytics reliability"
- A personal portfolio site -> "Designed and shipped a user-facing web application with responsive layout and content management"

**What users commonly miss:**
- **Implicit problems:** If the codebase introduced tests where there were none, that's a quality engineering motivation. If it added logging, that's observability. If it added input validation, that's security hardening.
- **Organizational problems:** A tool built for a team implies coordination, stakeholder management, or developer experience concerns.
- **Research motivation:** Even "I was curious" translates to "self-directed research initiative" in the right context.
- **Migration motivations:** Switching from one framework/library to another implies technical evaluation and migration planning.

### Dimension 2: Methods & Technology

**What to look for:**
- Import statements and dependency lists (package.json dependencies, requirements.txt, pyproject.toml, Cargo.toml, go.mod)
- Config files that reveal tooling (webpack, babel, eslint, prettier, mypy, ruff)
- Infrastructure files (Dockerfile, docker-compose.yml, kubernetes manifests, terraform files, serverless.yml)
- CI/CD configs (.github/workflows/, .gitlab-ci.yml, Jenkinsfile, .circleci/)
- Build system files (Makefile, CMakeLists.txt, build.gradle, pom.xml)
- Paper methodology sections describing algorithms, frameworks, or experimental setups
- Database schemas, migration files, ORM configurations

**Example extractions:**
- Uses pandas + numpy -> "Data engineering with Python scientific computing stack"
- Has a Dockerfile -> "Application containerization with Docker"
- Has GitHub Actions workflow -> "CI/CD pipeline design and automation"
- Uses React + TypeScript -> "Type-safe frontend development with React"
- Has Terraform files -> "Infrastructure as Code (IaC) with Terraform"
- Uses SQLAlchemy with Alembic -> "Database schema design with migration management"

**What users commonly miss (meta-methods):**
- **Makefile / task runners** -> build automation, developer experience engineering
- **Docker + docker-compose** -> containerization, local development environment design
- **Linter/formatter configs** (.eslintrc, .prettierrc, ruff.toml, mypy.ini) -> code quality standards, static analysis
- **Pre-commit hooks** (.pre-commit-config.yaml) -> automated code quality gates
- **Multiple package managers or build tools** -> polyglot engineering
- **Type annotations / TypeScript** -> type-safe programming, API contract enforcement
- **Environment variable patterns** (.env.example, config classes) -> configuration management, twelve-factor app methodology
- **Monorepo tooling** (lerna, nx, turborepo, workspaces) -> monorepo architecture and management

### Dimension 3: Scale & Impact

**What to look for:**
- Data sizes referenced in code, configs, or documentation (row counts, file sizes, batch sizes, rate limits)
- User counts, download metrics, or traffic figures mentioned in README or docs
- Team references ("contributors" section, CODEOWNERS file, PR templates)
- Metrics in papers (dataset sizes, accuracy numbers, performance benchmarks)
- Number of endpoints in an API, number of pages/routes in a web app
- Test coverage numbers, number of test files
- Deployment targets (multiple environments, regions, platforms)
- Version history (many releases implies sustained maintenance)

**Example extractions:**
- Config has batch_size=10000 and references S3 buckets -> "Processed large-scale datasets in cloud storage"
- README mentions "used by 3 teams internally" -> "Cross-team internal tooling adoption"
- Paper reports 95% accuracy on 50k sample dataset -> "Achieved 95% model accuracy on 50,000-sample evaluation dataset"
- 15 API endpoints with rate limiting -> "Designed and maintained a multi-endpoint REST API with rate limiting"

**What users commonly miss (implicit scale):**
- **Multiple config environments** (dev, staging, prod configs) -> multi-environment deployment management
- **Multiple language/locale files** (i18n/, locales/) -> internationalization (i18n) supporting global users
- **Database migrations** (many migration files) -> long-lived system with evolving data models
- **Multiple microservices in one repo** -> distributed systems architecture
- **Pagination logic** -> handling large datasets, API design for scale
- **Caching layers** (Redis config, memoization patterns) -> performance optimization at scale
- **Queue/worker patterns** (Celery, Bull, Sidekiq) -> asynchronous processing, event-driven architecture
- **Comprehensive test suites** (many test files) -> quality assurance at scale, test engineering

### Dimension 4: Hidden Capabilities

**This is the core value of the entire analysis.** Hidden capabilities are the cross-cutting engineering practices and skills embedded in how the project was built, which the user would almost never list on a resume because they consider them "just part of doing the work."

**What to look for:**
Examine the project's structure, configuration, and code patterns for evidence of skills that cut across the primary purpose of the project.

**Example extractions with evidence chains:**

| Evidence in Project | Hidden Capability | Industry Term |
|---|---|---|
| Try/except blocks with specific exception types, custom error classes, retry logic | Defensive programming, graceful degradation | **Reliability engineering** |
| Structured logging (JSON logs, log levels, correlation IDs) | Operational visibility design | **Observability engineering** |
| Multiple locale/translation files | Multi-language support | **Internationalization (i18n)** |
| Extensive docstrings, inline comments, API documentation | Knowledge transfer and maintainability | **Technical documentation** |
| PR templates, CODEOWNERS, review checklists | Team quality processes | **Code review leadership** |
| CI/CD pipeline configs with test, lint, deploy stages | Automated delivery pipeline | **DevOps / CI/CD engineering** |
| Dockerfile + K8s manifests + Helm charts | Container orchestration | **Infrastructure engineering** |
| Input validation, sanitization, CORS config, auth middleware | Security-conscious development | **Application security** |
| Database indexes, query optimization, connection pooling | Performance-aware data access | **Database performance engineering** |
| Feature flags, A/B test configs | Controlled rollout strategies | **Release engineering** |
| Accessibility attributes (aria-*, semantic HTML, alt text) | Inclusive design | **Accessibility (a11y) engineering** |
| API versioning, deprecation warnings, changelog | Interface lifecycle management | **API governance** |
| Monitoring configs (Prometheus, Datadog, Sentry) | Production health tracking | **Site reliability engineering (SRE)** |
| README with setup instructions, contributing guide | Onboarding documentation | **Developer experience (DX) engineering** |
| Data validation schemas (JSON Schema, Pydantic, Zod) | Contract enforcement | **Data contract design** |

**What users commonly miss:**
- They built error handling but would never say "reliability engineering"
- They added CI/CD but would never say "DevOps practices"
- They wrote extensive docs but would never say "technical communication"
- They structured their code cleanly but would never say "software architecture"
- They handled edge cases but would never say "defensive programming"
- They managed configs across environments but would never say "configuration management"

Look hard at this dimension. Read between the lines of the code. The most valuable findings are almost always here.

### Dimension 5: Transferable Pattern Tags

Generate 2-5 tags per project that capture reusable, cross-project patterns. These tags will be used in Step 3 (Narrative Synthesis) to identify theme lines across multiple projects.

**Tag format:** `#lowercase-hyphenated`

**Examples of good tags:**
- `#data-pipeline` -- project involved moving/transforming data from one form to another
- `#developer-tooling` -- project built tools for other developers
- `#api-design` -- project designed or consumed APIs as a core activity
- `#ml-pipeline` -- project involved machine learning model training/deployment
- `#full-stack` -- project spanned frontend and backend
- `#automation` -- project automated a previously manual process
- `#technical-writing` -- project produced significant documentation
- `#system-integration` -- project connected multiple systems or services
- `#data-visualization` -- project presented data visually
- `#infrastructure` -- project involved deployment, hosting, or operations

**Critical: Tag reuse protocol**

Before creating new tags for a project:

1. Glob `~/.career-spotlight/analyses/*.md` to find all existing analysis files.
2. Grep for `## Transferable Pattern Tags` sections across those files.
3. Collect all existing tags.
4. For each new tag you want to create, check whether a semantically equivalent tag already exists:
   - Use `#data-pipeline` not `#data-pipelines` if the singular form already exists.
   - Use `#api-design` not `#api-development` if the former already exists and covers the same concept.
   - Use `#ml-pipeline` not `#machine-learning-pipeline` if the shorter form already exists.
5. Only create a genuinely new tag when no existing tag captures the pattern.

This consistency is essential for Step 3, where tags are used to find cross-project themes.

---

## Term Translation

Translate what the user actually did into industry-recognized terminology. This bridges the gap between "I wrote a script" and "automated data pipeline engineering."

**Process:**

1. Use your knowledge of industry terminology as the primary source. For each method, tool, or practice found in the project, identify the standard industry term.
2. If a domain-specific reference file has been loaded (from `references/industry-terms-[domain].md`), cross-check your translations against it for domain-specific precision.
3. Format each translation as: `[what user did] -> **[industry term]**`
4. When you are uncertain about the most precise term, provide the most likely industry term and add a brief qualifying note (e.g., "likely falls under **platform engineering**, depending on organizational context").
5. This is best-effort at this stage. Term translations will be refined in Step 3 (Narrative Synthesis) after domain positioning is complete and domain reference files are loaded.

**Translation examples:**

| What the User Did | Industry Term |
|---|---|
| "Wrote a Python script to clean data" | **ETL / data pipeline engineering** |
| "Set up Docker for the project" | **Application containerization** |
| "Made the app work on phones too" | **Responsive web design** |
| "Fixed a bug where two users edited the same thing" | **Concurrency control / race condition resolution** |
| "Added login with Google" | **OAuth 2.0 integration / federated identity management** |
| "Made the page load faster" | **Frontend performance optimization** |
| "Wrote tests for the API" | **API test automation** |
| "Moved the database to a new server" | **Database migration / infrastructure migration** |

---

## Output

### File Creation

After completing the five-dimension extraction for a project, immediately write the analysis to disk. Do not hold multiple analyses in context -- write each one as it is completed.

**File path:** `~/.career-spotlight/analyses/[slugified-name].md`

Refer to the naming rules in SKILL.md Step 1 for slugification:
- Directory path -> slugify the directory name
- Single file -> slugify as `parentdir-filename`
- On collision -> append `-2`, `-3`, etc.

### Template

Use `templates/project-analysis.md` as the output format. Every analysis file must include:

**Frontmatter (required fields):**
```yaml
---
analysis_version: 1
source_path: /absolute/path/to/project
source_type: code-repo | paper | document | mixed
analyzed_date: YYYY-MM-DD
---
```

**Body sections (all required):**
- `## Problem & Motivation` -- 1-2 sentences
- `## Methods & Technology` -- bulleted list with term translations
- `## Scale & Impact` -- bulleted list with quantified or qualified scope
- `## Hidden Capabilities` -- bulleted list with evidence chains and term translations
- `## Transferable Pattern Tags` -- 2-5 tags on a single line

### Context Management

Write each analysis file to disk immediately after completing it. This ensures that:
1. Work is saved even if the session is interrupted.
2. Subsequent analyses can reference earlier ones (especially for tag reuse).
3. The user can review individual analyses as they are produced.
