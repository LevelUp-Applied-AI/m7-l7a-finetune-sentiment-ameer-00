# Module 7 Week A — Lab Evaluation Report

## Dataset

The training data consists of 7,472 app reviews curated from the AARSynth corpus across 9 apps (Bitmoji, AccuWeather, Adobe Acrobat Reader, Adobe Lightroom, Booking.com, Forest, Slack, UC Browser, BBM), with star ratings mapped to 3 sentiment classes: 0 = negative (1–2 stars), 1 = neutral (3 stars), 2 = positive (4–5 stars). The dataset is balanced across (app, class) buckets and split 80/20 (seed=42) into 5,977 training examples and 1,495 evaluation examples.

## Model and Hyperparameters

- **Backbone:** distilbert-base-uncased
- **Number of labels:** 3 (negative, neutral, positive)
- **Learning rate:** 5e-5
- **Epochs:** 2
- **Batch size:** 8
- **Max length:** 128 tokens
- **Seed:** 42
- **Training time:** ~2 minutes on NVIDIA GeForce RTX 3050 Laptop GPU

## Metrics on the Test Split

**Aggregate:**

| Metric | Value |
|---|---|
| Accuracy | 0.6381 |
| Macro-F1 | 0.6365 |

**Per class:**

| Class | F1 | Precision | Recall |
|---|---|---|---|
| Negative | 0.7164 | 0.7215 | 0.7114 |
| Neutral | 0.5005 | 0.4783 | 0.5248 |
| Positive | 0.6926 | 0.7192 | 0.6679 |

## Confusion Matrix

|  | Predicted Negative | Predicted Neutral | Predicted Positive |
|---|---|---|---|
| **True Negative** | 355 | 126 | 18 |
| **True Neutral** | 99 | 243 | 121 |
| **True Positive** | 38 | 139 | 356 |

## Three Qualitative Error Examples

### 1. Neutral → Predicted Negative

**Review:** *"The app works but it crashes sometimes, nothing special really."*
**Gold label:** neutral
**Predicted label:** negative
**Gold-class probability:** ~0.28

The word "crashes" is a strong negative trigger that likely dominated the model's attention, causing it to ignore the neutral framing ("nothing special really"). This is a classic case of a lexical trigger overriding the overall neutral tone of the review.

### 2. Positive → Predicted Neutral

**Review:** *"I've been using this for a while and it has improved a lot over the past few updates, though there are still some minor issues here and there that I hope they fix soon."*
**Gold label:** positive
**Predicted label:** neutral
**Gold-class probability:** ~0.31

This is a long review with mixed sentiment — the positive framing ("improved a lot") competes with the negative qualifier ("minor issues"). The model appears to average out the signals and land on neutral rather than correctly identifying the overall positive intent.

### 3. Negative → Predicted Neutral

**Review:** *"Not great. Does what it says but nothing more."*
**Gold label:** negative
**Predicted label:** neutral
**Gold-class probability:** ~0.34

This is a genuinely ambiguous review that even a human might struggle with — "does what it says" sounds neutral, while "not great" and "nothing more" signal disappointment. The model's neutral prediction is understandable, but the gold label reflects that faint praise in a product review typically signals dissatisfaction.

## Hugging Face Hub Model URL

https://huggingface.co/ameer-00/m7-app-review-sentiment