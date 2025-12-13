# Bardify
## Setup and Running the Notebook

### Requirements
- Google Colab (GPU runtime)
- Python ≥ 3.10
- Hugging Face account (required for gated models like Llama-3.1)

### Quick Start (Colab)
1. Open the main notebook in Google Colab
2. In Colab: Change runtime type to GPU
3. Run all cells from top to bottom

### Dependencies
All dependencies are installed inside the notebook:
- transformers
- datasets
- peft
- accelerate
- bitsandbytes
- scikit-learn

### Data
The Shakespeare sonnets corpus is expected at:
/content/drive/MyDrive/bardify/shakespeares-sonnets_TXT_FolgerShakespeare.txt

### Model Access
This project uses `meta-llama/Llama-3.1-8B`, which is a gated Hugging Face model
You must:
1. Request access on the model page
2. Log in with a Hugging Face token inside Colab

### Outputs
- Trained LoRA adapters are saved to /content/bardify_final_model/lora
- The notebook prints evaluation loss and perplexity (PPL)
- Example inference generates a 14-line Shakespearean sonnet

### Architecture and Pipeline

The project fine-tunes a large language model to generate Shakespearean sonnets using parameter-efficient fine-tuning (LoRA)

1. **Data Preparation**
   The Folger Shakespeare sonnets corpus is cleaned by removing front/back matter, normalising whitespace, and unifying punctuation.
   Each sonnet is split and stored as plain text. A 10-fold cross-validation split is created, producing train/validation files per fold

2. **Tokenisation and Chunking**
   Text is tokenised with the base model tokeniser. Sonnets are truncated to a fixed block size (512 tokens) and appended with an end-of-sequence token

3. **Model and Fine-Tuning**
   The base model `Llama-3.1-8B` is loaded in 4-bit precision (QLoRA) to reduce memory usage.
   Low-Rank Adaptation (LoRA) is applied to the attention projection layers `q_proj`, `k_proj`, `v_proj`, `o_proj`, training only a small number of additional parameters while keeping the base model frozen

4. **Training and Evaluation**
   Training is performed using Hugging Face `Trainer` with gradient accumulation and cosine learning-rate scheduling.
   For each fold, evaluation loss is measured on the validation split and converted to perplexity (PPL)

5. **Inference**
   After training, the LoRA adapters are loaded on top of the frozen base model.
   Given a topic prompt, the model generates a 14-line Shakespearean sonnet using controlled sampling (temperature, top-p)