# Multi-Metric Evaluation Framework for LLM Responses in Healthcare

A retrieval-grounded benchmarking pipeline that evaluates medical LLM responses across six clinically relevant dimensions using biomedical evidence from PubMed and EuropePMC.

---

## Overview

This project benchmarks commercial LLMs on real-world patient queries using a hybrid retrieval pipeline (BM25 + FAISS) and GPT-4 as an automated judge. Responses are scored across six metrics designed to capture both factual and human-aligned qualities.

## Metrics Evaluated

| Metric | Description |
|---|---|
| Correctness | Factual accuracy against retrieved evidence |
| Hallucination | Resistance to generating unsupported claims |
| Completeness | Coverage of clinically relevant information |
| Faithfulness | Alignment with retrieved source documents |
| Groundedness | Citation of domain-specific evidence |
| Empathy | Appropriate patient-facing tone and sensitivity |

## Models Benchmarked

- GPT-3.5-turbo (OpenAI)
- Claude 3 Opus (Anthropic)
- DeepSeek V3

## Key Results

| Metric | 1st | 2nd | 3rd |
|---|---|---|---|
| Correctness | DeepSeek (8.04) | ChatGPT (7.89) | Claude (7.86) |
| Hallucination | DeepSeek (8.43) | ChatGPT (8.39) | Claude (8.21) |
| Completeness | DeepSeek (8.07) | ChatGPT (7.89) | Claude (7.54) |
| Faithfulness | DeepSeek (7.29) | Claude (7.25) | ChatGPT (7.00) |
| Groundedness | Claude (6.32) | DeepSeek (6.32) | ChatGPT (6.07) |
| Empathy | DeepSeek (8.82) | ChatGPT (8.32) | Claude (7.32) |

## Project Structure

```
LLMs-Healthcare-Evaluation/
├── MedicalLLM_Evaluation_Pipeline.ipynb   # End-to-end pipeline
├── Results_Visualizer.ipynb               # Metrics visualisation
├── Results.csv                   # Final metric scores
├── sample_responses.csv                   # Input queries
├── requirements.txt                       # Dependencies
└── Trials/                                # Prior metric experiments
    ├── Accuracy/
    ├── Empathy/
    └── data.csv
```

## Setup

```bash
pip install -r requirements.txt
```

Create a `.env` file in the root folder with your API keys:

```
OPENAI_API_KEY=your_key_here
ANTHROPIC_API_KEY=your_key_here
DEEPSEEK_API_KEY=your_key_here
```

## Pipeline Summary

1. Accepts 32 real-world patient queries from the ChatDoctor dataset
2. Transforms queries into PubMed search strings using LLM prompting
3. Retrieves biomedical documents (cap: 50) from PubMed and EuropePMC
4. Ranks documents using BM25 (sparse) + FAISS (dense) hybrid scoring
5. Synthesises a pseudo-gold document using GPT-4
6. Generates responses from GPT-3.5, Claude, and DeepSeek via LangChain
7. Scores each response across 6 metrics using GPT-4 as judge
8. Outputs structured results to `Results.csv`

## Prerequisites

- Python 3.8+
- API keys for OpenAI, Anthropic, and DeepSeek
- Basic familiarity with Jupyter Notebooks and LangChain