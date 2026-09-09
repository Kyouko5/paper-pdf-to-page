---
name: paper-pdf-to-page
description: Create one detailed, evidence-grounded paper page from one IEEE Transactions or comparable technical-journal PDF, adapting the analysis to the paper's domain and contribution type while preserving the PDF in place.
---

# Paper PDF to Page

Use this skill when the user provides or points to exactly one IEEE Transactions or comparable technical-journal paper PDF and wants a structured, writing-ready reading output. It applies across communications, computer science, control, signal processing, AI/ML, energy, biomedical engineering, manufacturing, devices, measurement, and other technical fields.

## Scope

Produce exactly one Markdown paper page for the input PDF. The page must be self-contained and include the paper's problem framing, contribution-specific technical detail, evidence, and writing-useful content. Adapt the technical emphasis to the paper:

- `method` or `algorithm`: model, architecture, objective, training or optimization, inference, complexity, and baselines;
- `theory`: definitions, assumptions, propositions or theorems, guarantees, and proof strategy;
- `system` or `device`: architecture, interfaces, implementation, operating conditions, and measured behavior;
- `empirical`, `measurement`, `benchmark`, or `dataset`: protocol, data, metrics, baselines, statistics, and artifacts;
- `application`: domain problem, deployment context, method, and validation;
- `survey` or `review`: scope, taxonomy, comparison axes, synthesis, and limitations.

Do not assume the paper has a communication channel, task model, energy model, optimization problem, algorithm, hardware prototype, or empirical experiment. When a field does not apply, write `不适用（原因）`; when it should apply but the paper does not report it, write `未说明` or `证据不完整`. Do not create system-model pages, concept pages, topic pages, comparison pages, synthesis pages, experiment-asset pages, indexes, logs, or other companion files unless the user separately asks for them.

Treat the paper page as the single-paper reading and writing entry point, not as a short abstract archive. Keep the source PDF unchanged. Do not move, copy, rename, delete, annotate, or convert it in place. Temporary extraction files may be written only to a system temporary directory and should be removed after use.

## Before Reading

1. Resolve the input to one existing `.pdf` file. If the request names a folder containing several PDFs without selecting one, ask which PDF to process.
2. Inspect the current workspace for `schema.md`. If present, read it and apply only its generic paper-page, evidence, source-protection, and metadata rules. Ignore domain-specific examples or fields that do not fit the paper.
3. Choose the output path. Prefer `wiki/<paper-title>.md` when the workspace has a `wiki/` directory. Otherwise use `<workspace>/<paper-title>.md`, unless the user specifies another path.
4. Derive the page title from the PDF's formal title, correcting line-break and hyphenation artifacts. Use a filesystem-safe filename while keeping the full title as the Markdown H1.
5. Classify the article type and research paradigm before extracting details: for example `method`, `theory`, `system`, `empirical`, `benchmark`, `dataset`, `application`, `survey`, `position`, or `unknown`.

When the input is a folder and the user explicitly requests ingestion of that folder, enumerate the PDFs and process them as separate one-paper outputs. The input folder is an evidence source only: do not normalize it, archive it, or create companion Markdown, image, or intermediate files there.

## Reading and Evidence

Read the PDF's text, metadata, figures/captions, tables, equations, proofs, appendices, and references as needed. Use an available PDF parser or renderer; do not rely on the filename alone. For important architecture diagrams, plots, experimental tables, or hard-to-extract equations, render pages for visual inspection.

Separate three kinds of statements:

- **Paper reports**: directly supported by the PDF; include section, page, figure, table, or equation locators for important claims when practical.
- **Structured interpretation**: a concise explanation derived from the paper's equations, workflow, architecture, protocol, or proof.
- **Missing evidence**: write `未说明` or `证据不完整`; never infer data, hardware, code availability, deployment, or generality from vague claims.

Record the absolute source PDF path in frontmatter. Do not replace it with a fabricated citation key or a converted Markdown path.

## Required Page Shape

Use YAML frontmatter followed by these sections, in this order:

