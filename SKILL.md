---
name: paper-pdf-to-page
description: Create one detailed paper page from one research-paper PDF, preserving the PDF in place and avoiding cross-paper pages or batch knowledge-base expansion.
---

# Paper PDF to Page

Use this skill when the user provides or points to exactly one research-paper PDF and wants a structured paper-reading output.

## Scope

Produce exactly one Markdown paper page for the input PDF. The page must be self-contained and include the paper's modeling, algorithm, evidence, and writing-useful content. Do not create system-model pages, concept pages, topic pages, comparison pages, synthesis pages, experiment-asset pages, indexes, logs, or other companion files unless the user separately asks for them.

Keep the source PDF unchanged. Do not move, copy, rename, delete, annotate, or convert it in place. Temporary extraction files may be written only to a system temporary directory and should be removed after use.

## Before Reading

1. Resolve the input to one existing `.pdf` file. If the request names a folder containing several PDFs without selecting one, ask which PDF to process.
2. Inspect the current workspace for `schema.md`. If present, read it and apply its paper-page requirements, while retaining this skill's one-page scope.
3. Choose the output path. Prefer `wiki/<paper-title>.md` when the workspace has a `wiki/` directory. Otherwise use `<workspace>/<paper-title>.md`, unless the user specifies another path.
4. Derive the page title from the PDF's formal title, correcting line-break and hyphenation artifacts. Use a filesystem-safe filename while keeping the full title as the Markdown H1.

## Reading and Evidence

Read the PDF's text, metadata, figures/captions, tables, equations, and references as needed. Use an available PDF parser or renderer; do not rely on the filename alone. For important system diagrams or experimental tables, render pages for visual inspection when text extraction is ambiguous.

Separate three kinds of statements:

- **Paper reports**: directly supported by the PDF.
- **Structured interpretation**: a concise explanation derived from the paper's equations or workflow.
- **Missing evidence**: write `未说明` or `证据不完整`; never infer hardware, datasets, code availability, or real-world validation from generic claims such as “simulation results”.

Record the absolute source PDF path in frontmatter. Do not replace it with a fabricated citation key or a converted Markdown path.

## Required Page Shape

Use YAML frontmatter followed by these sections, in this order:

1. `# <正式论文标题>`
2. `## 单行摘要`
3. `## 题目驱动研究框架`
4. `## Algorithm Design 快照`
5. `## 图1系统框架草案`
6. `## System Model`
7. `## Algorithm Design 详解`
8. `## 实验证据卡片`
9. `## Introduction 写作素材`
10. `## Related Work 写作素材`

The `Algorithm Design 快照` is one Chinese paragraph of no more than 400 Chinese characters. It must state the scenario, research object, problem, concrete method, and intended effect.

The `图1系统框架草案` must identify system entities, task/data flow, control or optimization variables, and the origin of constraints so the user can draw a first system figure from the description.

The `System Model` section should cover entities, variables, objective, channel/task/energy/sensing assumptions, constraints, and optimization formulation at the level supported by the paper. Preserve important notation where it improves reuse.

The `Algorithm Design 详解` section should explain the actual solver or learning pipeline, subproblem decomposition, objective/reward design, constraint handling, convergence or complexity claims, and baseline role where reported.

The `实验证据卡片` must explicitly answer:

- validation type: theory, simulation, trace-driven, emulation, prototype, or field test;
- data origin and named datasets, if any;
- platform, software, frameworks, and simulator;
- hardware, compute, UAV/GBS/radar configuration;
- artifact availability: code, data, configuration, or unknown;
- reproducibility judgment and the specific missing pieces;
- key reported results with the condition or baseline attached.

The two writing-material sections should extract reusable motivation, research gap, contribution, and differentiating comparison points. Do not turn them into generic praise or a paper summary.

## Frontmatter

Use this shape and fill fields from the paper or mark unknown values explicitly:

```yaml
---
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:
  - /absolute/path/to/input.pdf
venue: Unknown
venue_tier: Unknown
literature_type: method
evidence_tier: supporting
paper_role: supporting
use_for:
  - methodology
validation_type:
  - simulation
data_origin:
  - unknown
platforms: []
frameworks: []
datasets: []
hardware_stack: []
artifact_availability: unknown
reproducibility_level: unknown
---
```

Apply explicit user metadata first, then metadata stated in the paper, then conservative defaults. Do not invent venue rankings. Keep `sources` stable if later metadata is refined.

## Completion Check

Before finishing, verify that:

- exactly one paper-page file was created or updated;
- the source PDF still exists at the original path and was not modified;
- all required sections are present;
- claims with missing evidence are marked as such;
- no cross-paper or companion pages were created;
- the output filename and H1 use the same paper title;
- the page is readable without needing the temporary extraction files.
