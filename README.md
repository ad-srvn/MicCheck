# MicCheck

MicCheck measures how strongly lightweight speech representations encode recording conditions. It compares paired close-talk (IHM) and distant-microphone (SDM) segments from the AMI Meeting Corpus.

The central finding is simple: bringing paired embeddings closer does not necessarily remove recording-condition information.

## What it evaluates

- Recording-condition recovery on held-out speakers and meetings
- Closed-set speaker and small lexical transfer across conditions
- Feature-family and amplitude ablations
- Source-only normalization and target-aware CORAL adaptation
- Paired representation alignment and residual condition leakage
- Clustered uncertainty and an exploratory spectral counterfactual

IHM and SDM differ in distance, room response, reverberation, noise, placement, gain, and hardware. The study therefore measures the broader recording condition, not an isolated microphone effect.

## Run in Google Colab

1. Download `MicCheck_AMI.ipynb`.
2. Upload it to [Google Colab](https://colab.research.google.com/).
3. Run all cells in order.

The notebook installs its dependencies, streams AMI through Hugging Face, and processes only the selected audio. It does not download the full corpus.

## Run locally

Python 3.11 or 3.12 is recommended.

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

Set `RUN_MODE` in the notebook to `smoke`, `standard`, or `extended`. `MAX_PAIRS` limits audio processing after complete metadata discovery.

## Method

Both recordings from each pair remain in the same partition. Scalers, PCA, encoders, condition responses, and probes are fit on training data only; validation data selects hyperparameters. CORAL alone uses permitted unlabeled target-condition training data and is reported separately.

The notebook writes tables and figures to `results/` and exports a versioned ZIP archive when the run finishes. Generated artifacts, audio, caches, and environments are excluded from Git.

## Repository

```text
MicCheck/
├── MicCheck_AMI.ipynb
├── requirements.txt
├── LICENSE
└── README.md
```

## Limitations

- AMI does not represent every language, room, device, speaking style, or population.
- The speaker task is closed-set; it is not speaker verification.
- The lexical task covers a small set of frequent single words.
- Few speaker or meeting clusters limit bootstrap precision.
- Pair-aligned and classical representations have different training assumptions.
- The spectral counterfactual is exploratory and does not recreate room geometry or noise.
- Condition recoverability alone does not establish demographic unfairness, downstream harm, or causality.

## License

MicCheck code and documentation are available under the [MIT License](LICENSE). The AMI Meeting Corpus is not redistributed and remains subject to its own license.
