# Protocol-v1 results snapshot

This directory contains the immutable aggregate outputs from the completed
MicCheck protocol-v1 standard run. The top-level notebook now implements
protocol v2, which has not yet been executed. These CSVs and figures must not
be interpreted as results from the revised methodology.

## Tables

- `dataset_summary.csv`: v1 sample and corpus composition.
- `baseline_results.csv`: v1 recording-condition, closed-set speaker, and
  small lexical probe metrics.
- `bootstrap_intervals.csv`: v1 item-level bootstrap intervals.
- `normalization_results.csv`: v1 preprocessing and CORAL results.
- `invariant_results.csv`: legacy filename for the v1 paired-alignment sweep.
  The current project terminology is **pair-aligned**, not invariant.
- `final_summary.csv`: compact v1 summary using the original evaluation
  semantics.

## Figures

The figures are generated v1 artifacts. In particular,
`central_tradeoff.png` mixes classical cross-condition logistic probes with a
jointly trained neural task head. It is preserved for provenance but should not
be used as a directly comparable representation leaderboard.

The v2 notebook will generate a replacement central plot in which every point
uses the same frozen logistic-probe protocol. It will also generate feature
ablations, meeting-held-out results, clustered uncertainty, and a direct
pair-distance-versus-condition-decodability plot.

AMI audio, transcripts, per-utterance features, and per-example plots are not
redistributed.
