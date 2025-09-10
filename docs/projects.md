# Projects for Fall 2025

## 1. Meditron

  1. MultiMeditron

  This project is about making Meditron multimodal: the user can provide Meditron with medical images, in addition to text. Work is two-fold: adapting the codebase of Meditron to make it have a multimodal architecture, and making the "expert" models that process the images and make embeddings fed to Meditron.

  Contact: Michael Zhang (michael.zhang@epfl.ch)

  2. Meditron Reasoning

  This project aims to improve our training pipeline by integrating novel reinforcement learning approaches, notably using GRPO algorithms. This is the continuation of a previous project conducted in this area, and we plan to expand the existing work to enhance our project performances (add MultiMeditron for multi-modal reasoning).

  Contact: Guillaume Boyé (guillaume.boye@epfl.ch)

  3. Polyglot Meditron

  Speaking English is nice, most content online is in English. Having a performant LLM for medical tasks formulated in English is useful. But not enough! In low-resource settings and even in most places of the globe, people usually prefer using their first language rather than English.

  This project aims at making Meditron models more proficient in other languages, with a focus on low-resource languages. Work is needed, since having a polyglot base model is generally not enough: popular models do not have a focus on low-resource languages, and there is also a need to make sure to teach the model non-English medical terminology.

  Contact: Fabrice Nemo (fabrice.nemo@epfl.ch)

## 2. MMORE

MMORE stands for Massive Multimodal Open RAG & Extraction, it is our Python library for a scalable multimodal pipeline for processing, indexing, and querying multimodal documents.

[GitHub repo](https://github.com/swiss-ai/mmore)

Contact: Fabrice Nemo (fabrice.nemo@epfl.ch)

## 3. Moove

The [moove](https://jointhemoove.org) is a collaborative platform where experts and communities co-design and validate AI models. The initiative focuses on aligning large language models with real-world standards, ensuring they are transparent, safe, and context-aware.

On the platform, experts can:
* Challenge AI with tough, domain-specific questions.
* Rank and critique answers, identifying mistakes, ethical issues, and bias.
* Improve AI outputs by rewriting them to align with professional standards.
* Collaboratively fine-tune models (like Meditron for medicine and Legitron for law) so they adapt to institutional and humanitarian needs.

The moove is already partnered with institutions such as CHUV, ICRC, the Gates Foundation and many hospitals around the world.

Here are examples of semester project opportunities:

* Migrating to a secure Convex-based database for handling sensitive patient data in local server environments.
* Broadening the platform to include non-medical experts, adapting workflows and validation mechanisms.
* Building functionality where human experts can answer questions like LLMs, and then enabling expert voting between LLM and human responses.
* Integrating MMORE to deliver a production-ready Retrieval-Augmented Generation (RAG) pipeline, with robust file summarization and contextualization.
* Improving the speech-to-speech feature, making it more polished and easy to use.
* Any other fancy idea you might have on how to improve the moove!

The project is software-engineering focused:

* Strong emphasis on code quality, code reviews, and GitHub pull requests.
* Stack includes React.js (knowledge helpful, but not mandatory).
* Work is collaborative and impactful, directly shaping how AI models are tested and aligned with human expertise.

Contact: Bryan Gotti (bryan.gotti@epfl.ch)

## 4. HIC-Lab AI Bootcamp

Very cool project about teaching the basics of AI applied to healthcare. The target audience is healthcare workers and computer scientists in Rwanda. Our work in LiGHT is to improve the content of the bootcamp so that students learn better, and mentor students there, guide them throughout their completion of the bootcamp.

Contact: Fabrice Nemo (fabrice.nemo@epfl.ch)



