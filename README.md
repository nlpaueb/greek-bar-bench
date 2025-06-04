# GreekBarBench 🇬🇷🏛️⚖️

This repository contains the code, prompts, and data associated with the research presented in the paper:

**GreekBarBench: A Challenging Benchmark for Free-Text Legal Reasoning and Citations**

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

The full paper describing the dataset, methodology, and results is available on arXiv:
[https://arxiv.org/abs/2505.17267v1](https://arxiv.org/abs/2505.17267)

## Dataset

The **GreekBarBench** dataset is publicly available on Hugging Face Datasets:
[https://huggingface.co/datasets/AUEB-NLP/greek-bar-bench](https://huggingface.co/datasets/AUEB-NLP/greek-bar-bench)

The core dataset files (`greekbarbench.csv` and `gbb_jme.csv`) used in this repository are included in the `data/` directory.

## Evaluation Methodology

Evaluating free-text legal reasoning with citations is challenging. The benchmark utilizes a three-dimensional scoring system and an LLM-as-a-judge framework, validated via a meta-evaluation benchmark (GBB-JME).

### LLM-as-a-Judge Framework

Given the cost and subjectivity of human evaluation, an LLM is employed to automatically score candidate model responses. This repository includes code and prompts for implementing and using LLM-judges. Two main prompting strategies are explored:

1.  **Simple-Judge:** Uses a basic prompt outlining the task and scoring criteria.
2.  **Span-Judge:** Leverages human-annotated span-based rubrics (from the `spans` field in `greekbarbench.csv`) to guide the LLM judge's evaluation process, directing it to specific facts, cited law, and analysis components within the ground truth answer.

The Span-Judge approach, using the `spans` field as a reference, was found to correlate much better with human judgments. The specific LLM-judge model and prompt combination yielding the highest correlation (GPT-4.1-mini with Span-Judge prompt) is recommended and supported by the code in this repository for evaluating new models.

### Judge Meta-Evaluation (GBB-JME)

The GBB-JME dataset (`gbb_jme.csv`) serves as a meta-benchmark to evaluate how well different LLM-judges correlate with human expert scores. By comparing the scores given by an LLM-judge to the human scores in `gbb_jme.csv` (using metrics like Soft Pairwise Accuracy), we can assess the reliability of the LLM-judge itself. This meta-evaluation process informed the choice of the primary LLM-judge used in the paper and this repository for larger scale evaluation.

## Citation

If you find this work helpful for your research, please cite our paper using the following BibTeX entry:

```bibtex
@misc{chlapanis2025greekbarbench,
      title={GreekBarBench: A Challenging Benchmark for Free-Text Legal Reasoning and Citations},
      author={Odysseas S. Chlapanis and Dimitrios Galanis and Nikolaos Aletras and Ion Androutsopoulos},
      year={2025},
      eprint={2505.17267},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2505.17267},
}
```

## Contact

For questions or feedback, please contact:

[Odysseas S. Chlapanis](https://github.com/odychlapanis)
```
