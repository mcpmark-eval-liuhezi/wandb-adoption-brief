# W&B Adoption Brief

**Purpose:** source-backed answers to the three questions leadership asked
before we commit to W&B Weave for LLM observability, for discussion at the
architecture review.

**Method:** We first tried W&B's own support bot with each question. It was
unreachable, so we fell back to public primary sources (PyPI, W&B's official
docs, and the `wandb/wandb` GitHub repository).

---

## Research note: W&B support bot status

**Status: UNREACHABLE.** The W&B support bot did not answer any of our three
questions. Every query returned the same connection failure:

> Error connecting to support bot: 404 Client Error: Not Found for url:
> https://morg--wandbot-api-wandbotapi-serve.modal.run/status

We tried all three questions separately (one bot query per question) and got
the same 404 each time. The bot returned no answer text and no sources for any
question, so it contributed nothing to the findings below. All answers
therefore come from public primary sources instead.

---

## Q1. What is the latest published version of the `wandb` Python package, per its PyPI metadata?

**Answer: `wandb` 0.31.0** (latest version published on the Python Package
Index at the time of writing).

**Primary source:** the project page for the `wandb` package on PyPI —
https://pypi.org/project/wandb/ — which is Python's package index and the
authoritative public metadata for the distribution ("A CLI and library for
interacting with the Weights & Biases API", MIT-licensed, Python 3.10–3.15).

**Cross-check:** the `v0.31.0` release of the `wandb/wandb` client repository
on GitHub (see Q3) publishes the same version number, so the package index and
the source repository **agree** — there is no discrepancy to reconcile.

**Support bot:** unreachable for this question (see research note above).

---

## Q2. Per W&B's official documentation, what is Weave for, and how does that differ from the experiment-tracking side of W&B (runs, sweeps, artifacts)?

**Answer:** Per W&B's official documentation, **W&B Weave is an observability
and evaluation platform for building reliable agents and LLM applications.**
W&B's own words: "W&B Weave is an observability and evaluation platform that
helps you track, evaluate, and improve your agents and LLM applications."
Weave's documented capabilities are:

- **Visibility** into agent sessions, multi-turn conversations, and individual
  function calls (Ops/Calls), including inputs, outputs, cost, token count and
  latency.
- **Systematic evaluation** of responses against curated test cases, using LLM
  judges and custom scorers.
- **Version tracking** for prompts, models and data.
- **Experimentation** comparing prompts and models.
- **Feedback collection** for human judgments and annotations.
- **Monitoring** in production with guardrails and scorers.

**How it differs from the experiment-tracking side of W&B:** the two address
different problems.

| | Experiment tracking (W&B core) | Weave |
|---|---|---|
| **Primary source** | Runs / Sweeps / Artifacts docs | Weave docs |
| **What it is for** | Training and fine-tuning ML models: tracking, visualizing and comparing experiments | Tracing, evaluating and improving agents and LLM applications in development and production |
| **Core unit** | The **run** — "a single unit of computation logged by Weights & Biases," e.g. a model-training process that logs metrics such as accuracy and loss | The **trace/call** — a record of an LLM call or function call, with inputs, outputs, cost, token counts and latency |
| **Hyperparameters** | **Sweeps** — "automate hyperparameter search and visualize rich, interactive experiment tracking" (Bayesian, grid, random search) | Not sweeps; instead prompt/model experimentation and comparison |
| **Data/model assets** | **Artifacts** — "track and version data as the inputs and outputs of your W&B Runs," e.g. datasets in, trained model checkpoints out | Versioned prompts, datasets and model configurations, oriented to LLM workflows |
| **SDK** | `wandb` Python package (`wandb.init()`, `run.log()`) | Separate `weave` Python/TypeScript libraries (`pip install weave`) |

W&B's docs themselves draw this line in their "Get Started" guidance:
training or fine-tuning ML models → experiment tracking, hyperparameter
optimization, artifacts and visualizations; tracing and evaluating existing
models and applications → tracing, prompt management, evaluation and cost
tracking for production AI apps. In W&B's own framing, "Building LLM
applications is fundamentally different from traditional software
development": LLM outputs are non-deterministic, quality is subjective and
context-dependent, and small prompt changes can cause unexpected behavior
changes — the problems Weave is built to address, rather than the
metric-logging problem runs and sweeps solve.

**Primary sources (all W&B official documentation):**
- Weave landing page — https://docs.wandb.ai/weave/ (W&B's official docs for
  Weave; pages are currently served from the CoreWeave docs platform, which
  acquired W&B).
