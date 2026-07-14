# Self-Refactoring: Data

Experimental data for the paper:

> **Can LLMs Refactor Their Own Smelly Code? Self-Refactoring Effectiveness and Behavioral Preservation Across Four Open-Source Models**
> Dong Kwan Kim, Young-Wuk Lee, Jung Sik Jeong. *Applied Sciences* (MDPI), 2026.

This repository contains the **core experimental data** underlying the tables and figures of the paper, produced from real (live) LLM inference and the accompanying smell-detection pipeline. It also includes the exact subset of the public **BigCodeBench** inputs used as the starting point (see *Data Sources* below). Intermediate checkpoints, logs, and the full multi-model BigCodeBench corpus are **not** included.

---

## Repository structure

```
data/
├── master_results.json                     # Aggregated results across all research questions
│
├── rq1_smells/                             # RQ1 — LLM-specific code smell taxonomy & prevalence
│   ├── rq1_real_results.json               #   Smell prevalence per model (Table 3, Figure 3)
│   ├── real_smell_analysis.json            #   Per-model smell counts (5,700 snippets total)
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

public_data/
└── bigcodebench/                           # Public input data actually used (subset of BigCodeBench)
    ├── tasks.jsonl                         #   1,140 BigCodeBench task definitions (prompt + unit tests)
    └── model_outputs_instruct/             #   Pre-generated instruct-mode solutions (1,140 each)
        ├── CodeLlama-13B-Instruct.jsonl
        ├── DeepSeekCoder-6.7B-Instruct.jsonl
        ├── StarCoder2-15B-Instruct.jsonl
        ├── Qwen2.5-Coder-14B-Instruct.jsonl
        ├── Mistral-7B-Instruct-v0.3.jsonl          # RQ1 prevalence & simulation only
        └── DeepSeekCoder-33B-Instruct.jsonl        # upscaled condition (simulation only)
```

## File format

All files are UTF-8 JSON. The `*_real.json` files under `rq2_refactoring/` are organized by condition (`self`, `zero_shot`); each per-snippet record holds `task_id` (which links to the original snippet in `public_data/bigcodebench/`), the model output `refactored_code`, the smells detected before and after (`smells_before`, `smells_after`), and per-snippet metrics (`srr`, `iterations`, `syntax_ok`). The original (smelly) code itself is not duplicated in these records—join on `task_id` with the BigCodeBench model outputs to recover it. The remaining files are aggregated metrics that map directly to the paper's tables and figures (annotated above).

## Data sources

- **LLM-generated code (input), under `public_data/bigcodebench/`.** The raw, pre-generated model solutions originate from the publicly available **BigCodeBench** benchmark (Zhuo et al., *ICLR 2025*). To keep the package self-contained yet compact, we include **only the subset actually used in this study**—the instruct-mode outputs for the six evaluated models (1,140 tasks each) plus the task definitions—rather than the full 163-model corpus. The complete benchmark is maintained upstream at https://github.com/bigcode-project/bigcodebench. These files retain their original content and are redistributed under BigCodeBench's license with attribution.
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
