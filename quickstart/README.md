# Quickstart

Two notebooks, both runnable in Google Colab.

### GreekBarBench — answering and LLM-judge

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nlpaueb/greek-bar-bench/blob/main/quickstart/quickstart_greekbarbench.ipynb)

Replicates the answering and judging experiments of the paper. You will need API keys for
OpenAI and Google models, or you can upload the generated *quickstart_data* we provide to run
it without spending anything.

### GreekBarRetrieval — statutory retrieval

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nlpaueb/greek-bar-bench/blob/main/quickstart/quickstart_greekbarretrieval.ipynb)

Runs two retrieval baselines end to end over the 6,308-article corpus: BM25 with a Greek
stemmer (CPU only) and EmbeddingGemma-300M (any GPU). **No API keys are needed.** It also
implements the benchmark's metrics — nDCG@10/@100, Recall@10/@100, MAP@100 — in plain Python.
