# Projects for Fall 2025

## 1. Meditron

- **MultiMeditron**

  This project is about making Meditron multimodal: the user can provide Meditron with medical images, in addition to text. Work is two-fold: adapting the codebase of Meditron to make it have a multimodal architecture, and making the "expert" models that process the images and make embeddings fed to Meditron.

  Contact: Michael Zhang (michael.zhang@epfl.ch)

- **Fine-tuning multimodal models for the medical use**

  This project aims to fine-tune generalist SOTA multimodal models (Qwen3 Omni, Llava, Llama4,...) with our medical multimodal data mixture. The goal is to build the best open-weights medical multimodal model according to the standard benchmark

  Contact: Michael Zhang (michael.zhang@epfl.ch)

- **Meditron Reasoning**

  This project aims to improve our training pipeline by integrating novel reinforcement learning approaches, notably using GRPO algorithms. This is the continuation of a previous project conducted in this area, and we plan to expand the existing work to enhance our project performances (add MultiMeditron for multi-modal reasoning).

  Contact: Guillaume Boyé (guillaume.boye@epfl.ch)

- **Polyglot Meditron**

  Speaking English is nice, most content online is in English. Having a performant LLM for medical tasks formulated in English is useful. But not enough! In low-resource settings and even in most places of the globe, people usually prefer using their first language rather than English.

  This project aims at making Meditron models more proficient in other languages, with a focus on low-resource languages. Work is needed, since having a polyglot base model is generally not enough: popular models do not have a focus on low-resource languages, and there is also a need to make sure to teach the model non-English medical terminology.

  Contact: Fabrice Nemo (fabrice.nemo@epfl.ch)

- **Giving Meditron a voice**

  There are many people around the world who even though cannot read, seek healthcare information and guidance. Currently, medical LLMs, even those that are multi-modal, are usually constrained to a few languages thereby limiting their application in this particular use-case of healthcare question answering. The main objective of this project is to extend the multi-lingual speech capabilities of our Meditron model to ensure that it is more accessible to people around the world.

  Contact: David Sasu (david.sasu@epfl.ch)

- **NeuroMeditron**

  NeuroMeditron develops robust multimodal models for dementia prediction using voice and typing dynamics from the mPower dataset. The project focuses on handling missing modalities through advanced fusion strategies, enabling reliable patient-level monitoring. A proof-of-concept “Neuro Expert” adapter will integrate these digital biomarkers into MultiMeditron.

  Contact: Arianna Francesconi (arianna.francesconi@epfl.ch)

## 2. MMORE

  MMORE stands for Massive Multimodal Open RAG & Extraction, it is our Python library for a scalable multimodal pipeline for processing, indexing, and querying multimodal documents.

  [GitHub repo](https://github.com/swiss-ai/mmore)

  Contact: Fabrice Nemo (fabrice.nemo@epfl.ch)

## 3. Moove

  The [moove](https://jointhemoove.org) is a collaborative platform where experts and communities co-design and validate AI models. The initiative focuses on aligning large language models with real-world standards, ensuring they are transparent, safe, and context-aware. It is already partnered with institutions such as CHUV, ICRC, the Gates Foundation and many hospitals around the world.

  If you want to help us make the moove even greater, don't hesitate to join!

  Note that the project is software-engineering focused.

Contact: Bryan Gotti (bryan.gotti@epfl.ch)

## 4. HIC-Lab AI Bootcamp

Very cool project about teaching the basics of AI applied to healthcare. The target audience is healthcare workers and computer scientists in Rwanda. Our work in LiGHT is to improve the content of the bootcamp so that students learn better, and mentor students there, guide them throughout their completion of the bootcamp.

Contact: Fabrice Nemo (fabrice.nemo@epfl.ch)

## 5. CHIT-CHAT
1. Embedding Humanitarian Principles in LLM Development

LLMs are usually not deployed for humanitarian applications since they are not intentionally designed to align to humanitarian values. This project therefore aims to develop a framework / checklist for LLM development and evaluation that can be applied in the creation and testing of Humanitarian-focused LLMs.

Contact: David Sasu (david.sasu@epfl.ch)

## 6. PRISM-AI

PRISM-AI leverages the PRISM dataset on pregnancy reference intervals to benchmark traditional ML/DL models against Large Language Models for risk prediction in maternal health. The project explores fine-tuning strategies and novel optimization methods (e.g., DPO/GRPO) to assess whether LLMs can provide clinically meaningful improvements over established approaches.

Contact: Arianna Francesconi (arianna.francesconi@epfl.ch)

## 7. Multimodal Learning from Voice and Keyboard Dynamics for Early Alzheimer’s Diagnosis

This project develops deep learning model to detect early Alzheimer’s disease from typing and voice signals. Students will design a multimodal models (RNNs for typing and CNN/ViT for voice) to capture motor and speech patterns linked to cognitive decline, comparing modality contributions and model interpretability.

Contact: Arianna Francesconi (arianna.francesconi@epfl.ch)

## 8. Cross-Disease Voice Prognosis: Parkinson and ALS Audio Modeling

Voice changes are early markers of neurodegenerative diseases. This project trains deep learning models on Parkinson’s voice recordings (mPower) and tests cross-disease generalization on ALS speech data, exploring transfer learning and shared vocal biomarkers across disorders.

Contact: Arianna Francesconi (arianna.francesconi@epfl.ch)

## 9. Balancing Time-Series Health Data Across Diseases

This project extends the [IMBALMED method](https://www.sciencedirect.com/science/article/pii/S0895611125000382) for class balancing in time-series models (LSTM/GRU) and benchmarks it against standard techniques such as SMOTE or focal loss. Students will analyze cross-disease robustness and ensemble diversity, building a reproducible benchmark for temporal health data.

Contact: Arianna Francesconi (arianna.francesconi@epfl.ch)

