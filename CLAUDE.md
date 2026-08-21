# CLAUDE.md

Project memory for the household-production / generative-AI research project.
Keep this file short. Put deeper material in `docs/` and link to it rather than pasting it here.

---

## 1. What this project is

An empirical research project asking: **what is the household-production potential of
generative AI, how much of that potential is realized, and how are the gains distributed
across households and household members?**

The organizing decomposition is **O → U → G**:

| Term | Name | Definition |
|---|---|---|
| $O_{ij}$ | Opportunity / exposure | $O_{ij} = B_{ij} \times S_j$ — baseline burden times researcher-coded technical suitability |
| $U_{ij}$ | Uptake | Whether household $i$ actually used AI for task $j$ |
| $G_{ij}$ | Realized gains | Vector of outcome margins, conditional on uptake |

Subscripts: $i$ = household, $j$ = task. Everything in the project is indexed this way.

Two components: (a) a **researcher-coded suitability instrument** $S_j$ built from an
ATUS-derived household task roster, and (b) a **household survey** measuring baseline time
use, adoption, post-AI time, felt burden, and market-substitute spending. The suitability
coding is the current active workstream; the survey is not yet fielded.

---

## 2. Notation — use these exactly

Do not silently rename, re-letter, or "simplify" any of these. They are load-bearing and
consistent across the proposal, the spec, and the code.

| Symbol | Meaning |
|---|---|
| $H_{ij}$ | Household service output (quantity or quality) |
| $T_{ij}$ | Time and effort input |
| $X_{ij}$ | Purchased goods and services (market substitutes) |
| $K_{ij}$ | Household capital |
| $A_{ij}$ | Labor-augmenting AI productivity |
| $S_j$ | Task-level technical suitability for AI (researcher-coded, continuous) |
| $B_{ij}$ | Baseline time / burden of the task |
| $\mu_{ij}$ | Felt burden — a **task-specific tax on time**, not a separate input |
| $c_{ij}$ | Adoption cost — a **time cost incurred only upon use**; indexed by $i$, not just $j$ |
| $\kappa_j$ | Satiation parameter (renamed from $\varepsilon_j$ to avoid clashing with the error term) |
| $\delta_j$ | Task fixed effects |
| $\delta^{\mu}$ | Burden relief — **distinct from** $\delta_j$; do not collapse them |

Gain vector:

$$G_{ij} = \begin{pmatrix} -\Delta T_{ij} \\ \Delta Q_{ij} \\ -\Delta M_{ij} \end{pmatrix}$$

— time saved, quality/completion change, administrative-burden change. Reported
**separately, never collapsed into an index**: the components are not commensurable and AI
may improve one while worsening another.

Production function:

$$H_{ij} = F_j(A_{ij} T_{ij},\; X_{ij},\; K_{ij})$$

**Labor-augmenting (GSY-style, Leontief with satiation) — not Hicks-neutral.** This is a
deliberate modeling choice, not a convenience. AI multiplies the effectiveness of an hour of
household time; it does not enter as a separate factor. If a change would make the
specification Hicks-neutral, flag it rather than making it.

---

## 3. Intellectual lineage

- **Becker (1965)** — time and goods jointly produce commodities the household wants.
  Justifies $\Delta Q_{ij}$ as a legitimate margin, not a residual.
- **Gronau (1977)** — diminishing returns to home time generate a shadow price, which
  governs the extensive-margin decision. Foundation for $U_{ij}$ depending on $O_{ij}$, and
  for letting $X_{ij}$ respond to AI.
- **Greenwood, Seshadri & Yorukoglu (2005), "Engines of Liberation"** — technology as
  labor-augmenting ($\zeta h$), with adoption as a separate lumpy decision. Direct template
  for the production function and for the potential/realized split.

These are used as **discipline, not as an estimated model.** The theory motivates which
controls to include, which tasks to survey, and why decomposition is necessary. It is not
itself estimated. Do not write text that implies the model is being structurally estimated.

---

## 4. Recurring errors to avoid

These have come up before. Check against them before drafting or coding.

