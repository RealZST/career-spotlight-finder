# Narrative Synthesis Guide (Step 3)

This guide is the methodology for Step 3 of the Career Spotlight Finder pipeline. Follow each section in order.

---

## 1. Purpose

The goal of narrative synthesis is to connect scattered projects into a coherent career story. Most people's work looks disjointed on the surface — a data pipeline here, a research paper there, a quick automation tool somewhere else — but underneath there are recurring themes that reveal who the person really is as a professional.

This step solves the problem: **"My work looks scattered but is actually connected by deeper themes."**

By the end of this step you will produce:

- **Theme lines** — 2-5 clusters of related projects that form coherent professional narratives.
- **Narrative arcs** — for each theme line, a story from origin to peak that shows growth.
- **A positioning statement** — one sentence capturing the user's core professional value.
- **Cross-theme capabilities** — differentiators that span the user's entire career.
- **Blind spots** — gaps the user should prepare for.
- **A consolidated term mapping table** — standardized industry terminology across all projects.

The output is written to `~/.career-spotlight/report.md` using the `templates/aggregated-report.md` format.

---

## 2. Clustering (Step 3a) — Group Projects into Theme Lines

### Procedure

1. Read all files in `~/.career-spotlight/analyses/*.md`.
2. From each analysis file, extract the `## Transferable Pattern Tags` section. Collect every tag (e.g., `#data-pipeline`, `#automation`, `#model-optimization`).
3. Build a master tag list across all projects. Note which projects contributed each tag.
4. Group tags into **2-5 semantic clusters**. Each cluster becomes a **theme line**. Use semantic similarity, not exact string matching — tags like `#ETL`, `#data-pipeline`, and `#data-cleaning` belong together even though their labels differ.
5. Assign each theme line a professional label — a clear, domain-appropriate name a hiring manager would recognize.
6. List which projects belong to each theme line. A project may appear in more than one theme line if it contributed tags to multiple clusters.

### How to Cluster

- Start by listing all unique tags and their project sources.
- Look for tags that co-occur in the same projects — co-occurrence suggests they belong to the same theme.
- Look for tags that share a domain even if they never co-occur — e.g., `#REST-API` and `#GraphQL` both relate to API design.
- Aim for clusters that are distinct enough to tell different stories but broad enough to contain 2+ projects each.
- If a tag does not fit any cluster, hold it — it may become a cross-theme capability (Section 6).

### Example

```
All tags collected:
  #data-pipeline (Projects 1, 3, 7)
  #ETL (Projects 1, 7)
  #automation (Projects 3, 7, 9)
  #model-pruning (Projects 2, 5)
  #quantization (Project 5)
  #latency-optimization (Projects 2, 5, 8)
  #mentoring (Projects 4, 6)
  #technical-writing (Projects 4, 10)
  #cross-team-coordination (Projects 3, 6)

Cluster A: #data-pipeline, #ETL, #automation
  -> Projects 1, 3, 7, 9
  -> Label: "Data Engineering & Automation"

Cluster B: #model-pruning, #quantization, #latency-optimization
  -> Projects 2, 5, 8
  -> Label: "Model Efficiency Optimization"

Cluster C: #mentoring, #technical-writing, #cross-team-coordination
  -> Projects 4, 6, 10 (and partially 3)
  -> Label: "Technical Leadership & Communication"
```

### Edge Cases

- **Only 1-2 projects analyzed:** Create 1-2 theme lines. Note that additional projects would strengthen the narrative and suggest the user add more (they can return to Step 1).
- **All projects cluster into one theme:** That is fine — the user may be deeply specialized. Create one strong main line and look for a supporting line from secondary tags (e.g., soft skills, tooling choices).
- **More than 5 clusters emerge:** Merge the most similar clusters until you have at most 5. Prioritize clusters with the most projects or the strongest relevance to the target domain.

---

## 3. Ranking (Step 3b) — Prioritize Theme Lines for the Target Domain

### Procedure

Given the target domain (set in Step 2 or via `$ARGUMENTS`), rank the theme lines by relevance:

| Rank | Role | Report Weight | Criteria |
|------|------|---------------|----------|
| **Main line** | Core value proposition | ~50% | Most directly relevant to the target domain. This is the "headline" of the user's career story. |
| **Supporting line** | Differentiation | ~30% | Relevant but distinct from the main line. This is what makes the user stand out from other candidates with similar main-line skills. |
| **Supplementary line** | Depth / soft skills | ~20% | Cross-cutting capabilities, soft skills, or adjacent expertise. Adds dimension. |

If there are more than 3 theme lines, the extras become supplementary (share the 20% weight) or are folded into the cross-theme capabilities section.

If there are only 2 theme lines, assign one as main and one as supporting. Omit supplementary.

If there is only 1 theme line, it is the main line. Look for sub-themes within it to create a supporting angle.

### Narrative Structure Decision

After ranking, determine whether the theme lines form a **progression**:

