# wandb-adoption-brief

This repository holds our team's **Weights & Biases (W&B) adoption brief**.

We are standardizing on W&B for ML experiment tracking and are evaluating the
**W&B Weave** product for LLM observability ahead of our architecture review.
The brief collects the factual questions leadership asked us to source before
we commit, plus the primary sources behind each answer:

1. The latest published version of the `wandb` Python package (per PyPI).
2. What Weave is for, and how it differs from the experiment-tracking side of
   W&B (runs, sweeps, artifacts).
3. The most recent release of the open-source `wandb/wandb` client repository
   on GitHub.

The brief itself lives in [`BRIEF.md`](BRIEF.md) (added via pull request).
Please read it and leave comments on the pull request so we can fold feedback
into the architecture review.

> **Note on scope:** W&B (and its Weave product) is now owned by CoreWeave,
> and parts of the official documentation have moved to the CoreWeave domain.
> The brief cites W&B's own docs pages at their current canonical URLs.

---

For reference, the auto-generated file list of this repository:
