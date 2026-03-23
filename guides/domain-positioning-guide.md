# Domain Positioning Guide

This guide defines the methodology for Step 2 of the Career Spotlight Finder pipeline. Follow these instructions when SKILL.md directs you to "read `guides/domain-positioning-guide.md` and follow its methodology."

---

## 1. Purpose

Domain positioning answers the question: **"What kind of role should these projects point toward?"**

There are two scenarios:

- **No target domain specified** (the `$ARGUMENTS` field was empty): Infer 2-3 possible career positioning directions from the project analyses, then present them to the user for selection.
- **Target domain specified** (the user passed a domain via `$ARGUMENTS`): Validate the domain label, load any matching reference files, and proceed to Step 3 without inference.

In either case, the end result of this step is a confirmed domain direction and (optionally) a loaded industry-terms reference file to inform Steps 3 and 4.

---

## 2. Inference Process

Use this process when no target domain was provided. The goal is to derive positioning directions entirely from the evidence already captured in the project analyses.

### 2.1 Collect all evidence

1. Glob `~/.career-spotlight/analyses/*.md` and read every file.
2. From each analysis, extract two categories of evidence:
   - **Term mappings** -- every `-> **[industry term]**` entry found in the "Methods & Technology" and "Hidden Capabilities" sections.
   - **Transferable pattern tags** -- the `#tag` entries in the "Transferable Pattern Tags" section.
3. Build a combined list of all term mappings and tags across every project. Keep track of which project each item came from.

Example extraction from a single analysis:

```
Project: data-cruncher
  Term mappings:
    - "batch ETL pipeline"         (Methods & Technology)
    - "schema evolution"           (Methods & Technology)
    - "data quality monitoring"    (Hidden Capabilities)
  Tags:
    #pipeline-design  #automation  #data-quality
```

### 2.2 Identify clusters

4. Group the collected terms and tags by semantic similarity. Look for these signals:
   - **Frequency**: Which industry terms and tags appear in two or more projects?
   - **Co-occurrence**: Which terms tend to appear together in the same project?
   - **Thematic overlap**: Which projects share common problem domains or solution patterns?

5. Name each cluster by the professional domain it maps to. Use the following heuristics as a starting point (not an exhaustive list):

   | Dominant evidence pattern | Likely domain |
   |--------------------------|---------------|
   | Data pipeline terms, ETL, schema, orchestration, warehouse | Data Engineering |
   | Model training, evaluation, feature engineering, experiment tracking | ML Engineering |
   | Inference optimization, serving, model compression, latency | ML Infrastructure / MLOps |
   | UI components, state management, accessibility, responsive design | Frontend Engineering |
   | API design, system architecture, database, microservices | Backend / Full-stack Engineering |
   | User research, metrics, A/B testing, roadmap, prioritization | Product Management |
   | CI/CD, infrastructure as code, monitoring, SRE | DevOps / Platform Engineering |
   | Technical writing, documentation systems, developer experience | Developer Relations / DevEx |

   If the evidence does not cleanly map to any single domain, consider hybrid labels (e.g., "Full-stack Engineer with data focus" or "ML Engineer with strong backend skills").

### 2.3 Formulate positioning directions

6. Select the top 2-3 clusters (ranked by how many projects and distinct pieces of evidence support them). For each, formulate a **positioning direction** with three components:

   - **Label**: A concise role-oriented title that captures the direction. Be specific rather than generic. For example, prefer "ML Engineer focused on training efficiency" over just "ML Engineer."
   - **Supporting projects**: List the project names whose evidence feeds into this direction.
   - **Reasoning**: 1-2 sentences explaining why this direction makes sense given the evidence. Reference specific term mappings or tags.

Example output (internal, before presenting to user):