- **Temporal progression:** The user moved from Theme A early in their career to Theme B later, then Theme C most recently. Check the `analyzed_date` and project chronology for clues.
- **Logical progression:** Theme A is foundational knowledge that enabled Theme B, which enabled Theme C (e.g., data engineering -> ML infrastructure -> ML product development).

**If a progression exists**, use a **progressive narrative**:

> "I started by building X, which gave me deep expertise in Y. That led me to Z, where I now focus on [main line]."

**If no clear progression exists**, use a **parallel narrative**:

> "My work spans three complementary areas: [main], [supporting], and [supplementary]. Together they make me uniquely effective at [positioning statement]."

Record which narrative structure you are using — it will shape the report and copywriting.

---

## 4. Narrative Arcs (Step 3c) — Build the Story for Each Theme Line

For each theme line, construct a four-part narrative arc using the projects assigned to that theme.

### 4.1 Origin

Identify the **earliest project** in this theme line. Answer:

- What foundational skill or insight was established here?
- What was the user's starting point in this domain?

Example: "In Project 1 (a university data scraper), the user first encountered the challenge of transforming unstructured web data into clean, structured datasets."

### 4.2 Growth

Identify **subsequent projects** that show the user deepening capability or expanding scope. Answer:

- How did the complexity increase from the origin?
- Did the user move from individual contribution to team-level or system-level work?
- Did the scale grow (more data, more users, higher stakes)?

Example: "Projects 3 and 7 show progression from single-source scrapers to multi-source ETL pipelines handling production data at scale, introducing monitoring and fault tolerance."

### 4.3 Peak

Identify the **most impressive project** in this theme line. This is determined by:

- Hardest technical challenge solved
- Largest scale (data volume, user count, team size)
- Most notable result (quantified impact, recognition, business outcome)
- Most recent is often but not always the peak

Example: "Project 7 (real-time data pipeline processing 10TB/day) represents the peak — it combined the data transformation skills from earlier projects with real-time streaming, cross-team coordination, and a measurable 60% reduction in query latency."

### 4.4 Positioning

Write **one sentence** that captures what this theme line says about who the user is as a professional.

Guidelines:
- Start with a professional identity, not a task description.
- Reference the arc from origin to peak.
- Use target-domain terminology.

Example: "A data engineer who has progressed from ad-hoc data collection to designing production-grade, real-time data infrastructure at scale."

### Handling Thin Arcs

If a theme line has only 1-2 projects, the arc will be compressed:

- 1 project: Origin and Peak are the same. Growth is implied by the complexity within the single project. State this honestly — do not fabricate progression.
- 2 projects: Origin is the earlier one, Peak is the later/more impressive one. Growth is the delta between them.

---

## 5. Term Refinement (Step 3d) — Standardize Industry Terminology

Now that you have the full picture across all projects and the domain reference file is loaded (from Step 2), revisit and consolidate terminology.

### Procedure

1. Collect all `## Methods & Technology` and `## Hidden Capabilities` entries from every analysis file. Each entry has the form `[what user did] -> [industry term]`.
2. If a domain reference file (`references/industry-terms-[domain].md`) was loaded, cross-check all mapped terms against it.
3. Identify and fix:
   - **Inconsistencies:** The same capability mapped to different terms across projects. Pick the best term and standardize.
   - **Missed translations:** Colloquial descriptions that were not mapped to an industry term during individual analysis. Map them now with the full domain context.
   - **Over-generic terms:** Terms like "programming" or "data analysis" that could be more specific (e.g., "distributed systems programming", "exploratory data analysis").
   - **Under-translations:** Cases where the user did something impressive but the term does not convey the full weight (e.g., "wrote a script" when the industry term should be "automated CI/CD pipeline").
4. Build a **consolidated term mapping table** for the report:

```
| What you did                        | What the industry calls it         | Source project    |
|-------------------------------------|------------------------------------|-------------------|
| Wrote cron jobs to move data        | ETL pipeline orchestration         | Project 1, 7      |
| Made the model smaller and faster   | Model compression / quantization   | Project 5         |
| Helped new team members get started | Technical onboarding & mentorship  | Project 4, 6      |
```

### Guidelines

- Prefer terms that the target domain actually uses. A term that is technically correct but unused in the target industry is not helpful.
- When in doubt, err toward terms that are slightly more impressive but still defensible — the user will review everything in Step 5.
- Deduplicate: if three projects each list "Python scripting," consolidate into one row that references all three projects rather than three separate rows.

---

## 6. Cross-Theme Capabilities

After building the theme lines, look for capabilities that appear in **2 or more theme lines**. These are not confined to one narrative — they are woven throughout the user's career.

### How to Identify

- Review the `## Hidden Capabilities` sections across all analyses.
- Look for capabilities that transcend a single theme. Common examples:
  - **Systems thinking** — the user designs end-to-end systems, not just components, across multiple projects.
  - **From-zero-to-one delivery** — the user repeatedly builds things from scratch rather than maintaining existing systems.
  - **Debugging under uncertainty** — the user consistently solves problems without clear specifications.
  - **Cross-functional communication** — the user works across team boundaries in multiple contexts.
  - **Performance mindset** — the user optimizes for speed, cost, or efficiency regardless of the domain.

