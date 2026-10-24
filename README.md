# RAG Evaluation

Evaluating a Retrieval-Augmented Generation (RAG) pipeline with **LangChain**, **LangGraph** and **LangSmith**. The project builds a RAG system over PDF documents, runs it against a test dataset, and scores the answers with automated evaluators for **correctness**, **groundedness**, **relevance** and **retrieval relevance**.

---

## Table of Contents

- [Overview](#overview)
- [Evaluation Metrics](#evaluation-metrics)
- [Results](#results)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Roadmap](#roadmap)
- [Author](#author)

---

## Overview

Building a RAG app is easy. Knowing whether it actually works is harder. This project shows how to measure RAG quality in a repeatable way:

1. Load and split PDF documents into chunks.
2. Embed the chunks and retrieve relevant context for a question.
3. Generate an answer with an LLM, orchestrated through LangGraph.
4. Create a **dataset** of questions and reference answers in LangSmith.
5. Run an **experiment** that scores every answer with LLM-as-judge evaluators.
6. Inspect the scores, latency and token usage in the LangSmith dashboard.

Everything is in a single notebook, `rag_evals.ipynb`.

## Evaluation Metrics

| Evaluator | What it checks |
|---|---|
| **Correctness** | Does the generated answer match the reference answer? |
| **Groundedness** | Is the answer supported by the retrieved documents, with no hallucinated claims? |
| **Relevance** | Does the answer actually address the user's question? |
| **Retrieval relevance** | Are the retrieved documents relevant to the question? |

Scores range from **0 to 1**.

## Results

The dataset **RAG Test Evaluation** was run through the experiment `rag-doc-relevance-8ba97fc1`.

### Experiments overview

Feedback scores, latency (P50 / P99) and token usage for the experiment.

![Experiments overview](screenshots/experiments-overview.png)

### Experiment results

Per-example view of the inputs, reference outputs, generated outputs and evaluator scores.

![Experiment results](screenshots/experiment-results.png)

### Summary

| Item | Value |
|---|---|
| Dataset | RAG Test Evaluation |
| Experiment | `rag-doc-relevance-8ba97fc1` |
| Examples | 3 |
| Runs completed | 3 / 3 |
| Correctness (avg) | 1.00 |
| Groundedness (avg) | 1.00 |
| Relevance (avg) | 1.00 |
| Retrieval relevance (avg) | 1.00 |

The three test questions cover ReAct agents and self-reflection, the five types of adversarial attacks, and biases in few-shot prompting.

> **Note:** The dataset is small (3 examples), so perfect scores show the pipeline works end to end but are not a strong benchmark. See the [Roadmap](#roadmap) for ideas on making the evaluation harder.

## Tech Stack

- **Python**
- **LangChain** and **LangGraph** for the RAG pipeline and orchestration
- **Google Generative AI** (`langchain-google-genai`) and **Groq** (`langchain-groq`) as LLM providers
- **Hugging Face** embeddings (`langchain_huggingface`)
- **pypdf** for PDF loading
- **LangSmith** for datasets, experiments, evaluators and tracing
- **Streamlit** for a UI
- **python-dotenv** for managing API keys

## Project Structure

```
rag-evaluation/
├── rag_evals.ipynb     # RAG pipeline + LangSmith evaluation workflow
├── requirements.txt    # Python dependencies
├── screenshots/        # Result screenshots used in this README
└── .gitignore
```

## Getting Started

### Prerequisites

- Python 3.9 or higher
- A [LangSmith](https://smith.langchain.com/) account and API key
- An API key for at least one LLM provider (Google Gemini or Groq)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/PremSalokhe1060/rag-evaluation.git
cd rag-evaluation

# 2. (Optional) Create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

### Environment variables

Create a `.env` file in the project root:

```env
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=your_langsmith_api_key
GOOGLE_API_KEY=your_google_api_key
GROQ_API_KEY=your_groq_api_key
```

Only add the keys for the providers you use. Never commit your `.env` file.

## Usage

1. Open the notebook:

   ```bash
   jupyter notebook rag_evals.ipynb
   ```

2. Run the cells in order to build the RAG pipeline, create the dataset and run the experiment.
3. Open [LangSmith](https://smith.langchain.com/) and go to **Datasets & Experiments → RAG Test Evaluation** to view scores, latency and token usage.
4. Open an individual experiment to compare inputs, reference outputs and generated outputs side by side.

## Roadmap

- [ ] Grow the dataset with more and harder questions
- [ ] Add questions that the documents cannot answer, to test hallucination handling
- [ ] Compare chunk sizes, embedding models and retrievers using LangSmith experiments
- [ ] Compare LLM providers (Gemini vs. Groq-hosted models)
- [ ] Add a Streamlit front end for interactive querying
- [ ] Add retrieval metrics such as precision@k and recall@k

## Author

**Prem Salokhe**
GitHub: [@PremSalokhe1060](https://github.com/PremSalokhe1060)

---

If you found this useful, consider giving the repo a star.