- "What is Weave?" — https://docs.wandb.ai/weave/concepts/what-is-weave
- Runs overview — https://docs.wandb.ai/models/runs
- Sweeps overview — https://docs.wandb.ai/models/sweeps
- Artifacts overview — https://docs.wandb.ai/models/artifacts
- "Get Started with Weights & Biases" — https://docs.wandb.ai/models/

**Source agreement:** no factual disagreement between these pages was found —
they consistently describe Weave as the LLM/GenAI observability + evaluation
side and runs/sweeps/artifacts as the ML experiment-tracking side. One
**material nuance for our adoption decision** (not a contradiction): the Weave
docs state that Weave's agent-tracing workflows are "also available in
CoreWeave Agent Lens, the successor to Weights & Biases Weave," and that both
services already receive agent tracing data with nothing to migrate. Also
consistent with this, the `wandb` v0.31.0 release notes state the default API
endpoint is now CoreWeave Forge. Since W&B is now owned by CoreWeave, we
should confirm with W&B which offering we'd be committing to before signing.

**Support bot:** unreachable for this question (see research note above).

---

## Q3. What is the most recent release of the open-source `wandb/wandb` client repository on GitHub?

**Answer: `v0.31.0`** is the most recent release of the open-source
`wandb/wandb` client repository on GitHub (tag `v0.31.0`, release name
`v0.31.0`, cut from the `release-0.31.0` branch).

Notable items from the official release notes, relevant to us as W&B
customers: the default API endpoint is now CoreWeave Forge
(`https://api.forge.coreweave.com`) with run links pointing to
`https://forge.coreweave.com/wandb` (existing `api.wandb.ai` keys keep
working); Python 3.15 support was added; and a new `wandb sweep-scheduler`
command can run a sweep's search locally with Optuna or Ax.

**Primary source:** the `wandb/wandb` repository's releases on GitHub —
https://github.com/wandb/wandb/releases (latest release `v0.31.0`,
https://github.com/wandb/wandb/releases/tag/v0.31.0). This is the project's
own open-source client repository, MIT-licensed.

**Cross-check:** PyPI's published `wandb` package is also `0.31.0` (see Q1),
so the GitHub repository and PyPI **agree**.

**Support bot:** unreachable for this question (see research note above).

---

## Summary

1. Latest `wandb` Python package version per PyPI metadata: **0.31.0**.
2. Weave is W&B's **observability and evaluation platform for LLM applications
   and agents** (tracing, evaluation, prompt/dataset versioning, feedback,
   production monitoring), which is distinct from the experiment-tracking side
   of W&B, where **runs** record training computations, **sweeps** automate
   hyperparameter search, and **artifacts** version datasets and model
   checkpoints.
3. Most recent release of `wandb/wandb` on GitHub: **v0.31.0**.

No two sources consulted disagreed on any fact; the one nuance worth a
decision is the CoreWeave Agent Lens / Forge transition noted in Q2 and Q3.

## All sources consulted

1. W&B support bot (`wandb_query_wandb_support_bot`) — queried once per
   question (3 queries); **unreachable** each time (404 error, see research
   note).
2. PyPI — `wandb` project page: https://pypi.org/project/wandb/
3. PyPI — `wandb` JSON API: https://pypi.org/pypi/wandb/json
4. W&B docs — Weave landing page: https://docs.wandb.ai/weave/
5. W&B docs — What is Weave?: https://docs.wandb.ai/weave/concepts/what-is-weave
6. W&B docs — Get Started with Weights & Biases: https://docs.wandb.ai/models/
7. W&B docs — Runs overview: https://docs.wandb.ai/models/runs
8. W&B docs — Sweeps overview: https://docs.wandb.ai/models/sweeps
9. W&B docs — Artifacts overview: https://docs.wandb.ai/models/artifacts
10. GitHub — `wandb/wandb` releases: https://github.com/wandb/wandb/releases
    (latest release https://github.com/wandb/wandb/releases/tag/v0.31.0)
