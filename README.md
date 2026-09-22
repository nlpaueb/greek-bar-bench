# GreekBarBench 🇬🇷🏛️⚖️

This repository contains the code and prompts for two related benchmarks built on the Greek Bar exams:

- **GreekBarBench (GBB)** — free-text legal reasoning with citations.
  *GreekBarBench: A Challenging Benchmark for Free-Text Legal Reasoning and Citations*, Findings of EMNLP 2025.
- **GreekBarRetrieval (GBR)** — retrieving the statutory articles a question turns on.
  *GreekBarRetrieval: A Benchmark for Greek Statutory Retrieval*, NLLP 2026.

The paper introduces the **GreekBarBench** dataset, designed for evaluating models on complex free-text legal reasoning with citations. GreekBarBench comprises legal questions sourced from the Greek Bar exams, requiring models to analyze case facts, identify applicable legal articles from provided context, synthesize this information, and provide a free-text answer that includes explicit citations. This benchmark is designed to evaluate complex legal reasoning abilities, including multi-hop reasoning and the accurate application of statutory law, simulating the open-book format of the exams.

## Project Overview

GreekBarBench (GBB) is a benchmark specifically curated to evaluate the ability of large language models (LLMs) to perform complex legal reasoning in Greek. It focuses on tasks that require models to:

1.  Read and understand detailed case facts.
2.  Identify and apply relevant legal principles from provided statutory texts.
3.  Synthesize facts and law to arrive at a conclusion.
4.  Generate a free-text answer.
5.  Include explicit citations to both case facts and legal articles used in the reasoning.

The dataset is based on past Greek Bar exams (2015-2024) covering five key legal areas. It is designed to be challenging due to the multi-step reasoning, large context windows required (for the legal codes), and the citation requirement.

## Paper

- **GBB:** [arXiv:2505.17267](https://arxiv.org/abs/2505.17267) — [Findings of EMNLP 2025](https://aclanthology.org/2025.findings-emnlp.1368/)
- **GBR:** [arXiv:2608.18752](https://arxiv.org/abs/2608.18752) — to appear at NLLP 2026

## Dataset

Both benchmarks live in one Hugging Face repo:
[AUEB-NLP/greek-bar-bench](https://huggingface.co/datasets/AUEB-NLP/greek-bar-bench)

| Subset | Rows | What a row is |
|---|---|---|
| `greekbarbench` | 284 | An exam question, with facts, the official answer, and the legal chapters as context |
| `gbb-jme` | 305 | One model's answer to one of 61 questions, with two lawyers' scores |
| `corpus` | 6,308 | A statutory article — the pool GBR retrieves from |
| `queries` | 257 | A question plus its facts, as the retrieval input |
| `qrels` | 660 | A (question, gold article) pair |

All subsets share a stable `id` of the form `{area}_{date}_q{number}`, so a retrieval query
joins to its full record in `greekbarbench`.

```python
from datasets import load_dataset
ds = load_dataset("AUEB-NLP/greek-bar-bench", "queries", split="test", revision="v2.0")
```

Pin `revision="v2.0"` for reproducibility: the 2024 questions are held out as a semi-private
set and are planned for release in a later revision. Tag `v1.0-emnlp2025` is the state of the
repo at EMNLP publication.

## Evaluation Methodology

Evaluating free-text legal reasoning with citations is challenging. The benchmark utilizes a three-dimensional scoring system and an LLM-as-a-judge framework, validated via a meta-evaluation benchmark (GBB-JME).

### LLM-as-a-Judge Framework

Given the cost and subjectivity of human evaluation, an LLM is employed to automatically score candidate model responses. This repository includes code and prompts for implementing and using LLM-judges. Two main prompting strategies are explored:

1.  **Simple-Judge:** Uses a basic prompt outlining the task and scoring criteria.
2.  **Span-Judge:** Leverages human-annotated span-based rubrics (from the `spans` field in `greekbarbench.csv`) to guide the LLM judge's evaluation process, directing it to specific facts, cited law, and analysis components within the ground truth answer.

The Span-Judge approach, using the `spans` field as a reference, was found to correlate much better with human judgments. The specific LLM-judge model and prompt combination yielding the highest correlation (GPT-4.1-mini with Span-Judge prompt) is recommended and supported by the code in this repository for evaluating new models.

### Judge Meta-Evaluation (GBB-JME)

The GBB-JME dataset (`gbb_jme.csv`) serves as a meta-benchmark to evaluate how well different LLM-judges correlate with human expert scores. By comparing the scores given by an LLM-judge to the human scores in `gbb_jme.csv` (using metrics like Soft Pairwise Accuracy), we can assess the reliability of the LLM-judge itself. This meta-evaluation process informed the choice of the primary LLM-judge used in the paper and this repository for larger scale evaluation.

## GreekBarRetrieval

GBB hands the model the relevant legal chapters. GBR removes that assumption: given only the
question and the case facts, a system must find the applicable articles itself, from 6,308
candidates across 23 Greek legal sources. Gold articles are those cited in the official
suggested solutions — on average 2.57 per query, and the labels are not exhaustive.

To get started, see
[`quickstart/quickstart_greekbarretrieval.ipynb`](quickstart/quickstart_greekbarretrieval.ipynb):
BM25-GreekStemmer and EmbeddingGemma-300M baselines end to end, plus the benchmark's metrics.
No API keys required.

## Citation

If you find this work helpful for your research, please cite our paper using the following BibTeX entry:

```bibtex
@inproceedings{chlapanis-etal-2025-greekbarbench,
    title = "{G}reek{B}ar{B}ench: A Challenging Benchmark for Free-Text Legal Reasoning and Citations",
    author = "Chlapanis, Odysseas S.  and
      Galanis, Dimitrios  and
      Aletras, Nikolaos  and
      Androutsopoulos, Ion",
    editor = "Christodoulopoulos, Christos  and
      Chakraborty, Tanmoy  and
      Rose, Carolyn  and
      Peng, Violet",
    booktitle = "Findings of the Association for Computational Linguistics: EMNLP 2025",
    month = nov,
    year = "2025",
    address = "Suzhou, China",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2025.findings-emnlp.1368/",
    doi = "10.18653/v1/2025.findings-emnlp.1368",
    pages = "25099--25119",
    ISBN = "979-8-89176-335-7"
}
```

```bibtex
@inproceedings{beta-etal-2026-greekbarretrieval,
    title = "{G}reek{B}ar{R}etrieval: A Benchmark for {G}reek Statutory Retrieval",
    author = "Beta, Ernest  and
      Chlapanis, Odysseas S.  and
      Galanis, Dimitrios  and
      Androutsopoulos, Ion",
    booktitle = "Proceedings of the Natural Legal Language Processing Workshop 2026",
    year = "2026",
    publisher = "Association for Computational Linguistics",
    url = "https://arxiv.org/abs/2608.18752",
    note = "To appear",
}
```

## Contact

For questions or feedback, please contact:

[Odysseas S. Chlapanis](https://github.com/odychlapanis)
