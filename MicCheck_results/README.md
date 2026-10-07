# MicCheck protocol-v2 results

This directory contains the curated aggregate outputs from the completed
protocol-v2 standard run. The run used 2,000 sampled IHM/SDM pairs drawn from
10,595 discovered metadata pairs, covering 21 speakers and 18 meetings.

All committed CSVs report `protocol_version = 2.0`. The notebook and these
results use the same frozen-probe comparison protocol.

## Aggregate tables

- `dataset_summary.csv`: discovery and sampling audit.
- `baseline_results.csv`: speaker-held-out recording-condition probes,
  closed-set speaker-information probes, and the small lexical probe.
- `bootstrap_intervals.csv`: item-, speaker-, and meeting-bootstrap intervals
  where applicable.
- `feature_ablation_results.csv`: predefined feature-family and waveform-level
  amplitude ablations.
- `grouped_microphone_results.csv`: separate speaker-held-out and
  meeting-held-out recording-condition folds.
- `meeting_grouped_downstream_results.csv`: meeting-held-out speaker and small
  lexical probe directions.
- `normalization_results.csv`: source-only preprocessing and separate CORAL
  target-aware adaptation results.
- `pair_alignment_results.csv`: frozen-probe and geometric results for each
  pair-alignment weight.
- `standardized_frozen_probe_results.csv`: directly comparable points used by
  the central trade-off figure.
- `final_summary.csv`: compact protocol-v2 summary, including CORAL's distinct
  comparison scope.

## Selected checks

- The validation-selected primary recording-condition model, a linear SVM,
  reached 95.46% speaker-held-out balanced accuracy.
- Mean five-fold balanced accuracy was 95.52% for speaker-held-out and 94.92%
  for meeting-held-out evaluation.
- Removing both energy and duration retained 95.16% balanced accuracy; peak
  and RMS waveform normalization each retained 94.96% with all features.
- Validation selected pair-alignment λ=0.1. Relative to λ=0, it reduced matched
  pair distance by 27.89% but condition-probe accuracy by only 0.87 percentage
  points.
- At λ=1, pair distance fell by 72.89% while condition-probe accuracy fell by
  only 1.74 percentage points. Geometric alignment therefore did not imply
  invariance.

## Excluded artifacts

AMI audio, transcripts, the paired manifest, extracted features,
excluded-sample logs, transcript-bearing counterfactual rows, and per-example
plots are not committed. They remain reproducible through the notebook but are
not required to review the aggregate findings.
