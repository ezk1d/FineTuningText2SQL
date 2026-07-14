# Text-to-SQL Fine-Tuning with QLoRA

Fine-tuning a Large Language Model (LLM) to translate natural language questions into executable SQLite queries using QLoRA and the BIRD benchmark.

---

## Overview

Text-to-SQL enables users to query relational databases using natural language instead of writing SQL manually. While modern LLMs demonstrate strong SQL generation capabilities, they often struggle with schema grounding, hallucinated columns, and database-specific reasoning.

This project investigates whether parameter-efficient fine-tuning (QLoRA) can improve the performance of a pretrained language model on the Text-to-SQL task.

The project includes:

- Fine-tuning Qwen3-4B using QLoRA
- Automated execution-based evaluation
- SQL error analysis
- Comparison between the base and fine-tuned models

---

## Features

- QLoRA fine-tuning using PEFT
- 4-bit quantization with BitsAndBytes
- Execution Accuracy evaluation
- Exact SQL Match evaluation
- SQLite query execution
- SQL timeout handling
- Error categorization
- Automatic schema formatting
- Hugging Face Transformers + TRL training pipeline

---

## Technologies

- Python
- PyTorch
- Hugging Face Transformers
- TRL
- PEFT (LoRA / QLoRA)
- BitsAndBytes
- SQLite
- Pandas
- NumPy

---

## Dataset

This project uses the **BIRD (BIg Bench for Relational Database)** benchmark.

Each example contains:

- Natural language question
- Database schema
- Evidence
- Ground-truth SQL
- SQLite database

The model must generate SQL that executes correctly on the provided database.

---

## Methodology

### 1. Baseline Evaluation

The pretrained Qwen3-4B model was first evaluated on the BIRD development set.

For each sample:

1. Format the prompt
2. Generate SQL
3. Execute generated SQL
4. Execute reference SQL
5. Compare execution results
6. Categorize SQL errors

---

### 2. Fine-Tuning

The model was fine-tuned using:

- QLoRA
- 4-bit quantization
- LoRA adapters
- TRL SFTTrainer

Training focused on learning SQL generation while updating only a small fraction of model parameters.

---

### 3. Evaluation

Performance was measured using:

- Execution Accuracy
- Exact SQL Match
- SQL Execution Errors

SQL errors were categorized into:

- Syntax Errors
- Missing Columns
- SQL Misuse
- Query Timeouts

---

## Results

### Base Model

| Metric | Value |
|---------|-------|
| Execution Accuracy | **45.8%** |
| Exact SQL Match | **19.0%** |
| SQL Errors | **30.0%** |

Error Breakdown:

| Error | Count |
|--------|------:|
| Syntax | 7 |
| Missing Column | 121 |
| Misuse | 4 |
| Timeout | 12 |

---

### Fine-Tuned Model

| Metric | Value |
|---------|-------|
| Execution Accuracy | **46.6%** |
| Exact SQL Match | **19.4%** |
| SQL Errors | **27.8%** |

Error Breakdown:

| Error | Count |
|--------|------:|
| Syntax | 7 |
| Missing Column | 122 |
| Misuse | 4 |
| Timeout | 0 |

---

## Discussion

QLoRA fine-tuning produced modest improvements in execution accuracy while reducing the overall SQL execution error rate.

The most notable improvement was the elimination of query timeout failures, indicating that the fine-tuned model generated more reliable and executable SQL.

However, hallucinated schema columns remained the dominant source of failure, suggesting that parameter-efficient fine-tuning alone is insufficient to solve schema grounding.

Future improvements could incorporate Retrieval-Augmented Generation (RAG) to retrieve relevant database schemas before SQL generation.

---

## Project Structure

```
.
├── notebooks/
│   └── text_to_sql.ipynb
├── README.md
└── requirements.txt
```

---

## Future Work

- Evaluate larger instruction-tuned models
- Integrate Retrieval-Augmented Generation (RAG)
- Improve schema grounding
- Compare multiple Text-to-SQL models
- Deploy as a web application using FastAPI and Streamlit

---

## Key Takeaways

- Built an end-to-end Text-to-SQL evaluation pipeline.
- Fine-tuned an LLM using QLoRA.
- Automated SQL execution and error analysis.
- Benchmarked model performance using execution-based metrics.
- Identified schema grounding as the primary limitation of the model.