1. `# <正式论文标题>`
2. `## 单行摘要`
3. `## 题目驱动研究框架`
4. `## Algorithm Design 快照` (method-oriented; use the paper's actual contribution)
5. `## 图1系统框架草案` (system, method, process, taxonomy, or proof-dependency flow)
6. `## System Model` (also covers problem formulation when no system model exists)
7. `## Algorithm Design 详解` (method, algorithm, or theory details as applicable)
8. `## 实验证据卡片`
9. `## Introduction 写作素材`
10. `## Related Work 写作素材`

The `题目驱动研究框架` treats the title as an initial hypothesis. Identify the title's object, task or problem, method family, and claimed effect, then check whether these are actually the paper's main axis. Explicitly flag any mismatch between the title and the substantive paper.

The `Algorithm Design 快照` is one Chinese paragraph of no more than 400 Chinese characters. It is a method-oriented heading, not an instruction to invent an algorithm. State the setting, research object, problem or gap, contribution or method family, core mechanism, and intended effect. For a theory, survey, device, measurement, or benchmark paper, describe its actual contribution.

The `图1系统框架草案` must identify entities or components, boundaries and interfaces, inputs and outputs, data/signal/control flow, state or decision variables, module relationships, feedback or evaluation paths, and the source of assumptions or boundary conditions. It may describe a method pipeline, process flow, taxonomy, or proof-dependency flow rather than a physical system.

The `System Model` section should cover, as applicable, the research object and boundary, entities or components, inputs/observations/outputs, states/actions/parameters, data or signals, objective/loss/utility/evaluation metrics, assumptions, uncertainty or measurement model, constraints and boundary conditions, and any optimization, statistical, causal, or formal problem. It also serves as `Problem Formulation` for papers without an explicit system model. Theory papers should state theorem conditions; ML or data papers should state representation, splits, and train/inference tasks; system papers should state architecture, interfaces, resources, and operating conditions. Do not force a system model where the paper has none.

The `Algorithm Design 详解` section should explain whichever technical path the paper actually uses: model, algorithm, network, protocol, architecture, hardware process, training/inference or solver steps, objective/loss/reward, constraint handling, complexity and resource cost, theorem statements and proof strategy, convergence/correctness/stability/generalization claims, ablations, and baseline role where reported. Preserve pseudocode, update equations, or proof dependencies when useful. For papers without an algorithm, explain the method, protocol, architecture, measurement procedure, benchmark, or proof chain instead. Do not fabricate a solver, reward, proof, or decomposition.

The `实验证据卡片` must explicitly answer, when applicable:

- validation type: theory/proof, analytical, simulation, synthetic-data experiment, public-dataset or benchmark, trace-driven or offline replay, emulation, laboratory experiment, prototype, hardware-in-the-loop, user study, clinical study, field test, deployment, literature review, or another precise type;
- data origin, dataset name, sample size, split, preprocessing, and whether data are public, proprietary, self-collected, real-world measurement, synthetic, borrowed, mixed, or unavailable;
- platform, software, framework, simulator, protocol, instrument, laboratory, clinical, industrial, or deployment setting;
- hardware, compute, device, sensor, fabrication, network, operating condition, and domain configuration;
- evaluation metrics, units, baselines, fairness of comparisons, ablations, sensitivity analyses, statistical significance, confidence intervals, or uncertainty when reported;
- artifact availability: code, data, configuration, model, measurement setup, or unknown;
- reproducibility judgment and the specific missing pieces;
- key reported results with dataset or setting, metric and unit, baseline, and uncertainty when available;
- for theory papers, theorem or proposition assumptions, conclusion, and proof or verification path.

Every evidence field is conditional. Use `不适用（原因）` when a field does not belong to the paper, and `未说明` or `证据不完整` when it should apply but the paper does not provide enough information. Distinguish real-data-driven evaluation from real deployment, simulation from emulation, and a formal guarantee from an empirical observation.

The two writing-material sections should extract reusable background, importance, concrete gap, research question, contribution, evidence boundary, and differentiating comparison axes. Do not turn them into generic praise or an abstract-only summary.

## Reading Workflow

Follow this order when building the page:

1. Establish the title-driven research framework and identify the paper's object, problem, method family, claim, and effect.
2. Classify the contribution type and research paradigm.
3. Write the `Algorithm Design 快照` before drafting the Introduction material.
4. Extract the entities, flows, variables, and assumptions needed for the `图1系统框架草案`.
5. Complete `System Model` and the detailed method, algorithm, or theory explanation from the paper's equations, procedure, architecture, protocol, or proofs.
6. Extract structured theory, experiment, and reproducibility evidence.
7. Derive tags and comparison material, then write the Introduction and Related Work sections.

Do not make a claim stronger than its source evidence. A trace-driven experiment is real-data-driven but still not real deployment. If a paper only says that “simulation results show” an improvement without naming the platform, data, or configuration, mark the evidence as `证据不完整`. Treat a theorem, proof, prototype, clinical result, or benchmark claim as evidence only at the strength supported by its assumptions and protocol.

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
literature_type: unknown
article_type: unknown
evidence_tier: supporting
paper_role: supporting
use_for:
  - methodology
validation_type:
  - unknown
data_origin:
  - unknown
platforms: []
frameworks: []
datasets: []
hardware_stack: []
experimental_stack: []
artifact_availability: unknown
reproducibility_level: unknown
# Optional fields; include when useful and supported:
# problem_domain: []
# research_problem: []
# method_family: []
# contribution_types: []
# evaluation_metrics: []
# baselines: []
# theoretical_results: []
# limitations: []
# authors: []
# year: unknown
# doi: unknown
---
```

Apply explicit user metadata first, then metadata stated in the paper, then conservative defaults. Do not infer CCF, JCR, or other venue rankings from the fact that a venue is an IEEE Transactions journal. Record the actual journal name in `venue`; use `Unknown` for `venue_tier` unless a tier is explicitly supplied. Keep `sources` stable if later metadata is refined.

For an existing paper page, use this precedence when refining metadata: explicit user instruction, existing page frontmatter, the defaults in this skill or workspace schema, and only then conservative inference from the PDF. `venue_tier`, `evidence_tier`, and `paper_role` are separate fields and must not be used as substitutes for one another.

## Completion Check

Before finishing, verify that:

- exactly one paper-page file was created or updated;
- the source PDF still exists at the original path and was not modified;
- all required sections are present and adapted to the article type;
- claims with missing evidence are marked as such, while inapplicable fields include a reason;
- no cross-paper or companion pages were created;
- the output filename and H1 use the same paper title;
- the page is readable without needing temporary extraction files;
- validation type, data origin, artifact availability, reproducibility level, and key metrics or theoretical evidence are present when applicable;
- any `prototype`, `field_test`, or deployment claim has a concrete configuration, and any public dataset claim names the dataset;
- key numerical results include their metric and experimental or theoretical conditions;
- important claims have PDF locators when practical;
- no UAV-, communication-, optimization-, or hardware-specific assumption was added unless supported by the paper.
