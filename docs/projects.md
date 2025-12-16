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

- :material-head-dots-horizontal:{ .lg .middle } __Meditron reasoning__

    ---

    Integrate reasoning through unsupervised reinforcement learning into Meditron aiming to further elevate its performance and decision-making abilities.

    [:octicons-arrow-right-24: See Meditron reasoning information](#meditron-reasoning)

- :material-language-html5:{ .lg .middle } __MOOVE__

    ---

    *MOOVE* (Massive Open Online Validation and Evaluation) is a large-scale, participatory evaluation platform designed to collect, structure, and analyze expert feedback on the outputs of clinical large language models (LLMs).

    [:octicons-arrow-right-24: See MOOVE information](#moove-massive-open-online-validation-and-evaluation)

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

## Meditron reasoning

*This project is supervised by Guillaume Boyé and Lars Klein*

Reasoning has been a significant breakthrough in advancing the capabilities of
large language models in recent years. It has consistently demonstrated its ability
to enhance decision-making processes within these systems. The objective of this project
is to integrate reasoning through unsupervised reinforcement learning into Meditron
aiming to further elevate its performance and decision-making abilities.

__Completed:__

- Integrated VERL on the cluster with distributed training on multi-node with appropriate docker image
- Docker image for SGLang inference
- LLM-as-a-judge based reward
- Distributed setup
- Prototype dataset and prototype reward function
- Prototype support for multiturn and tooling for python execution

__Possible Tasks:__

- Experiment with new datasets and reward modeling for reasoning tasks to enhance model generation
- Explore additional RL algorithms and architecture for improving capabilities (multi-agent setup)
- Expand the tool based and introduce RAG system to improve the observability of the reasoning
- Benchmark model performance on complex tasks

__(Required) Experience:__

- Strong knowledge of __Python__, __PyTorch__ experience is a plus
- Experience on distributed infrastructure using __SLURM__, working with server is a plus
- __Linux__ knowledge (for building Docker image, GLHF)
- Knowledge of reward modeling is a plus

## MOOVE: Massive Open Online Validation and Evaluation

*This project is supervised by Fay Elhassan and Karian For*

*MOOVE* (Massive Open Online Validation and Evaluation) is a large-scale, participatory evaluation platform designed to collect, structure, and analyze expert feedback on the outputs of clinical large language models (LLMs). Built in collaboration with clinicians and healthcare institutions across diverse geographies including Sub-Saharan Africa, South Asia, Latin America, and Europe MOOVE is the first multilingual, context-sensitive evaluation environment tailored to healthcare AI systems in low- and middle-income as well as high-resource settings.
