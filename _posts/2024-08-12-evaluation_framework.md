---
layout: post
title: Evaluation Framework using LLM-As-A-Judge
date: 2024-08-12 11:11:11-111
description: Creating an evaluation framework using llm-as-a-judge
tags: evaluation_famrwork llm_as_a_judge
categories: LLM
related_posts: false
giscus_comments: true
---

**Disclaimer**
This work was completed by me on behalf of Dataworkz.
<br>Original Article: [Dataworkz RAG Builder: Evaluation Framework](https://www.dataworkz.com/2024/08/12/dataworkz-rag-builder-evaluation-framework/)

# Evaluation Framework

The most popular metrics for evaluating QnA systems are BLEU, ROUGE-N, and BertScore. These metrics measure textual similarity but can sometimes be misleading.
For example, two LLM responses might be phrased differently yet satisfy the same customer need, or two similarly phrased responses may vary significantly in their effectiveness.
To address this, it is essential to have an evaluation model that reflects the intended customer experience. Such a model should capture the subjective experience of each individual answer. This requires human evaluators—typically domain experts—who can provide nuanced assessments. To combine both automated and subjective evaluation, we introduce an evaluation method leverages LLM-as-a-Judge that captures the essence of a response and provides a measurable metrics. We tested against publicly available datasets and QA benchmarks, highlighting limitations of existing metrics and introducing a more effective solution.

---

## Evaluation Metrics

We incorporate the following three evaluation metrics:

1. Answer Accuracy – Comparison of the golden answer to the generated answer.
2. Context Relevance – Comparison of the golden context to the generated context.
3. Faithfulness – Comparison of the golden context to the generated answer.

This blog focuses on Answer Accuracy, with subsequent blogs covering the remaining metrics.

---

## I. Answer Accuracy

### Setup

We used a publicly available finance dataset, Apple 10-K filing for fiscal year 2022, and FinQABench, a QA benchmark designed for financial applications based on Apple’s 2022 10-K filing.

- FinQABench: Contains 100 questions with golden answers and contexts.
- Evaluated Metrics: BLEU Score, ROUGE-1, ROUGE-L, BertScore (Precision, Recall, F1), and Similarity Score.

### Results and Observations with Existing Metrics

Results for 100 FinQABench questions revealed the following average scores:

| BLEU Score | ROUGE-1 | ROUGE-L | BERT Precision | BERT Recall | BERT F1 | Similarity |
| ---------- | ------- | ------- | -------------- | ----------- | ------- | ---------- |
| 0.2951     | 0.5533  | 0.5095  | 0.3158         | 0.5354      | 0.4191  | 0.7302     |

#### Key Observations

- Existing metrics often fail to evaluate numerical or differently formatted data accurately.
- Semantic similarity is inconsistent across various question types.
- Misalignment between golden answers and generated answers leads to misleading metrics.

##### Examples: Fact-Based QnA

In the table below, although all responses are correct, the scores received are substantially lower. This indicates that current metrics fail to verify the generated responses against the golden responses. A critical limitation of these metrics is exposed: if the data is numerical or not formatted exactly like the golden answer, the scores suffer significantly.

| Question                                                                          | Golden Response     | Response                                           | BLEU Score | ROUGE-1 | ROUGE-L | BERT F1 | Similarity Score |
| --------------------------------------------------------------------------------- | ------------------- | -------------------------------------------------- | ---------- | ------- | ------- | ------- | ---------------- |
| What is the date of the certification of the Chief Executive Officer?             | 2022-10-27 00:00:00 | The date of the certification is October 27, 2022. | 0.0000     | 0.1481  | 0.0741  | -0.6088 | 0.3347           |
| What is the jurisdiction of incorporation of Apple South Asia (Thailand) Limited? | Thailand            | The jurisdiction is Thailand.                      | 0.0000     | 0.1538  | 0.1538  | -0.1914 | 0.5613           |

##### Examples: Descriptive QnA

Additionally, we observe similar issues with “Descriptive QnA”. The similarity score is high in the first question, however, this score is not consistent across all types of questions. The answer to the next question in the table below is evidence to that observation. It captures the main facts of the golden answer but the similarity score is not a good measure of its relevance.

| Question                                   | Golden Response                                                                                  | Dataworkz Response                                                                   | BLEU Score | ROUGE-1 | ROUGE-L | BERT F1 | Similarity Score |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ | ---------- | ------- | ------- | ------- | ---------------- |
| What is Rule 10b5-1 Trading Plans?         | Written document that preestablishes the amounts, prices, and dates of stock purchases or sales. | A written document preestablishing amounts, prices, and dates of stock transactions. | 0.5538     | 0.9216  | 0.9216  | 0.7631  | 0.9534           |
| Who signed the Annual Report on Form 10-K? | List of Apple executives and directors.                                                          | Detailed list of signatories, including titles.                                      | 0.0092     | 0.4516  | 0.4355  | 0.0993  | 0.5076           |

Our observations indicate that any one metric is not able to consistently reflect the quality of answer score if their explanations do not closely align with the language of the golden response. This misalignment can be misleading, it fails to accurately reflect the true quality and comprehensiveness of the Q&A system’s responses when compared to the benchmark. Traditional metrics seem to have substantial constraints to correlate with human preferences. Hence, we utilized the current state-of-the-art solution to build a scalable solution which aligns well with human evaluation called “LLM-as-a-Judge”. LLM-as-a-judge is a reference-free metric that directly prompts a powerful LLM to evaluate the quality of another model’s output.

---

## LLM-as-a-Judge

### Introduction

LLM-as-a-Judge leverages advanced language models for reference-free evaluations, aligning responses with human preferences. Through prompt engineering, the model evaluates responses across **precision**, **recall**, and **F1 score**, providing more nuanced insights.

### Methodology

We developed a generic prompt capable of evaluating all types of responses, whether they are descriptive, numerical, or factual. Our evaluation criteria, embedded within the prompt, include the following essential instructions:

1. Calculate the total number of claims in the golden response with respect to the question.
2. Calculate the total number of claims in the candidate response with respect to the question.
3. Calculate the total number of claims common to both the golden response and the candidate response.

This process generates lists of claims for both the golden response and the Dataworkz response. Once these claims are derived from the original responses, we calculate precision, recall, and F1 scores based on the following criteria:

- Precision: Number of claims common to both the golden response and Dataworkz response / Total number of claims in the Dataworkz response.
- Recall: Number of claims common to both the golden response and Dataworkz response / Total number of claims in the golden response.
- F1 Score: Harmonic mean of precision and recall.

---

### Results with LLM-as-a-Judge

LLM-AS-A-Judge was able to capture the essence of the generated responses in comparison to the golden answers. The overall result for the metrics was:

| LLM Precision | LLM Recall | LLM F1 |
| ------------- | ---------- | ------ |
| 0.8252        | 0.7350     | 0.7490 |

#### Example: Fact-Based QnA

The table below indicates that the evaluation framework was able to correctly evaluate the fact based answers irrespective of the format of the generated answer.

| Question                               | Golden Response     | Generated Response | LLM F1 | BLEU Score | ROUGE-1 | ROUGE-L | BERT F1 | Similarity Score |
| -------------------------------------- | ------------------- | ------------------ | ------ | ---------- | ------- | ------- | ------- | ---------------- |
| What is the date of the certification? | 2022-10-27 00:00:00 | October 27, 2022   | 1.0000 | 0.0000     | 0.1481  | 0.0741  | -0.6088 | 0.3347           |

### Descriptive QnA

The evaluation framework was also consistent across “Descriptive QnA” and was able to evaluate the content accurately.

| Question                                                                                   | Golden Response                                                                                                                                                                                                                                                                                              | Generated Response                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | LLM Score (F1) | Bleu Score | Rouge-1 | Rouge-L | Bert Score (F1) | Similarity Score |
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- | ---------- | ------- | ------- | --------------- | ---------------- |
| What is Rule 10b5-1 Trading Plans?                                                         | Rule 10b5-1 Trading Plans is a written document that preestablishes the amounts, prices and dates (or formula for determining the amounts, prices and dates) of future purchases or sales of the Company’s stock, including sales of shares acquired under the Company’s employee and director equity plans. | According to the context, Rule 10b5-1 Trading Plans refer to written documents that preestablish the amounts, prices, and dates (or formula for determining the amounts, prices, and dates) of future purchases or sales of the Company’s stock, including sales of shares acquired under the Company’s employee and director equity plans.                                                                                                                                                                                                                                                                                                         | 1.0000         | 0.5538     | 0.9216  | 0.9216  | 0.7631          | 0.9534           |
| Who signed the Annual Report on Form 10-K on behalf of the Registrant on October 27, 2022? | Timothy D. Cook, Luca Maestri, Chris Kondo, James A. Bell, Al Gore, Alex Gorsky, Andrea Jung, Arthur D. Levinson, Monica Lozano, Ronald D. Sugar, and Susan L. Wagner.                                                                                                                                       | The following persons signed the Annual Report on Form 10-K on behalf of the Registrant on October 27, 2022: 1. Timothy D. Cook – Chief Executive Officer and Director (Principal Executive Officer) 2. Luca Maestri – Senior Vice President, Chief Financial Officer (Principal Financial Officer) 3. Chris Kondo – Senior Director of Corporate Accounting (Principal Accounting Officer) 4. James A. Bell – Director 5. Al Gore – Director 6. Alex Gorsky – Director 7. Andrea Jung – Director 8. Arthur D. Levinson – Director and Chair of the Board 9. Monica Lozano – Director 10. Ronald D. Sugar – Director 11. Susan L. Wagner – Director | 1.0000         | 0.0092     | 0.4516  | 0.4355  | 0.0993          | 0.5076           |

---

# Example

To elaborate on how our evaluation system works, let us take the first question from the previous table:

Question: What is Rule 10b5-1 Trading Plans?

## Part I

The first part is retrieving all individual claims from each response:

Total Number of claims in Golden response: 6

1. : ‘Rule 10b5-1 Trading Plans is a written document.’,

2. : “Rule 10b5-1 Trading Plans preestablish the amounts of future purchases or sales of the Company’s stock.”

3. : “Rule 10b5-1 Trading Plans preestablish the prices of future purchases or sales of the Company’s stock.”

4. : “Rule 10b5-1 Trading Plans preestablish the dates of future purchases or sales of the Company’s stock.”

5. : “Rule 10b5-1 Trading Plans include a formula for determining the amounts, prices, and dates of future purchases or sales of the Company’s stock.”

6. : “Rule 10b5-1 Trading Plans include sales of shares acquired under the Company’s employee and director equity plans.”

Total Number of claims in generated response: 6

1. : ‘Rule 10b5-1 Trading Plans refer to written documents.’

2. : “Rule 10b5-1 Trading Plans preestablish the amounts of future purchases or sales of the Company’s stock.”

3. : “Rule 10b5-1 Trading Plans preestablish the prices of future purchases or sales of the Company’s stock.”

4. : “Rule 10b5-1 Trading Plans preestablish the dates of future purchases or sales of the Company’s stock.”

5. : “Rule 10b5-1 Trading Plans include a formula for determining the amounts, prices, and dates of future purchases or sales of the Company’s stock.”

6. : “Rule 10b5-1 Trading Plans include sales of shares acquired under the Company’s employee and director equity plans.”

Total Number of common claims: 6

1. : ‘Rule 10b5-1 Trading Plans is a written document.’

2. : “Rule 10b5-1 Trading Plans preestablish the amounts of future purchases or sales of the Company’s stock.”

3. : “Rule 10b5-1 Trading Plans preestablish the prices of future purchases or sales of the Company’s stock.”

4. : “Rule 10b5-1 Trading Plans preestablish the dates of future purchases or sales of the Company’s stock.”

5. : “Rule 10b5-1 Trading Plans include a formula for determining the amounts, prices, and dates of future purchases or sales of the Company’s stock.”

6. : “Rule 10b5-1 Trading Plans include sales of shares acquired under the Company’s employee and director equity plans.”

## Part II

Calculate the metrics based on the total claims retrieved in the previous step.

- Precision: Number of common claims / Number of claims in Dataworkz response i.e. 6/6 = 1.
- Recall: Number of common claims / Number of claims in Golden response i.e. 6/6 = 1.
- F1 Score: Harmonic mean of precision and recall i.e. 1.

---

# Final Thoughts

The results demonstrate that the evaluation framework comprehensively measures any QA system against existing benchmarks. By leveraging LLM-as-a-Judge, this framework surpasses standard evaluation metrics like BLEU, ROUGE, and BERTScore, offering a more nuanced and accurate assessment of QA system performance. It is evident that traditional metrics like BLEU, ROUGE and BertScore have certain limitations for evaluating QnA systems and LLM-as-a-Judge can be utilized for generating qualitative and contextual metrics.

---

# References

- [FinQABench](https://huggingface.co/datasets/lighthouzai/finqabench)
- [Apple 10-K Filing 2022](https://investor.apple.com/sec-filings/default.aspx)
- BLEU Score
  - [Wikipedia](https://en.wikipedia.org/wiki/BLEU)
  - [Bleu Metrics](https://medium.com/nlplanet/two-minutes-nlp-learn-the-bleu-metric-by-examples-df015ca73a86)
- ROUGE Score
  - [Wikipedia](<https://en.wikipedia.org/wiki/ROUGE_(metric)>)
  - [ROUGE Metrics](https://medium.com/nlplanet/two-minutes-nlp-learn-the-rouge-metric-by-examples-f179cc285499)
- BERT Score
  - [Paper](https://arxiv.org/abs/1904.09675)
  - [Bert Score](https://medium.com/@abonia/bertscore-explained-in-5-minutes-0b98553bfb71)
- [Using LLMs for Evaluation](https://open.substack.com/pub/cameronrwolfe/p/llm-as-a-judge?r=35moeb&utm_campaign=post&utm_medium=web)
- [Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena](https://arxiv.org/abs/2306.05685)
