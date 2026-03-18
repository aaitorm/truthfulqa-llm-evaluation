# TruthfulQA LLM Evaluation: Base vs Instruct Models

> Evaluation of truthfulness and informativeness in LLMs using few-shot prompting and judge models.

---

## Repository Structure

- `src/` → generation and evaluation scripts
- `data/` → generated model outputs in JSON format
- `results/` → figures, screenshots, and evaluation summaries
- `report/` → academic report with methodology and conclusions
- `requirements.txt` → dependencies

---

## Overview

Large Language Models can generate fluent and convincing answers, but they are also prone to producing false statements, common misconceptions, and misleading information.

This project evaluates the behaviour of two language models:

- **Llama-3.1-8B Base**
- **Llama-3.1-8B Instruct**

The evaluation is based on **TruthfulQA-Multi**, using both **English** and **Basque** data. The main goal is to analyze how instruction tuning affects the quality of responses, with special attention to:

- **truthfulness**
- **informativeness**
- **multilingual behaviour**

The project combines prompting, automatic evaluation, and manual analysis to obtain a broader view of model performance.

---

## Methodology

The system is divided into three stages.

### Z1 — Few-shot generation

In the first stage, both models are prompted with the same few-shot setup using examples from the training split.

- **Llama-3.1-8B Base** is prompted with a plain Q/A format
- **Llama-3.1-8B Instruct** uses the tokenizer chat template
- The same methodology is applied to both Spanish and Basque data

The outputs for the full validation set are stored in JSON files for later evaluation.

---

### Z2 — Automatic evaluation

In the second stage, generated answers are evaluated automatically with independent judge models:

- **Truthfulness judge:** `HiTZ/gemma-2-9b-it-multi-truth-judge`
- **Informativeness judge:** `HiTZ/gemma-2-9b-it-multi-info-judge`

These judges classify each answer with a binary decision:

- **truthful / not truthful**
- **informative / not informative**

This stage allows large-scale comparison between Base and Instruct outputs.

---

### Z3 — Manual analysis

The final stage focuses on qualitative inspection.

The worst-scoring examples are manually reviewed in order to:

- compare generated answers against the reference answers
- identify where the model fails
- identify where the automatic evaluator fails
- analyze multilingual limitations, especially in Basque

This stage is important because automatic metrics alone do not fully capture answer quality.

---

## Dataset

This project uses **TruthfulQA-Multi**, a multilingual adaptation of TruthfulQA.

The dataset includes:

- misleading or misconception-triggering questions
- one best reference answer
- alternative incorrect answers
- category and question type metadata
- training examples for few-shot prompting
- a validation split used in all experiments

The experiments in this repository focus on:

- **Spanish**
- **Basque**

---

## Models

### Generation models
- `meta-llama/Llama-3.1-8B`
- `meta-llama/Llama-3.1-8B-Instruct`

### Judge models
- `HiTZ/gemma-2-9b-it-multi-truth-judge`
- `HiTZ/gemma-2-9b-it-multi-info-judge`

---

## Key Findings

The experiments were designed to study the effect of instruction tuning on response quality. According to the project report, the instruct-aligned model produces responses that are generally more coherent, useful, and reliable than the base model, while multilingual evaluation reveals additional challenges in Basque. :contentReference[oaicite:4]{index=4}

At a system level, the project shows three important points:

- instruction tuning improves response quality over the base model
- automatic judges are useful for large-scale comparison
- manual analysis is necessary to detect evaluator errors and multilingual edge cases

---

## Example Research Questions

This project explores questions such as:

- Does instruction tuning improve truthfulness?
- Does it also improve informativeness?
- Do judge models align with human judgement?
- Does performance degrade in multilingual settings such as Basque?

---
## Results Visualization

### Quantitative Comparison

>TRUTHFULNESS:

![Metrics](results/truthfulness_comparison_es.png)
![Metrics](results/truthfulness_comparison_eus.png)


>INFORMATIVENESS:
![Metrics](results/informativeness_comparison_es.png)
![Metrics](results/informativeness_comparison_eus.png)
>
### Example Outputs

![Examples](results/baseVSinstruct_comparison_es.png)
![Examples](results/baseVSinstruct_comparison_eus.png)
---

## Files

### Main scripts
- `src/z1_generation.py` → answer generation with Base and Instruct
- `src/z2_truth.py` → automatic truthfulness evaluation
- `src/z2_informativeness.py` → automatic informativeness evaluation

### Data files
- `data/z1_outputs_full.json` → Spanish outputs
- `data/z1_outputs_full_eus.json` → Basque outputs

---

## How to Run

Install dependencies:

```bash
pip install -r requirements.txt
```

Run generation:

```bash
python src/z1_generation.py
```

Run truthfulness evaluation:

```bash
python src/z2_truth.py
```

Run informativeness evaluation:

```bash
python src/z2_informativeness.py
```

---

## Notes

- The original code was developed in a research/academic context
- Some scripts were initially created from Colab notebooks and then adapted for repository use
- Paths may need to be adjusted depending on the local environment
- Large output files and figures are included only when useful for reproducibility and inspection

---

## Report

The full academic report is available in:

```text
report/
```

---

## Future Improvements

Possible next steps for this project include:

- cleaner modularization of the evaluation pipeline
- unified configuration for paths and models
- better visualization of metrics
- extension to more languages and more judge models
- deeper agreement analysis between automatic and manual evaluation

---

## Author

Aitor Milicua  
Artificial Intelligence student focused on machine learning, NLP, and applied AI systems.
