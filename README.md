# 🇰🇷 Policy LLM Fine-Tuning with LoRA (Polyglot-ko-1.3B)

이 프로젝트는 한국의 국가 과학기술 정책 언어에 특화된 질의응답 언어모델을 개발하기 위해, Polyglot-ko-1.3B 모델을 기반으로 **LoRA (Low-Rank Adaptation)** 파인튜닝 기법을 적용한 실험입니다. 학습 데이터는 「제3차 국가초고성능컴퓨팅 육성 기본계획 (2023~2027)」 문서를 instruction 형식으로 가공하여 구성되었으며, 파인튜닝 이후 BLEU, ROUGE, 정책 키워드 반영률 등 다양한 관점에서 성능을 평가하였습니다.

---

## 📁 디렉토리 구조

policy-llm-lora-finetuning/
│
├── README.md
├── requirements.txt
│
├── data/
│ ├── raw/ # 원본 정책 문서
│ └── processed/ # 전처리된 instruction 포맷 학습 데이터
│
├── src/
│ ├── prepare_dataset.py # 데이터 전처리 스크립트
│ ├── run_finetune.py # LoRA 기반 파인튜닝 실행 스크립트
│ ├── eval_compare.py # 튜닝 전후 응답 평가 스크립트
│ └── config/ # (옵션) 설정 json 파일 등
│
├── outputs/
│ ├── koni-lora-checkpoint/ # 학습된 adapter 결과
│ └── response_outputs/ # 튜닝 전후 결과 파일
│
└── notebooks/
└── exploratory_analysis.ipynb # 정량 및 정성 평가 노트북

yaml
복사
편집

---

## 🚀 코드 실행 예시

### 1. 의존성 설치
```bash
pip install -r requirements.txt
2. 데이터 전처리
bash
복사
편집
python src/prepare_dataset.py \
  --input data/raw/policy_source.txt \
  --output data/processed/koni_finetune_sntp_sample.csv
3. LoRA 기반 파인튜닝 실행
bash
복사
편집
python src/run_finetune.py \
  --base_model EleutherAI/polyglot-ko-1.3b \
  --data_path data/processed/koni_finetune_sntp_sample.csv \
  --output_dir outputs/koni-lora-checkpoint
4. 튜닝 전후 응답 비교 및 평가
bash
복사
편집
python src/eval_compare.py \
  --questions_file data/processed/eval_questions.txt \
  --model_paths EleutherAI/polyglot-ko-1.3b outputs/koni-lora-checkpoint
🧾 데이터 출처
학습 데이터는 「제3차 국가초고성능컴퓨팅 육성 기본계획(2023~2027)」 원문을 기반으로 수작업으로 instruction 형식 (instruction, input, output)으로 가공하였습니다.

해당 문서는 과학기술정보통신부가 공개한 공공 정책 문서로, 출처는 국가과학기술지식정보서비스(NTIS) 등에서 확인할 수 있습니다.

평가용 질문은 실제 과학기술 정책 영역에서 자주 등장하는 핵심 쟁점을 바탕으로 구성되었습니다.

🧠 사용 모델 정보
항목	정보
기반 모델	EleutherAI/polyglot-ko-1.3b
튜닝 기법	LoRA (Low-Rank Adaptation) with Huggingface PEFT
학습 환경	Google Colab (T4 / A100 GPU 환경)
튜닝 목적	정책 질의응답의 정보 반영력 및 언어 품질 향상
출력 형식	adapter_config.json, adapter_model.safetensors

📊 평가 지표
BLEU / ROUGE-L: 정책 응답 정량적 정확도

정성 평가: 오류 유형 분류 (정보 누락, 정책 기관 오용 등)

정책 키워드 반영률: 핵심 용어 포함 비율

질문 유형별 성능 비교: 법령/기관/산업/추론 등 분류 기준에 따른 성능 변화

📚 참고 문헌
Hu et al. (2021). LoRA: Low-Rank Adaptation of Large Language Models

Caudron et al. (2024). Adaptation of LLMs for the Public Sector

Ren et al. (2025). AT-RAFT for Research Policy Interpretation

Sun et al. (2024). Instruction-Tuning for Policy Text Classification
