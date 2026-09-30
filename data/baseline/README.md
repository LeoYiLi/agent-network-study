# Baseline experiment records

The raw records behind Study 1, the relay-hop check and the attribute studies
of the AAMAS 2027 manuscript. They were run between 1 and 3 September 2026 and
lived only in ignored `runs/` directories until now. The later Knobs and
Payables experiments are archived separately under `data/raw/` (PR #2).

The payload is **4,004 files / 36.2 MB**, archived per run as 5.1 MB of
`.tar.gz`. These are the original file bytes. Tar ownership, modes and
timestamps are normalized and gzip timestamps are zero, as in `data/raw/`;
`manifest.json` records when each run's summaries were written, its models,
configurations, scenarios, seeds, file counts and archive SHA-256.

| Archive | Episodes | What it is | Used in the manuscript for |
|---|---:|---|---|
| `axis2x2.tar.gz` | 72 | A–D at E = 0.3 and 0.7, plain directory cards | Study 1 |
| `realistic.tar.gz` | 72 | A–D at E = 0.3 and 0.7, vetted and relational cards | Study 1 |
| `matched-E0.tar.gz` | 27 | public, private and hybrid at E = 0, before the access axis existed | the E = 0 check (public and private only) |
| `matched-E0-glm.tar.gz` | 18 | the same on `z-ai/glm-4.6` | the second-model pilot in Limitations |
| `scaling.tar.gz` | 112 | A and C at E = 0.7 with and without a relay hop, at edge cost 0, 1 and 2 | the relay-hop check (edge cost 0 only) |
| `seedaxis.tar.gz` | 72 | A and C at E = 0.7 under six relationship profiles | Study 4, profiles against the placebo |
| `repscan.tar.gz` | 48 | A at E = 0.7 with accurate, random or no reputation marks | Study 4, reputation marks |

Every model is `deepseek/deepseek-v4-flash` except `matched-E0-glm`.
`matched-E0` ran on the morning of 1 September, before answer-only access was
added: its public and private configurations share one older interface, so they
compare formation only. The console logs of `axis2x2` and `realistic`, which
sat at the repository root as `runs-axis2x2.log` and `runs-realistic.log`,
restore as `runs/axis2x2.log` and `runs/realistic.log`. No archive contains API
credentials.

## Restore and verify

From the repository root, into an empty or absent `runs/` directory
(`--keep-old-files` refuses to overwrite anything already there):

```bash
sha256sum -c data/baseline/SHA256SUMS            # macOS: shasum -a 256 -c
for archive in data/baseline/*.tar.gz; do
  tar --keep-old-files -xzf "$archive"
done
sha256sum --quiet -c data/baseline/FILES.sha256  # macOS: shasum -a 256 -c --quiet
```

`SHA256SUMS` verifies the seven archives and `FILES.sha256` every restored file.

## Reproducing the manuscript's numbers

The manuscript's `analysis/aamas-2027/analyze.py` reads these runs together
with the `data/raw/` archives. On 1 October 2026 it was run on records restored
from these archives in a clean directory, and its three outputs (`numbers.tex`,
`results.json`, `episodes.csv`) matched the committed copies byte for byte.
