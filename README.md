# MicCheck

**Measuring recording-condition information in lightweight speech representations**

MicCheck uses paired close-talk and distant-microphone recordings of the same
speech events to measure recording-condition information in acoustic features.
It tests whether that information harms cross-condition transfer, identifies
which feature families carry the signal, compares source-only preprocessing
with target-aware adaptation, and asks whether bringing paired recordings
closer in embedding space actually removes recoverable condition information.

> **Core finding:** geometric pair alignment is not necessarily
> representational invariance.

IHM and SDM differ in distance, room acoustics, reverberation, noise, placement,
gain behavior, and hardware. MicCheck therefore studies **close-talk versus
distant-microphone recording condition**. It does not isolate microphone
hardware or establish a causal microphone effect.

## Protocol status

| Component | Status |
|---|---|
| Committed tables and figures | **Protocol v1 results snapshot** from an actual completed run |
| Current notebook methodology | **Protocol v2, pending rerun** |

The committed numeric results were generated with the protocol documented in
the current results snapshot. The notebook now includes balanced metadata
sampling, waveform and feature-family ablations, meeting-held-out evaluation,
clustered uncertainty, and standardized frozen-probe comparisons. Those
improvements require a fresh run before v2 values can be reported.

No committed CSV has been edited to imitate v2 output. The original mixed-
protocol central trade-off figure remains available in the v1 results folder
for provenance, but is intentionally not presented as a headline figure.

## Existing v1 findings

| Experiment | Observed v1 result | Scope |
|---|---:|---|
| Raw recording-condition probe | **98.65% balanced accuracy** | IHM versus SDM, held-out speakers |
| Closed-set speaker-information probe, same condition | **0.764 macro-F1** | Known speakers, held-out utterance pairs |
| Closed-set speaker-information probe, cross condition | **0.300 macro-F1** | Known speakers, held-out utterance pairs |
| Small lexical probe, same condition | **0.769 macro-F1** | Three words in this run |
| Small lexical probe, cross condition | **0.599 macro-F1** | Three words in this run |
| MFCC-CMVN recording-condition probe | **95.8% balanced accuracy** | Only the cepstral portion was normalized |
| CORAL cross-condition speaker probe | **0.500 macro-F1** | Target-aware UDA using unlabeled target-condition training data |

These results show strong recording-condition recoverability and weak
cross-condition transfer in this particular subset. They do not establish a
universal effect, demographic unfairness, or causality.

## Pair alignment is not the same as channel invariance

In the v1 alignment sweep, increasing the alignment weight from λ=0 to λ=1
reduced mean matched-pair cosine distance from **0.131 to 0.028**, a **78.3%**
reduction. Over the same endpoints, recording-condition probe accuracy changed
only from **94.1% to 92.0%**, a reduction of about **2.1 percentage points**.

The important observation is not that the representation became invariant. It
is that matched recordings moved much closer while their recording condition
remained highly decodable. The v2 notebook now evaluates pair distance and
frozen-probe leakage separately for every λ.

![Protocol-v1 paired-alignment sweep](MicCheck_results/figures/invariant_sweep.png)

The filename and labels above are retained from the historical v1 artifact;
the current notebook uses the more accurate term **pair-aligned**.

## Study design

```text
AMI paired speech events
        |
        +-- IHM: close-talk headset condition
        +-- SDM: distant-microphone condition
                         |
              metadata-first pairing
                         |
       balanced sampling across meetings/speakers
                         |
             acoustic representations
        +----------------+----------------+
        |                |                |
  condition probes   transfer probes   mitigation
  feature ablations  speaker/lexical   source-only preprocessing
  amplitude controls same/cross        CORAL target-aware UDA
                                      paired alignment
                         |
            standardized frozen probes
        condition accuracy vs cross-condition F1
```

The v2 notebook makes the following comparisons explicit:

- **Channel recoverability:** can a probe identify IHM versus SDM?
- **Cross-channel robustness:** how much does downstream performance change?
- **Geometric alignment:** how close are matched embeddings?
- **Representational leakage:** can a frozen probe still recover condition?

## Selected v1 visual evidence

### Recording-condition recoverability

| Confusion matrix | Most predictive acoustic features |
|---|---|
| ![V1 recording-condition confusion matrix](MicCheck_results/figures/microphone_confusion.png) | ![V1 recording-condition coefficients](MicCheck_results/figures/microphone_coefficients.png) |

