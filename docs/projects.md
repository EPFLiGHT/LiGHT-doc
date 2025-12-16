# Projects for Fall 2025

<div class="grid cards" markdown>

- :material-text-box-search-outline: __MMORE__

    ---

    [MMORE](https://github.com/swiss-ai/mmore) is our Python library for a scalable multimodal pipeline for *processing*, *indexing*, and *querying* multimodal documents. It is used for *retrieval augmented generation* (RAG) applications.

    [:octicons-arrow-right-24: See MMORE information](#mmore-mirage)

- :material-palm-tree:{ .lg .middle } __MIRAGE__

    ---

    [MIRAGE](https://github.com/EPFLiGHT/MIRAGE) is a platform designed to streamline the processing of datasets using generative models.

    [:octicons-arrow-right-24: See MIRAGE information](#mmore-mirage)

- :fontawesome-solid-language:{ .lg .middle } __Polyglot__

    ---

    *Polyglot Meditron* is a project aimed at evaluating and enhancing the multilingual capabilities of our Meditron model.

    [:octicons-arrow-right-24: See Multilingual Meditron information](#polyglot-meditron)

- :material-human-male-board-poll:{ .lg .middle } __LiGHT Bootcamp__

    ---

    Improve the content of the MOOC on *AI applied to healthcare*

    [:octicons-arrow-right-24: See LiGHT Bootcamp information](#light-ai-bootcamp)

- :material-file-image-plus:{ .lg .middle } __MultiMeditron__

    ---

    Improve Meditron's multimodal capabilities by enabling it to process and understand multiple modalities.

    [:octicons-arrow-right-24: See MultiMeditron information](#multimeditron)

- :material-cpu-32-bit:{ .lg .middle } __Quantisation of Medical LLMs__

    ---

    Explore model quantisation in practice and document the results in a reproducible way.

    [:octicons-arrow-right-24: See Quantisation of Medical LLMs information](#quantisation-of-medical-llms)

- :material-bottle-tonic:{ .lg .middle } __Distillation of Medical LLMs__

    ---

    Explore knowledge distillation for language models, with an emphasis on comparing different distillation strategies, data choices, and model architectures.

    [:octicons-arrow-right-24: See Distillation of Medical LLMs information](#distillation-of-medical-llms)

</div>

## MMORE & Mirage

*This project is supervised by Fabrice Nemo*

[MMORE](https://github.com/swiss-ai/mmore) stands for Massive Multimodal Open RAG & Extraction, it is our Python library for a scalable multimodal pipeline for processing, indexing, and querying multimodal documents.
[MIRAGE](https://github.com/EPFLiGHT/MIRAGE) stands for Multimodal Intelligent Reformatting and Augmentation Generation Engine, it is our advanced platform designed to streamline the processing of datasets using generative models.

The aim of these two projects is to work on maintaining the library: solve the issues raised by the community, fix bugs, make new features that could be useful and challenging for students (would be suggested by students or by Fabrice if the idea comes up as important enough).

## Polyglot Meditron

*This project is supervised by Fabrice Nemo*

Speaking English is nice, most content online is in English. Having a performant LLM for medical tasks formulated in English is useful. But not enough! In low-resource settings and even in most places of the globe, people usually prefer using their first language rather than English.

This project aims at making Meditron models more proficient in other languages, with a focus on low-resource languages (current focus on Amharic, Hindi, Swahili, Tamil, eventually also Arabic, Bembe, French, Kinyarwanda, Luo, Nyanja, Twi, Urdu). In written and spoken speech. Work is needed, since having a polyglot base model is generally not enough: popular models do not have a focus on low-resource languages, and there is also a need to make sure to teach the model non-English medical terminology.

## LiGHT AI Bootcamp

*This project is supervised by Fabrice Nemo*

Teaching the basics of AI applied to healthcare. We already have a MOOC (almost) ready. The target audience is healthcare workers and computer scientists in Africa, who would be following the MOOC with human mentoring provided by LiGHT. Our work in LiGHT is to improve the content of the MOOC so that students learn better, and mentor students in Africa, guide them throughout their completion of the bootcamp.
Students may work on this project either as a side project (1 or 2 hours per week of mentoring) or as a full time semester/optional project (for instance for making deeper research on how to improve the MOOC with evidence from educational science, for developing more evaluation content… To be discussed with Fabrice).

## MultiMeditron

*This project is co-supervised by David Sasu, Lars Klein, Frabrice Nemo and Arianna Francesconi*

This project aimed at improving Meditron __multimodal capabilities__. Healthcare data is often multimodal, combining text, images, signals, and other data types. Enabling Meditron to process and understand multiple modalities can significantly enhance its performance in medical applications.

The goal of this project is:

- Adapting the codebase of Meditron to make it have a multimodal architecture, adapted to new modalities (for now the codebase only supports images)
- Making and improving the "expert" models that process the modalities and make embeddings fed to Meditron.

## Quantisation of Medical LLMs

*This project is supervised by Lars Klein*

This project focuses on exploring model quantisation in practice and documenting the results in a reproducible way. The goal is to gain hands-on experience with commonly used quantisation tools and to produce clear notes and artifacts that capture their behavior, trade-offs, and performance characteristics.

__Tasks:__

- Select and evaluate 1-3 quantisation tools (e.g. __bitsandbytes__, __llama.cpp__)
- Apply quantisation to one or more models and document the process in detail, including:
  - exact steps taken,
  - runtime of the quantisation process,
  - resulting model size and size reduction.
- Run the quantised models through benchmarks and record:
  - inference speed,
  - resource usage,
  - any observable changes in output quality or behavior.

__Required Experience:__

- Basic __Python__ programming
- Familiarity with running machine learning models from the command line or in scripts

## Distillation of Medical LLMs

*This project is supervised by Lars Klein and Arianna Francesconi*

This project explores knowledge distillation for language models, with an emphasis on comparing different distillation strategies, data choices, and model architectures. The aim is to better understand how teacher selection, loss functions, and training data affect the performance and efficiency of distilled student models.

__Tasks:__

- Identify suitable teacher models and, if needed, construct datasets for intermediate representations (e.g. activations).
- Implement and compare different distillation losses, including:
  - logit-based distillation,
  - MiniLM-style objectives
- Experiment with different training datasets, such as:
  - general-purpose corpora,
  - task-specific datasets.
- Benchmark distilled models on relevant evaluation tasks and performance metrics.
- Explore distillation across architectures, including heterogeneous setups (e.g. LFM-style distillation between models such as Apertus and Meditron).

__Required Experience:__

- Solid Python programming
- Familiarity with training and evaluating neural networks
- Basic understanding of language models and knowledge distillation techniques
<!-- ## 1. Meditron

- **MultiMeditron**

  This project is about making Meditron multimodal: the user can provide Meditron with medical images, in addition to text. Work is two-fold: adapting the codebase of Meditron to make it have a multimodal architecture, and making the "expert" models that process the images and make embeddings fed to Meditron.

  Contact: Michael Zhang (michael.zhang@epfl.ch)

- **Fine-tuning multimodal models for the medical use**

  This project aims to fine-tune generalist SOTA multimodal models (Qwen3 Omni, Llava, Llama4,...) with our medical multimodal data mixture. The goal is to build the best open-weights medical multimodal model according to the standard benchmark

  Contact: Michael Zhang (michael.zhang@epfl.ch)

- **Meditron Reasoning**

  This project aims to improve our training pipeline by integrating novel reinforcement learning approaches, notably using GRPO algorithms. This is the continuation of a previous project conducted in this area, and we plan to expand the existing work to enhance our project performances (add MultiMeditron for multi-modal reasoning).

  Contact: Guillaume Boyé (guillaume.boye@epfl.ch)

- **Polyglot Meditron & Giving Meditron a Voice**

  Speaking English is nice, most content online is in English. Having a performant LLM for medical tasks formulated in English is useful. But not enough! In low-resource settings and even in most places of the globe, people usually prefer using their first language rather than English.

  There are many people around the world who even though cannot read, seek healthcare information and guidance. Currently, medical LLMs, even those that are multi-modal, are usually constrained to a few languages thereby limiting their application in this particular use-case of healthcare question answering. The main objective of this project is to extend the multi-lingual speech capabilities of our Meditron model to ensure that it is more accessible to people around the world.

  This project aims at making Meditron models more proficient in other languages, with a focus on low-resource languages. In written and spoken speech. Work is needed, since having a polyglot base model is generally not enough: popular models do not have a focus on low-resource languages, and there is also a need to make sure to teach the model non-English medical terminology.

  Contact: Fabrice Nemo (fabrice.nemo@epfl.ch) & David Sasu (david.sasu@epfl.ch)

- **NeuroMeditron**

  NeuroMeditron develops robust multimodal models for dementia prediction using voice and typing dynamics from the mPower dataset. The project focuses on handling missing modalities through advanced fusion strategies, enabling reliable patient-level monitoring. A proof-of-concept “Neuro Expert” adapter will integrate these digital biomarkers into MultiMeditron.

  Contact: Arianna Francesconi (arianna.francesconi@epfl.ch)

  
- **Meditron-4: Clinical feedback alignment and SOTA dev**

  Meditron-4 is the next iteration of Meditron, designed to close the gap between medical knowledge and guideline-faithful, clinically contextualized behavior. While Meditron-3 is now lagging behind state-of-the-art, Meditron-4 will deliver an open-source fine-tuning and evaluation pipeline and the best clinically aligned model we can produce on top of leading open medical and general base models—while also pushing small, offline-capable models (e.g., MedGemma 4B, LFM-2) for low-resource deployment.

  Contact: Xavier Theimer-Lienhard (xavier.theimer-lienhard@epfl.ch)

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

 -->