1. **Handa et al. (2025) ≠ Eloundou et al. (2023).** The Anthropic Economic Index measures
   *realized usage*. Eloundou-style exposure measures *potential*. These are conceptually
   distinct and must never be conflated or cited interchangeably.
2. **GSY does include an adoption cost** (money price $q$). The project's claimed extension
   is that the *currency* of the cost changes from money to time — not that cost is newly
   introduced. Do not overclaim.
3. **Gain margins are parallel outcomes, not regressors.** Each margin is its own dependent
   variable in its own regression. Never put one on the right-hand side.
4. **Do not pre-answer empirical questions.** The proposal is designed to *test* whether
   gains accrue to the baseline task performer, whether uptake tracks suitability, and so on.
   Language that assumes the answer is a defect.
5. **The gender hypothesis is grounded**, not asserted. It follows from the
   planning-and-coordination composition of household labor, and should always be stated
   that way.
6. **Behavioral-trace data cannot substitute for the survey.** ATLAS (Google) and Blank,
   Schubert & Zhang measure usage among adopters. They cannot supply the non-adopter
   denominator, outcome/quality data, within-household distributional data, or
   household-demographic linkage. Anthropic conversation data is a *complementary
   revealed-preference instrument*, not a primary source.

---

## 5. Repo layout

```
.
├── CLAUDE.md                  # this file
├── README.md                  # public-facing pitch; renders on GitHub
├── proposal.pdf               # stable filename — do not rename
├── tex/                       # LaTeX source for the proposal
├── docs/
│   └── spec.md                # living model + empirical-strategy spec
├── data/
│   ├── raw/                   # gitignored; rebuilt by scripts/fetch_atus.py
│   └── derived/               # small CSVs, committed
├── coding/
│   ├── task_roster.csv        # ~24 household tasks
│   ├── rubric.md              # ported Eloundou rubric, continuous scoring
│   └── scores.csv             # coded S_j values
└── scripts/
```

Adjust as the project actually develops; update this section when you do.

---

## 6. Conventions

**Suitability coding.** The task roster is ATUS-derived, roughly 24 items, built with a
hybrid Tier 2 / Tier 3 grain rule. Scores are **continuous, not binary** — this is a
deliberate departure from the Eloundou rubric as published. Every coding decision needs a
written rationale in the same row; the diff history is part of the research output.

**Commit hygiene.** `main` is always presentable — it is a public repo linked from fellowship
materials. Do work-in-progress on branches. Commit messages in imperative mood, one logical
change each.

**Version control scope.** Commit: LaTeX source, `proposal.pdf`, all coding CSVs, scripts,
docs. Do not commit: LaTeX build artifacts, raw ATUS extracts, virtualenvs, anything with
respondent-identifying information.

**LaTeX.** The NSF-spec build is Times 11pt, 1-inch margins, references counted within the
page limit. Display-equation spacing is set globally after `\parskip` via
`\abovedisplayskip`, `\belowdisplayskip`, `\abovedisplayshortskip`, `\belowdisplayshortskip`.
Do not change these to fix a page-count problem without saying so — cut words instead.
Enforce page counts with `pdfinfo`; check content with `pdftotext`.

**Data.** All raw data must be re-derivable by a committed script. Nothing in `data/raw/`
should be irreplaceable.

---

## 7. Working with me

- **Prose should be tight.** Move explanatory and mathematical detail to supplements rather
  than padding the main text.
- **Address inline flags explicitly.** I use bracketed questions and flags while drafting.
  Answer each one directly before producing a revised version — do not silently resolve them.
- **Prefer factual framing over strategic framing.** State what is true; don't hedge toward
  what sounds advantageous.
- **Push back.** If a modeling or writing choice is wrong, say so and say why. Compressed
  explanations that skip the reasoning are worse than long ones.
- **Ask before large refactors** of the roster, rubric, or notation. These are settled objects.

---

## 8. Build and check

```bash
# compile the proposal
cd tex && pdflatex proposal.tex && pdflatex proposal.tex

# check page count
pdfinfo proposal.pdf | grep Pages

# dump text to check content and reference placement
pdftotext proposal.pdf - | less
```
