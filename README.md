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
- Example inference generates a 14-line Shakespearean sonnet.