### Why These Matter

Cross-theme capabilities are powerful differentiators because they are hard to teach and hard to screen for in interviews. They signal adaptability and depth. Highlight them prominently in the report.

### Format

List each cross-theme capability with:
- A clear label
- Evidence from at least 2 projects (cite specific projects)
- One sentence explaining why this capability is valuable in the target domain

Example:
```
**Systems Thinking**
Evidenced in: Project 3 (end-to-end ETL pipeline), Project 5 (full model training-to-deployment optimization), Project 6 (org-wide documentation system)
Value: In [target domain], this translates to the ability to own entire feature lifecycles rather than just individual tasks.
```

---

## 7. Blind Spots

Based on the target domain and the user's inferred seniority level, identify the top **3-5 capabilities** that are commonly expected but **not evidenced** in any of the analyzed projects.

### How to Identify

1. Consider what the target domain typically requires at the user's seniority level. Seniority is inferred from the complexity, scope, and leadership signals in the analyzed projects.
2. Compare those expectations against the full set of capabilities evidenced across all analyses.
3. The gap = blind spots.

### What to Include for Each Blind Spot

For each identified blind spot, provide:

1. **Capability name** — the specific skill or experience area.
2. **Why it matters** — a concrete explanation of why this capability is expected in the target domain at the inferred seniority. Not generic; tied to the specific domain.
3. **Possible explanations** — the user may actually have this capability but it was not evidenced in the analyzed projects. Acknowledge this possibility.

### Example

```
1. **Production Monitoring & Observability**
   Why it matters: [Target domain] roles at the senior level are expected to own
   service reliability. Monitoring, alerting, and incident response are table stakes.
   Note: This may exist in your experience but was not evidenced in the analyzed
   projects. Consider adding projects that demonstrate this capability.

2. **Stakeholder Communication / Business Translation**
   Why it matters: At the senior level in [target domain], translating technical
   decisions into business impact for non-technical stakeholders is critical for
   project approval and resource allocation.
   Note: Your projects show strong technical depth but limited evidence of
   business-facing communication.
```

### Guidelines

- Be honest but not discouraging. Frame blind spots as areas to address, not deficiencies.
- Always include the note that the gap may be due to project selection, not actual missing capability.
- Limit to 3-5. More than 5 blind spots becomes overwhelming and unhelpful.

---

## 8. Output — Write the Report

### Pre-Write Check

Before writing, check if `~/.career-spotlight/report.md` already exists.

- **If it exists:** Archive it to `~/.career-spotlight/history/report-YYYY-MM-DDTHH-MM-SS.md` using the current timestamp. Then proceed to write the new report.
- **If it does not exist:** Proceed directly.

### Write the Report

Write `~/.career-spotlight/report.md` using the format defined in `templates/aggregated-report.md`. The report must contain:

1. **Meta section** — date, target domain, project count, theme line count.
2. **One-Sentence Positioning** — synthesized from the theme line positioning statements. This is the single most important sentence in the entire report. It should:
   - Name the user's professional identity (not a job title, but what they do).
   - Reference the main theme line.
   - Include a differentiator from the supporting theme line.
   - Be specific enough that it could not describe just anyone in the field.
3. **Term Mapping Table** — the consolidated table from Step 3d.
4. **Theme Lines** — each with its narrative arc (origin, growth, peak, positioning) and key projects. Main line first, then supporting, then supplementary.
5. **Cross-Theme Capabilities** — from Section 6.
6. **Blind Spots** — from Section 7.

### Quality Checklist

Before finalizing the report, verify:

- [ ] Every analyzed project appears in at least one theme line.
- [ ] The positioning statement is specific and defensible (every claim is backed by a project).
- [ ] Term mappings are consistent — the same capability is not called different things in different theme lines.
- [ ] Narrative arcs show genuine progression, not invented progression. If growth is limited, say so honestly.
- [ ] Blind spots reference the specific target domain, not generic career advice.
- [ ] The report uses target-domain terminology throughout, not the user's original colloquial descriptions.

---

## Summary of Sub-Steps

| Sub-Step | Action | Output |
|----------|--------|--------|
| 3a | Cluster tags into theme lines | 2-5 named theme lines with project assignments |
| 3b | Rank theme lines by target-domain relevance | Main / Supporting / Supplementary ranking + narrative structure |
| 3c | Build narrative arcs | Origin -> Growth -> Peak -> Positioning for each theme line |
| 3d | Refine and consolidate terminology | Consolidated term mapping table |
| 3e | Identify cross-theme capabilities | List of career-spanning differentiators |
| 3f | Identify blind spots | 3-5 expected-but-missing capabilities |
| 3g | Write report | `~/.career-spotlight/report.md` |