```
Direction 1:
  Label: "Data Engineer specializing in pipeline reliability"
  Supporting projects: data-cruncher, log-aggregator, etl-monitor
  Reasoning: Three out of four projects involve building or improving data
  pipelines. Recurring terms include "batch ETL," "schema evolution," and
  "data quality monitoring." Tags #pipeline-design and #automation appear
  across all three.

Direction 2:
  Label: "Backend Engineer with data infrastructure focus"
  Supporting projects: data-cruncher, api-gateway, log-aggregator
  Reasoning: Strong API design and systems architecture evidence in
  api-gateway, combined with data pipeline work. Tags #system-design and
  #api-architecture complement the data-oriented terms.

Direction 3:
  Label: "ML Engineer focused on production systems"
  Supporting projects: model-serving, data-cruncher
  Reasoning: model-serving shows inference optimization and serving
  infrastructure. Combined with data pipeline skills from data-cruncher,
  this points toward ML engineering roles emphasizing production readiness
  over research.
```

---

## 3. Presenting to User

Present the inferred directions to the user as a numbered list. Each option should include the label, supporting projects, and a brief rationale. Keep it concise -- the user should be able to scan and decide quickly.

Use this format:

```
Based on your project analyses, here are the positioning directions I see:

1. **[Label]**
   Projects: [project-a], [project-b], [project-c]
   [1-2 sentence reasoning]

2. **[Label]**
   Projects: [project-a], [project-d]
   [1-2 sentence reasoning]

3. **[Label]**
   Projects: [project-b], [project-d]
   [1-2 sentence reasoning]

4. **None of these / I have a different direction in mind**

Which direction best fits where you want to position yourself?
```

Rules for this interaction:

- Always include option 4 ("None of these / I have a different direction in mind") so the user is never forced into a suggestion.
- If the user picks option 4, ask them to describe their desired direction in a few words. Use that as the target domain.
- **Wait for the user to respond before proceeding.** Do not auto-select or skip ahead.
- If the user asks for clarification about a direction, explain in more detail which evidence supports it and what kinds of roles or companies it would resonate with.
- Once the user selects a direction (or provides their own), record it as the confirmed target domain for the rest of the pipeline.

---

## 4. Loading References

After the domain is confirmed (whether inferred and selected, user-provided, or passed via `$ARGUMENTS`), attempt to load a matching industry-terms reference file.

### 4.1 Find matching reference files

1. Glob `${CLAUDE_SKILL_DIR}/references/industry-terms-*.md` to list all available reference files. (`${CLAUDE_SKILL_DIR}` resolves to the skill's installation directory at runtime.)
2. The three built-in reference files are:
   - `industry-terms-ml.md` -- Machine Learning, ML Engineering, MLOps
   - `industry-terms-swe.md` -- Software Engineering, Backend, Frontend, Full-stack
   - `industry-terms-pm.md` -- Product Management, Program Management
3. Match the confirmed domain to the most relevant built-in file:
   - If the domain is ML-related (ML Engineer, Data Scientist, MLOps, AI Engineer, etc.) -> load `industry-terms-ml.md`
   - If the domain is engineering-related (Backend, Frontend, Full-stack, Data Engineering, DevOps, Platform, etc.) -> load `industry-terms-swe.md`
   - If the domain is product/program-related (Product Manager, TPM, etc.) -> load `industry-terms-pm.md`
   - If the domain spans multiple areas, load all relevant files.
4. If a matching file exists, read it. Its contents will be used in Steps 3 and 4 to ensure the narrative and copy use accurate, current industry terminology.
5. If no matching file exists (e.g., the domain is niche or the reference files have not been created yet), proceed using Claude's own knowledge of the domain. Note this to the user: "No specific industry-terms reference file found for [domain]. I'll use my general knowledge of [domain] terminology."

### 4.2 Check for user-added reference files

1. After loading built-in references, check if any additional `industry-terms-*.md` files exist that do NOT match the three built-in names (`ml`, `swe`, `pm`).
2. If user-added reference files are found, list them for the user:
   ```
   I also found these custom reference files:
   - industry-terms-devrel.md
   - industry-terms-data-science.md

   Would you like me to include any of these as well?
   ```
3. If the user says yes (to one or more), read those files and include their terminology alongside the built-in references.
4. If no user-added files are found, skip this sub-step silently.

### 4.3 What to do with loaded references

- Do not present the raw reference content to the user.
- Hold the loaded terminology in context for use during Step 3 (Narrative Synthesis) and Step 4 (Copywriting).
- The reference content helps ensure that the term mapping table, theme lines, and copy variants use terms that are recognized and valued in the target domain.
