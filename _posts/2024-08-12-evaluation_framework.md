---
layout: post
title: Evaluation Framework usingLLM-As-A-Judge
date: 2024-08-12 11:11:11-111
description: Creating an evaluation framework using llm-as-a-judge
tags: evaluation_famrwork llm_as_a_judge
categories: LLM
related_posts: false
giscus_comments: true
---

**Disclaimer**
This work was primarily done by me as a consultant working for Dataworkz, the original article reference is provided below:
<br>Original Article: [Dataworkz RAG Builder: Evaluation Framework](https://www.dataworkz.com/2024/08/12/dataworkz-rag-builder-evaluation-framework/)

# Evaluation Framework

The most popular metrics for evaluating QnA systems are **BLEU**, **ROUGE-N**, and **BertScore**. These metrics measure textual similarity but can sometimes be misleading.
For example, two LLM responses might be phrased differently yet satisfy the same customer need, or two similarly phrased responses may vary significantly in their effectiveness.

To address this, it is essential to have an evaluation model that reflects the intended customer experience.
Such a model should capture the subjective experience of each individual answer.
This requires human evaluators—typically domain experts—who can provide nuanced assessments.

To combine both automated and subjective evaluation, we introduce an evaluation method leverages **LLM-as-a-Judge** that captures the essence of a response and provides a measurable metrics. We tested against publicly available datasets and QA benchmarks, highlighting limitations of existing metrics and introducing a more effective solution.

---

## Evaluation Metrics

We incorporate the following three evaluation metrics:

1. **Answer Accuracy** – Comparison of the golden answer to the generated answer.
2. **Context Relevance** – Comparison of the golden context to the generated context.
3. **Faithfulness** – Comparison of the golden context to the generated answer.

This blog focuses on **Answer Accuracy**, with subsequent blogs covering the remaining metrics.

---

## I. Answer Accuracy

### Setup

We used a publicly available finance dataset, **Apple 10-K filing for fiscal year 2022**, and **FinQABench**, a QA benchmark designed for financial applications based on Apple’s 2022 10-K filing.

- **FinQABench**: Contains 100 questions with golden answers and contexts.
- Evaluated Metrics: **BLEU Score**, **ROUGE-1**, **ROUGE-L**, **BertScore** (Precision, Recall, F1), and **Similarity Score**.

### Results and Observations with Existing Metrics

Results for 100 FinQABench questions revealed the following average scores:

| Metric             | Score  |
| ------------------ | ------ |
| **BLEU Score**     | 0.2951 |
| **ROUGE-1**        | 0.5533 |
| **ROUGE-L**        | 0.5095 |
| **BERT Precision** | 0.3158 |
| **BERT Recall**    | 0.5354 |
| **BERT F1**        | 0.4191 |
| **Similarity**     | 0.7302 |

#### Key Observations

- Existing metrics often fail to evaluate numerical or differently formatted data accurately.
- Semantic similarity is inconsistent across various question types.
- Misalignment between golden answers and generated answers leads to misleading metrics.

##### Examples: Fact-Based QnA

| Question                                                                          | Golden Response     | Dataworkz Response                                 | BLEU Score | ROUGE-1 | ROUGE-L | BERT F1 | Similarity Score |
| --------------------------------------------------------------------------------- | ------------------- | -------------------------------------------------- | ---------- | ------- | ------- | ------- | ---------------- |
| What is the date of the certification of the Chief Executive Officer?             | 2022-10-27 00:00:00 | The date of the certification is October 27, 2022. | 0.0000     | 0.1481  | 0.0741  | -0.6088 | 0.3347           |
| What is the jurisdiction of incorporation of Apple South Asia (Thailand) Limited? | Thailand            | The jurisdiction is Thailand.                      | 0.0000     | 0.1538  | 0.1538  | -0.1914 | 0.5613           |

##### Examples: Descriptive QnA

| Question                                   | Golden Response                                                                                  | Dataworkz Response                                                                   | BLEU Score | ROUGE-1 | ROUGE-L | BERT F1 | Similarity Score |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ | ---------- | ------- | ------- | ------- | ---------------- |
| What is Rule 10b5-1 Trading Plans?         | Written document that preestablishes the amounts, prices, and dates of stock purchases or sales. | A written document preestablishing amounts, prices, and dates of stock transactions. | 0.5538     | 0.9216  | 0.9216  | 0.7631  | 0.9534           |
| Who signed the Annual Report on Form 10-K? | List of Apple executives and directors.                                                          | Detailed list of signatories, including titles.                                      | 0.0092     | 0.4516  | 0.4355  | 0.0993  | 0.5076           |

---

## LLM-as-a-Judge

### Introduction

**LLM-as-a-Judge** leverages advanced language models for reference-free evaluations, aligning responses with human preferences. Through prompt engineering, the model evaluates responses across **precision**, **recall**, and **F1 score**, providing more nuanced insights.

### Methodology

1. **Claims Extraction**:
   - Extract claims from golden and candidate responses.
2. **Metrics Calculation**:
   - **Precision**: Common claims / Total claims in candidate response.
   - **Recall**: Common claims / Total claims in golden response.
   - **F1 Score**: Harmonic mean of precision and recall.

---

### Results with LLM-as-a-Judge

| Metric            | Score  |
| ----------------- | ------ |
| **LLM Precision** | 0.8252 |
| **LLM Recall**    | 0.7350 |
| **LLM F1**        | 0.7490 |

#### Example: Fact-Based QnA

| Question                               | Golden Response     | Dataworkz Response | LLM F1 | BLEU Score | ROUGE-1 | ROUGE-L | BERT F1 | Similarity Score |
| -------------------------------------- | ------------------- | ------------------ | ------ | ---------- | ------- | ------- | ------- | ---------------- |
| What is the date of the certification? | 2022-10-27 00:00:00 | October 27, 2022   | 1.0000 | 0.0000     | 0.1481  | 0.0741  | -0.6088 | 0.3347           |

---

## Final Thoughts

Our framework surpasses traditional metrics, providing a robust, human-aligned evaluation system using **LLM-as-a-Judge**. This approach effectively captures the contextual and semantic accuracy of QA systems, ensuring high standards of performance.

For detailed results and open-source code, refer to the [Evaluation Results](#).

---

## References

- [FinQABench](https://huggingface.co/datasets/lighthouzai/finqabench)
- [Apple 10-K Filing 2022](https://investor.apple.com/sec-filings/default.aspx)
- BLEU Score
  - [Wikipedia](https://en.wikipedia.org/wiki/BLEU)
  - [Bleu Metrics](https://medium.com/nlplanet/two-minutes-nlp-learn-the-bleu-metric-by-examples-df015ca73a86)
- ROUGE Score
  - [Wikipedia](https://en.wikipedia.org/wiki/ROUGE)
  - [ROUGE Metrics](https://medium.com/nlplanet/two-minutes-nlp-learn-the-rouge-metric-by-examples-f179cc285499)
- BERT Score
  - [Paper](https://arxiv.org/abs/1904.09675)
  - [Bert Score](https://medium.com/@abonia/bertscore-explained-in-5-minutes-0b98553bfb71)
- [Using LLMs for Evaluation](https://open.substack.com/pub/cameronrwolfe/p/llm-as-a-judge?r=35moeb&utm_campaign=post&utm_medium=web)
- [Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena](https://arxiv.org/abs/2306.05685)