### Cross-condition task transfer

| Closed-set speaker-information probe | Small lexical probe |
|---|---|
| ![V1 speaker-information transfer](MicCheck_results/figures/speaker_cross_channel.png) | ![V1 small lexical transfer](MicCheck_results/figures/lexical_cross_channel.png) |

### Exploratory counterfactual diagnostic

| Probability shift | Estimated spectral response |
|---|---|
| ![V1 counterfactual probability shifts](MicCheck_results/figures/counterfactual_probability.png) | ![V1 estimated recording-condition response](MicCheck_results/figures/estimated_channel_response.png) |

Only three v1 counterfactual examples were available. This is a qualitative
diagnostic, not quantitative or causal evidence.

## Run directly in Google Colab

The notebook is self-contained and does not require `requirements.txt` in
Colab.

1. Download `MicCheck_AMI.ipynb` from this repository.
2. Open [Google Colab](https://colab.research.google.com/) and upload it.
3. Run all cells from top to bottom.

The installation cell declares its dependencies directly. AMI is streamed
through Hugging Face, selected audio is processed on demand, and the full
corpus is not downloaded.

## Run locally

Python 3.11 or 3.12 is recommended:

```bash
git clone https://github.com/ad-srvn/MicCheck.git
cd MicCheck
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter lab MicCheck_AMI.ipynb
```

On Windows PowerShell, activate the environment with:

```powershell
.venv\Scripts\Activate.ps1
```

The workload presets are:

- `smoke`: a quick pipeline check.
- `standard`: up to 2,000 audio pairs.
- `extended`: up to 5,000 audio pairs.

`MAX_PAIRS` limits expensive audio processing, not metadata discovery. The v2
sampler discovers valid metadata pairs first, then deterministically samples
round-robin across meeting–speaker groups.

## What protocol v2 adds

- Direct Colab dependency installation.
- Protocol-version metadata in generated results.
- Complete metadata pairing before deterministic balanced subset selection.
- Raw, peak-normalized, and RMS-normalized waveform representations.
- Predefined MFCC, delta, spectral, energy, pitch/voicing, and nuisance-removal
  feature ablations.
- Separate speaker-held-out and meeting-held-out recording-condition results.
- Direction-specific closed-set speaker and small lexical probes.
- Explicit MFCC-CMN and MFCC-CMVN terminology.
- Separation of source-only methods from CORAL (UDA).
- Identical frozen logistic-probe semantics for the central comparison.
- Speaker- and meeting-cluster bootstrap intervals.
- Separate automated assessments for geometric pair alignment and recoverable
  condition information.

## Repository layout

```text
MicCheck/
├── MicCheck_AMI.ipynb        # self-contained protocol-v2 notebook
├── MicCheck_results/         # immutable protocol-v1 snapshot
│   ├── *.csv                 # generated v1 aggregate results
│   └── figures/              # generated v1 figures
├── requirements.txt          # local runtime dependencies
├── LICENSE
└── README.md
```

Fresh runs write to `results/` and create a protocol-versioned archive. AMI
audio, transcripts, per-utterance manifests, extracted feature matrices,
caches, and local environments are intentionally excluded from Git.

## Important limitations of the v1 snapshot

- It contains 2,000 pairs from 14 speakers and 5 meetings; the test partition
  contains only 3 speakers and 2 meetings.
- Meetings overlap across its speaker-grouped partitions.
- Its speaker task is closed-set, not unseen-speaker identification or speaker
  verification.
- Its lexical probe contains only `yeah`, `okay`, and `um`, with 40 test pairs.
- Its confidence intervals are item-level rather than cluster-level.
- Its original central trade-off figure mixes frozen classical probes with a
  jointly trained neural task head and is not directly comparable point by
  point. The v2 notebook fixes this for future runs.
- CORAL uses unlabeled target-condition training data and is not a source-only
  baseline.

## Responsible interpretation

Recording-condition leakage means that a model can recover condition
information from its representation. It does not by itself prove demographic
bias, unfair treatment, or causality. Report the corpus, condition contrast,
split policy, cluster counts, uncertainty method, and adaptation assumptions
whenever using these results.

## License

MicCheck code and documentation are released under the [MIT License](LICENSE).
The AMI Meeting Corpus is not redistributed and remains subject to its own
license and usage terms.
