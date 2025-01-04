
# Improving Language Understanding by Generative Pre-Training

This repository contains a concise overview of the paper **"Improving Language Understanding by Generative Pre-Training"** by Alec Radford, Karthik Narasimhan, Tim Salimans, and Ilya Sutskever (OpenAI). The paper explores a semi-supervised approach for natural language understanding (NLU) tasks by combining **unsupervised pre-training** with **supervised fine-tuning**. This method significantly improves performance across various benchmarks, setting new state-of-the-art results.

---

## Key Contributions

1. **Two-Stage Training Framework**:
   - **Unsupervised Pre-Training**: A language model is trained on a large corpus of unlabeled text using a standard language modeling objective.
   - **Supervised Fine-Tuning**: The pre-trained model is fine-tuned on task-specific datasets with minimal architecture changes.

2. **Model Architecture**:
   - Uses a **Transformer-based decoder** for pre-training, leveraging its capability to handle long-range dependencies.
   - Incorporates task-specific input transformations to adapt structured inputs (e.g., text pairs) to the model.

3. **Generalization and Performance**:
   - Achieves state-of-the-art results on 9 out of 12 NLU tasks, including:
     - **Commonsense Reasoning** (e.g., Story Cloze Test): +8.9%
     - **Question Answering** (e.g., RACE): +5.7%
     - **Textual Entailment** (e.g., MultiNLI): +1.5%
   - Performs effectively on datasets of varying sizes, showcasing robust generalization.

---

## Framework Overview

### 1. **Unsupervised Pre-Training**
- Objective: Maximize the likelihood of token sequences using a multi-layer Transformer.
- Dataset: BooksCorpus (7,000+ unpublished books with long contiguous text).
- Highlights:
  - Handles long-range contexts.
  - Achieves low perplexity (18.4 on BooksCorpus).

### 2. **Supervised Fine-Tuning**
- Adapts the pre-trained model for specific tasks.
- Adds task-specific linear output layers and input transformations.
- Includes auxiliary language modeling objectives to improve generalization and convergence.

### 3. **Task-Specific Input Transformations**
- Textual Entailment: Concatenate premise and hypothesis with a delimiter.
- Semantic Similarity: Evaluate both sentence orderings and combine their representations.
- Question Answering: Encode context, question, and answer candidates into structured sequences.

---

## Experimental Results

| **Task**                  | **Dataset**        | **Improvement** |
|---------------------------|--------------------|-----------------|
| Commonsense Reasoning     | Story Cloze Test   | +8.9%           |
| Question Answering        | RACE               | +5.7%           |
| Textual Entailment        | MultiNLI           | +1.5%           |
| Semantic Similarity       | STS-Benchmark      | +1.0%           |
| Classification            | CoLA (Linguistic)  | +10.4%          |

---

## Why It Matters
- Reduces reliance on large labeled datasets by leveraging unlabeled data.
- Simplifies model adaptation to diverse tasks with task-agnostic pre-training.
- Paves the way for effective use of Transformers in semi-supervised learning.

---

## Reference

Radford, A., Narasimhan, K., Salimans, T., & Sutskever, I. (2018). Improving Language Understanding by Generative Pre-Training. [arXiv Preprint](https://arxiv.org/abs/1801.00001).
