# Clinical PHI Named Entity Recognition (NER) with DistilBERT

This repository contains a complete pipeline for fine-tuning a transformer-based model (**DistilBERT**) to detect and extract Protected Health Information (PHI) from clinical text notes in compliance with HIPAA privacy standards.

---

## 📌 Project Overview

Extracting PHI from unstructured medical text (such as clinical notes, discharge summaries, and patient intake forms) is critical for medical data anonymization. This project fine-tunes `distilbert-base-uncased` for token classification using standard BIO tagging conventions across 13 major entity categories (27 label states in total).

### Supported Entities (BIO Tagged)
* **Demographics:** `NAME`, `AGE`, `LOCATION`, `DATE`
* **Contact Information:** `PHONE`, `EMAIL`, `URL`, `IP`
* **Identifiers:** `SSN`, `MRN` (Medical Record Number), `HEALTH_PLAN_ID`, `ACCOUNT_ID`, `LICENSE_ID`

---

## 🛠️ Requirements & Dependencies

Ensure you have the following packages installed:

```bash
pip install torch transformers datasets evaluate seqeval numpy
```

---

## 🚀 Pipeline Workflow

1. **Label Mapping:**
   * Generates a 27-label scheme (`O`, `B-TAG`, `I-TAG`).
   * Configures bidirectional mappings (`LABEL2ID` and `ID2LABEL`).

2. **Model Training:**
   * Architecture: `DistilBertForTokenClassification`
   * Base Model: `distilbert-base-uncased`
   * Evaluation Metric: `seqeval` (overall Precision, Recall, F1, Accuracy)
   * Optimizer & Scheduler: Hugging Face `Trainer` defaults with AdamW ($lr = 3 \times 10^{-5}$)

3. **Inference & Extraction:**
   * Evaluates unseen sample notes and outputs extracted tokens alongside predicted PHI entity tags.

---

## 💻 How to Run

1. Open your Jupyter Notebook or Google Colab environment.
2. Ensure GPU acceleration is enabled (**Runtime -> Change runtime type -> T4 GPU**).
3. Run the data preprocessing and tokenization cells to initialize `train_dataset`, `val_dataset`, and `tokenizer`.
4. Execute the training and evaluation script:

```python
# The model will train for 3 epochs and automatically evaluate metric performance on the validation set.
trainer.train()
```

5. Run the `extract_phi` function to test predictions on new clinical notes:

```python
sample_note = "Patient Jane Doe (DOB: 05/14/1980) visited St. Jude Hospital at 456 Elm Street. Contact: 555-0199."
extracted_entities = extract_phi(sample_note, model, tokenizer)
```

---

## 📊 Sample Inference Output

```text
Sample Note: Patient Jane Doe (DOB: 05/14/1980) visited St. Jude Hospital at 456 Elm Street. Contact: 555-0199.

Detected PHI Entities:
 - jane            : B-NAME
 - doe             : I-NAME
 - 05              : B-DATE
 - /               : I-DATE
 - 14              : I-DATE
 - /               : I-DATE
 - 1980            : I-DATE
 - st              : B-LOCATION
 - .               : I-LOCATION
 - jude            : I-LOCATION
 - hospital        : I-LOCATION
 - 456             : B-LOCATION
 - elm             : I-LOCATION
 - street          : I-LOCATION
 - 555             : B-PHONE
 - -               : I-PHONE
 - 0199            : I-PHONE
```

---

## 📄 License & Compliance

This project is built for educational and research purposes. Ensure all data handled follows appropriate health data privacy regulations (HIPAA/GDPR) when deploying on non-synthetic data.