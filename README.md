# Self-Refactoring: Data

Experimental data for the paper:

> **Can LLMs Refactor Their Own Smelly Code? Self-Refactoring Effectiveness and Behavioral Preservation Across Four Open-Source Models**
> Dong Kwan Kim, Young-Wuk Lee, Jung Sik Jeong. *Applied Sciences* (MDPI), 2026.

This repository contains the **core experimental data** underlying the tables and figures of the paper. All results were produced from real (live) LLM inference and the accompanying smell-detection pipeline; intermediate checkpoints, logs, and raw third-party corpora are **not** included (see *Data Sources* below).

---

## Repository structure

```
data/
├── master_results.json                     # Aggregated results across all research questions
│
├── rq1_smells/                             # RQ1 — LLM-specific code smell taxonomy & prevalence
│   ├── rq1_real_results.json               #   Smell prevalence per model (Table 3, Figure 3)
│   ├── real_smell_analysis.json            #   Overall smell-count summary (5,700 snippets)
│   ├── kappa_report.json                   #   Card-sort intra-rater agreement (Cohen's κ = 0.91)
│   └── threshold_report.json               #   Detector validation, precision/recall/F1 (Table 4)
│
├── rq2_refactoring/                        # RQ2 — Self-refactoring effectiveness (real inference)
│   ├── rq2_results.json                    #   Aggregate SRR/SVR/RSR/ITC (Table 6)
│   ├── rq2_smell_type_breakdown.json       #   Per-smell-type & per-model SRR (Tables 8, 9)
│   ├── rq2_codellama_real.json             #   Per-snippet refactoring outputs — CodeLlama-13B (n=457)
│   ├── rq2_deepseekcoder_real.json         #   Per-snippet refactoring outputs — DeepSeekCoder-6.7B (n=316)
│   ├── rq2_qwen_real.json                  #   Per-snippet refactoring outputs — Qwen2.5-Coder-14B (n=617)
│   └── rq2_starcoder2_real.json            #   Per-snippet refactoring outputs — StarCoder2-15B (n=14)
│
├── rq2_behavioral_preservation/            # RQ2 — Behavioral Preservation Rate (unit-test based)
│   ├── behavioral_preservation.json        #   BPR, self condition (Table 7; overall 96.0%)
│   ├── behavioral_preservation_zero_shot.json  #   BPR, zero-shot condition
│   └── bpr_statistics.json                 #   Per-model BPR with confidence intervals
│
├── rq3_style_coherence/                    # RQ3 — Style Coherence Hypothesis
│   ├── rq3_style_coherence.json            #   Generation-density vs. SRR correlation (simulation, ρ=0.964)
│   └── rq3_real_correlation_4models.json   #   Real-inference correlation, 4 models (Table 10, ρ=0.23)
│
└── rq4_feedback/                           # RQ4 — Generation–Refactoring Feedback Loop
    ├── rq4_results.json                    #   Simulation feedback-loop results (Table 11, Figure 6)
    ├── rq4_deepseekcoder_fair.json         #   Controlled real-inference experiment, DeepSeekCoder (Table 12, n=48)
    └── rq4_qwen_fair.json                  #   Controlled real-inference experiment, Qwen (Table 12, n=35)
```

## File format

All files are UTF-8 JSON. The `*_real.json` files under `rq2_refactoring/` contain, per snippet, the original (smelly) code, the model-refactored output, the smells detected before/after, and the computed metrics (SRR, iterations, syntax/test validity). The remaining files are aggregated metrics that map directly to the paper's tables and figures (annotated above).

## Data sources

- **LLM-generated code (input).** The raw, pre-generated model solutions originate from the publicly available **BigCodeBench** benchmark (Zhuo et al., *ICLR 2025*), instruct-mode outputs for five open-source models. Because that corpus is large and maintained upstream, it is **not** redistributed here; the relevant input snippets are embedded within the `rq2_refactoring/*_real.json` result files. See: https://github.com/bigcode-project/bigcodebench
- **Experiment-generated data (this repository).** All smell-detection results, refactoring outputs, behavioral-preservation checks, and correlation analyses were produced by the authors' pipeline via live LLM inference (LM Studio, GGUF Q4_K_M, greedy decoding, T=0).

## Models evaluated

CodeLlama-13B-Instruct · DeepSeekCoder-6.7B-Instruct · StarCoder2-15B-Instruct · Qwen2.5-Coder-14B-Instruct (real inference); Mistral-7B and DeepSeekCoder-33B appear in prevalence/simulation analyses only.

## Citation

```bibtex
@article{kim2026selfrefactoring,
  title   = {Can LLMs Refactor Their Own Smelly Code? Self-Refactoring Effectiveness and Behavioral Preservation Across Four Open-Source Models},
  author  = {Kim, Dong Kwan and Lee, Young-Wuk and Jeong, Jung Sik},
  journal = {Applied Sciences},
  year    = {2026}
}
```

## License

The data in this repository is released under **CC BY 4.0**. BigCodeBench-derived inputs remain subject to the original benchmark's license.
