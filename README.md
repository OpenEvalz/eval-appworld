# eval-appworld

**AppWorld Benchmark (Interactive Coding Agents over App APIs)**

> ⚠️ **Third-party eval.** This is a `register/` pointer in inspect_evals — the task code lives in an external repository of unaudited provenance and will execute on OpenEvalz infrastructure. Onboarding it is a security review, not a packaging task.

**Paper:** https://arxiv.org/abs/2407.18901v1

AppWorld evaluates autonomous agents on 750 day-to-day digital tasks requiring iterative
Python code generation against 457 APIs across 9 simulated apps. Tasks are split into
test_normal (168) and test_challenge (417, includes unseen Amazon/Gmail APIs). Scoring uses
programmatic state-based unit tests checking database diffs for goal completion (Task Goal
Completion, TGC) and collateral damage avoidance, rather than reference-solution comparison.
Scenario Goal Completion (SGC) measures consistency across task variants.

## At a glance

| | |
|---|---|
| Upstream | [`register/appworld`](https://github.com/UKGovernmentBEIS/inspect_evals/tree/main/register/appworld) |
| Group | — |
| Total samples | 0 |
| Execution class | `plain` |
| Cost class | `low` |
| Flags | no sandbox, no network |
| Tags | — |

### Tasks

| Task | Samples |
|---|---|
| `appworld` | 0 |

### External assets

_None declared upstream._

## Running one problem

OpenEvalz is problem-level: the atomic unit is a single sample, not the whole eval.

```bash
inspect eval inspect_evals/appworld \
  --sample-id "<sample-id>" \
  --model openai-api/trustedrouter/<model> \
  --token-limit 200000
```

> **Two things that bite here, both verified in Inspect's source.**
>
> 1. **`--cost-limit` does not work on this routing path.** Inspect only records cost when its
>    pricing table resolves the model, and `_model_info.py` strips only `azure|bedrock|vertex`
>    prefixes — so `trustedrouter/<model>` never resolves and the cap silently never binds. The
>    real spend cap is enforced **server-side by TrustedRouter** via the delegated key's
>    `limit_microdollars` and spend window. Use `--token-limit` as the in-process bound.
> 2. **`--sample-id` matches with `fnmatch`.** A glob silently selects many samples and only warns.
>    Always pass a literal id.

## Reproducibility

`bundle.template.json` is the contract. A run that cannot emit a complete bundle does not publish.
Every image is pinned by `sha256` digest and every dataset by revision.

## Licensing

OpenEvalz wrapper code in this repository is **Business Source License 1.1** (see `LICENSE`) —
Licensor Lore Hex Corp, Change Date four years from publication, Change License Apache 2.0, no
Additional Use Grant. Same terms as TrustedRouter. Source-available, not open source: you may read,
modify and make non-production use of it, but production use needs a commercial licence
(licensing@openevalz.com).

**The packaged evaluation is NOT relicensed.** The task code, dataset and container images come from
upstream under their own terms — inspect_evals is MIT (UK AI Security Institute), and individual
datasets and images carry their own, sometimes unstated, licences. BSL covers only the OpenEvalz
packaging around them. See `NOTICE.md`, which must be completed before this repo publishes anything.
