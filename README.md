# Dialogue Summarization Using a BERT Encoder-Decoder Architecture

## Project Overview

Modern messaging platforms generate large volumes of conversational data, making it difficult for users to quickly catch up on missed discussions. Important decisions, action items, and updates can become buried in lengthy group chats, creating information overload and reducing user engagement.

This project develops a proof-of-concept dialogue summarization system using the SAMSum dataset and a BERT-based encoder-decoder architecture. The goal is to automatically generate concise summaries that capture the essential information from multi-speaker conversations.

## Business Problem

Acme Communications is exploring AI-powered features to improve the user experience within its messaging platform.

### Challenges

- Users struggle to catch up on long conversations.
- Important information is often buried in message history.
- Information overload reduces engagement and satisfaction.
- Manually reviewing conversations is time-consuming.

### Proposed Solution

Develop an automated dialogue summarization system capable of generating concise summaries from conversational text, allowing users to quickly understand key discussion points without reading every message.

## Dataset

This project uses the SAMSum dataset, a benchmark dataset for dialogue summarization consisting of messenger-style conversations paired with human-written summaries.

Dataset statistics:


|Split | Examples |
|---------|----------|
| Train  | 14,732  |
| Validation  | 818  |
| Test  | 819  |

The final model was trained on approximately 12,000 dialogue-summary pairs (~81% of the available training data).

## Model Architecture

The model uses a transformer-based encoder-decoder architecture implemented with Hugging Face's `EncoderDecoderModel`.

### Encoder

- Model: `bert-base-uncased`
- Purpose: Encode dialogue conversations into contextual representations.

### Decoder

- Model: `bert-base-uncased`
- Purpose: Generate abstractive summaries from encoded dialogue representations.

### Architecture

Dialogue

↓

Tokenizer

↓

BERT Encoder

↓

Contextual Representations

↓

BERT Decoder

↓

Generated Summary

## Training Configuration

### Final Model Parameters

|Parameter | Value |
|----------|-------|
| Training Examples  | 12,000  |
| Validation Examples  | 818  |
| Test Examples  | 400  |
| Max Input Length  | 256  |
| Max Target Length  | 64  |
| Batch Size  | 4  |
| Epochs  | 5  |
| Learning Rate  | 3e-5  |

### Optimization Techniques

- AdamW optimizer
- Learning rate scheduling
- Gradient clipping
- Early stopping
- Model checkpointing
- Beam search decoding

Generation parameters:

```python
num_beams = 4
min_length = 8
max_length = 64
no_repeat_ngram_size = 3
length_penalty = 2.0
early_stopping = True
```
## Experimental Results

Model performance improved significantly as additional training data was introduced.

### ROUGE Score Progression

| Experiment	| Train Size	| ROUGE-1	| ROUGE-2	| ROUGE-L |
|---------------|---------------|-----------|-----------|---------|
| Initial Model	| 1,000	        | 0.146	    | 0.016    	| 0.114   |
| Intermediate Model |	3,000 |	0.197 |	0.036 |	0.159 |
| Expanded Training	| 4,000	| 0.268	| 0.071	| 0.216 |
| Final Model |	12,000 |	0.376 |	0.126 |	0.301 |

### Final Results

|Metric | Score |
|----------|-------|
| ROUGE-1  | 0.376  |
| ROUGE-2  | 0.126  |
| ROUGE-L  | 0.301  |
| ROUGE-Lsum  | 0.301  |

## Evaluation and Analysis

### Strengths

- Successfully summarizes multi-speaker conversations.
- Learns important dialogue patterns.
- Produces concise summaries from lengthy conversations.
- Performance improved substantially with additional training data.

### Observed Limitations

Qualitative analysis revealed several common issues:

- Repetition in generated summaries.
- Hallucinated information not present in the dialogue.
- Speaker attribution errors.
- Missing important details in some conversations.

These limitations are partly due to adapting BERT, which was originally designed as an encoder-only architecture, for generative summarization.

## Key Findings

The most important finding from this project was that increasing the amount of training data produced larger performance gains than simply increasing the number of training epochs.

Performance improved consistently as the training set increased from 1,000 to 12,000 examples, demonstrating the importance of dataset size for transformer-based summarization tasks.

## Business Impact

The results demonstrate the feasibility of automated dialogue summarization for messaging platforms.

Potential benefits include:

- Reduced information overload.
- Faster conversation catch-up.
- Improved user engagement.
- Better accessibility of group discussions.
- Foundation for future AI-powered messaging features.

Although the model is not production-ready, it successfully demonstrates the value of conversation summarization as a product capability.

## Future Improvements

Potential future enhancements include:

- Training on the full SAMSum dataset.
- Hyperparameter optimization.
- Human evaluation of summary quality.
- Factual consistency verification.
- Action-item extraction.
- Migration to summarization-specific architectures such as:
  - BART
  - T5
  - PEGASUS
- Deployment as a REST API or messaging platform feature.

## References

- SAMSum Corpus: A Human-Annotated Dialogue Dataset for Abstractive Summarization
- Hugging Face Transformers Documentation
- Hugging Face Datasets Documentation
- BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding


