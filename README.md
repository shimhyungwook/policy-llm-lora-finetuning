# 🇰🇷 Fine-Tuning a Korean Policy Domain LLM with LoRA (Polyglot-ko-1.3B)

This project fine-tunes a Korean language model for national science and technology policy question answering (QA) tasks using **LoRA (Low-Rank Adaptation)**. We utilize the publicly available base model `EleutherAI/polyglot-ko-1.3b` and train it on instruction-style data derived from an official government policy document. Evaluation includes BLEU/ROUGE metrics, domain-specific keyword coverage, and qualitative error analysis.

---

## 📁 Project Structure

policy-llm-lora-finetuning/
│
├── README.md
├── requirements.txt
│
├── data/
│ ├── raw/ # Original policy text source
│ └── processed/ # Instruction-format training data
│
├── src/
│ ├── prepare_dataset.py # Data preprocessing script
│ ├── run_finetune.py # LoRA fine-tuning training script
│ ├── eval_compare.py # Evaluation: pre/post-tuning comparison
│ └── config/ # (Optional) config files
│
├── outputs/
│ ├── koni-lora-checkpoint/ # Trained LoRA adapter results
│ └── response_outputs/ # Model answer outputs before/after tuning
│
└── notebooks/
└── exploratory_analysis.ipynb # Metric comparison and QA analysis

yaml
복사
편집

---

## 🚀 How to Run

### 1. Install dependencies
```bash
pip install -r requirements.txt
2. Preprocess raw policy data
bash
복사
편집
python src/prepare_dataset.py \
  --input data/raw/policy_source.txt \
  --output data/processed/koni_finetune_sntp_sample.csv
3. Run LoRA fine-tuning
bash
복사
편집
python src/run_finetune.py \
  --base_model EleutherAI/polyglot-ko-1.3b \
  --data_path data/processed/koni_finetune_sntp_sample.csv \
  --output_dir outputs/koni-lora-checkpoint
4. Compare QA results (before/after fine-tuning)
bash
복사
편집
python src/eval_compare.py \
  --questions_file data/processed/eval_questions.txt \
  --model_paths EleutherAI/polyglot-ko-1.3b outputs/koni-lora-checkpoint
🧾 Data Source
The training data is derived from the 3rd National Basic Plan for Supercomputing Development in Korea (2023–2027).

Original source: Ministry of Science and ICT (MSIT), Korea. Available via public policy portals such as NTIS.

Instruction-format dataset was created manually with structured instruction, input, and output fields.

Evaluation questions reflect key issues and topics frequently discussed in Korean science and technology policy.

🧠 Model Details
Component	Information
Base Model	EleutherAI/polyglot-ko-1.3b
Tuning Method	LoRA (Low-Rank Adaptation) using Huggingface PEFT
Training Environment	Google Colab (GPU T4 / A100)
Output Format	adapter_config.json, adapter_model.safetensors
Objective	Enhance QA relevance and policy-specific fluency for Korean government text

📊 Evaluation Criteria
BLEU / ROUGE-L: Quantitative similarity to reference answers

Qualitative Error Typology: Information omission, incorrect institutions, incomplete reasoning

Policy Keyword Coverage: Ratio of domain-specific terms in generated responses

By Question Type: Performance categorized by law, institution, policy concept, inference, basic/applied research

📚 References
Hu et al. (2021). LoRA: Low-Rank Adaptation of Large Language Models

Caudron et al. (2024). Adaptation of LLMs for the Public Sector

Ren et al. (2025). AT-RAFT: Retrieval-Augmented Fine-Tuning for Research Policy Interpretation

Sun et al. (2024). Instruction-Tuned Policy Text Classification

🔓 License
MIT License

This repository is provided for academic research and reproducibility. For any commercial usage or adaptation of the model and data, please contact the author.
