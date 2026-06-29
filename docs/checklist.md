# LiGHT Technical Publication Checklists

This document contains the LiGHT checklists for publishing Hugging Face model cards, Hugging Face dataset cards, and GitHub repositories.

---

## Table of Contents

- [LiGHT Hugging Face Model Card Checklist](#light-hugging-face-model-card-checklist)
- [LiGHT Hugging Face Dataset Card Checklist](#light-hugging-face-dataset-card-checklist)
- [LiGHT GitHub Repository Checklist](#light-github-repository-checklist)

---

# LiGHT Hugging Face Model Card Checklist

Use this checklist before publishing or updating a Hugging Face model card. The goal is to make the card easy to discover, technically reproducible, scientifically credible, clinically responsible, and useful to multiple audiences: researchers, engineers, clinicians, auditors, policy teams, journalists, and downstream builders.

## A good model card should define exactly

1. What the model is.
2. How it was trained.
3. What evidence supports its claims.
4. What it must not be used for as well as what risks remain.
5. How users can reproduce, evaluate, cite, and contact the maintainers.

## Extremely important things to keep in mind

Do not publish a model card until a skeptical external reader can answer these eight questions without contacting the authors:

1. What is the model, what base model does it derive from, and what changed?
2. What license governs the model, code, data, and derived use?
3. What training data, synthetic data, filtering, decontamination, and governance processes were used?
4. What evaluations were run, on which benchmarks, under which prompts/settings, and with what uncertainty?
5. What is the model intended for, and what is it explicitly not intended for?
6. What are the known or possible clinical, demographic, geographic, language, safety, privacy, and misuse limitations?
7. How can a user run the model correctly and reproduce the reported results?
8. Who maintains the model, how should it be cited, and how can issues be reported?

## Model card checklist

The table below provides checklist items intended to guide the preparation of model cards for upload to the lab's Hugging Face repository. Because the repository will host different types of models, the guiding principle is that each checklist item should be addressed where it is relevant to the model being uploaded. If a checklist item is not applicable, or if you choose not to satisfy it for another reason, please document a clear and reasonable justification for that decision.

| Done | Requirement | What excellent looks like | Evidence to include |
|---|---|---|---|
| [ ] | YAML metadata is complete | The top metadata block includes `pipeline_tag`, `library_name`, `license`, `language`, `base_model`, `datasets`, `tags`, arXiv/preprint links, and `model-index`/evaluation entries when available. | `README.md` YAML block |
| [ ] | One-paragraph executive summary | The first screen tells users (1) what the model is, (2) why it exists, (3) what it improves, and (4) the main non-use boundary. | Opening paragraph |
| [ ] | Claims are evidence-bound | Every performance, safety, openness, or SOTA claim is tied to a benchmark, evaluation setting, paper section, table, or repository artifact. | Benchmark table and, where necessary, paper link(s) |
| [ ] | Clinical disclaimers are precise, if you are releasing a medical-related model | Since we are an intelligent global health research lab releasing these models to aid in research, all written medical-related model cards should state research-only status and exclude diagnosis, treatment, triage, individual patient advice, autonomous decision-making, or deployment-adjacent use without independent validation. However, if the intended purpose of the released model is to facilitate not just research but also support commercial use, the written disclaimer should reflect this as well. | Intended Use / Limitations |
| [ ] | Model lineage and upstream relationship are clear | State the exact base model, upstream license, checkpoint version, architecture changes if any, tokenizer, context length, parameter count, precision, and release organization. | Model Details table |
| [ ] | Intended users and intended tasks are specific | Name the target users and the concrete tasks the model is intended to support. Avoid vague statements such as "for healthcare" or "for research" without task boundaries. | Intended Use section |
| [ ] | Training, evaluation, and validation data are responsibly documented | Describe the major data sources and dataset components used for training, evaluation, and validation. This should include, where applicable, data licenses, permissions, redistribution status, synthetic data use, preprocessing steps such as filtering, deduplication, annotation, and quality-control procedures, privacy and sensitive-data handling, and any known data characteristics, gaps, or limitations that may affect the model's reliability, fairness, or generalizability. | Data / Data Governance section |
| [ ] | Decontamination and leakage controls are explained | If applicable, distinguish de-duplication from benchmark decontamination. State what benchmarks were checked, what method was used, and the residual risk of benchmark or synthetic-data contamination. | Decontamination / Data Processing section |
| [ ] | Hardware requirements and generation settings are stated | Specify expected VRAM/RAM, dtype, multi-GPU requirements, quantization availability if any, recommended decoding settings, and whether serving frameworks such as vLLM, TGI, `llama.cpp`, or quantized deployment were actually tested. | Usage / Requirements section |
| [ ] | Licensing and regulatory status are unambiguous | State the license for model weights, code, data, synthetic data, and paper where applicable. For medical-related models, state whether the model is not a medical device, not clinically cleared, and not validated for patient care unless formally true. | License / Regulatory Status section |
| [ ] | Disclose environmental information when feasible | Where possible, include training compute, energy/carbon estimates, or explain why they are unavailable. | Environmental Impact |
| [ ] | Clarify authorship and affiliations | List release organization, authors/contributors, maintainers, and acknowledgments. Avoid vague "Authors: LiGHT" only if individual attribution is expected. | Authors section |
| [ ] | Acknowledgments and disclosure of funding | Clearly state any acknowledgements that made the work or release possible and also provide information on the source of funding behind the model development. | Acknowledgements and Disclosure of Funding section |

---

# LiGHT Hugging Face Dataset Card Checklist

Use this checklist before publishing or updating a Hugging Face dataset card on the lab's official repository. The goal is to make everything technically usable and scientifically credible to multiple audiences.

## A good dataset card should define exactly

1. What the dataset is and why it exists.
2. Where the data came from, how it was collected and/or generated, and how it was processed.
3. What the dataset contains: schema, features, labels, splits, examples, and scale.
4. Who may use the dataset, for what purposes, under what license or access conditions, and with what obligations.
5. What the dataset must not be used for, what risks remain, and what communities or settings are underrepresented.
6. How users can load, reproduce, evaluate, cite, maintain, and report issues with the dataset.
7. If it applies, how the dataset is being governed.

## Extremely important things to keep in mind

Do not publish a dataset card until a skeptical external reader can answer these ten questions without contacting the authors:

1. What exactly is the dataset, what version is being released, and, if applicable, what changed from earlier versions?
2. What license governs the dataset, annotations, code, examples, source data, and derived use?
3. Where did the data come from, who owns it, and what permissions, consent, ethics approvals, or exemptions apply?
4. What does each file, config, split, feature, column, label, and example represent?
5. How were data collected/generated, processed, and quality checked?
6. What are the dataset's intended uses, intended users, and explicit non-use boundaries?
7. What are the known privacy, re-identification, copyright, misuse, safety, and temporal risks?
8. How representative is the dataset, and what communities, contexts, modalities, or conditions are missing or underrepresented?
9. How can a user load the dataset correctly, reproduce the documented splits/statistics, and verify file integrity?
10. Who maintains the dataset, how should it be cited, and how can issues, takedown requests, or safety concerns be reported?

## Data card checklist

The table below provides checklist items intended to guide the preparation of data cards for upload to the lab's Hugging Face repository. The guiding principle here is that each checklist item should be addressed where it is relevant to the dataset being uploaded. If a checklist item is not applicable, or if you choose not to satisfy it for another reason, please document a clear and reasonable justification for that decision.

| Done | Requirement | What excellent looks like | Evidence to include |
|---|---|---|---|
| [ ] | YAML metadata is complete | The top metadata block is valid, parses on Hugging Face, and includes `license`, `language`, `name`, `task_categories`, `task_ids` where useful, `tags`/modality, `size_categories`, `annotations_creators` if applicable, `source_datasets`, papers/arXiv, and configs/data_files when needed. | `README.md` YAML block |
| [ ] | One-paragraph executive summary | Tell users what the dataset is, why it exists, who it is for, what it enables, and the main non-use or access boundary. | Opening paragraph |
| [ ] | Dataset identity and version, if applicable, are unambiguous | State dataset name, version/date, release organization, maintainers, DOI or canonical citation if available, repository visibility, and whether the release is initial, updated, frozen, deprecated, or superseded. | Dataset Details table |
| [ ] | Provenance is transparent | Users can see where the data came from, how it was collected/generated, licensed, consented, and, if applicable, governed. | Provenance section |
| [ ] | Structure, schema, and splits are clear | The card describes files, configs, columns/features, labels, data types, units, train/validation/test splits, and any protected holdout set. Dataset Viewer loads correctly or the card explains why it cannot. | Dataset Structure / Viewer |
| [ ] | Usage code is tested | The sample code loads the exact repo and config, names the split, handles authentication/gating where applicable, and prints at least one realistic example without errors. | Usage section |
| [ ] | Licensing and permissions are explicit | The card distinguishes dataset license, source-data licenses, annotation licenses, code license, paper license, redistribution rights, commercial-use restrictions, and third-party constraints. | License table |
| [ ] | Privacy and sensitive-data risks are addressed | The card states whether the dataset contains personal data such as protected health information, faces, voices, geolocation, rare diseases, identifiable institutions, or other sensitive attributes, and describes redaction/de-identification/access controls. | Privacy / Governance |
| [ ] | Intended use and explicit non-use are stated | The card names intended users/tasks and explicitly excludes unsupported, unsafe, and unlawful uses where relevant. | Intended Use / Non-use |
| [ ] | Limitations are visible | If known, provide information on representativeness, bias, label noise, missingness, geographic/language/resource-setting gaps, temporal drift, and annotation uncertainty. | Limitations section |
| [ ] | Citations and contact channels are present | BibTeX, authorship, affiliations, issue reporting, responsible disclosure, takedown requests, and community links are included. | Citation / Contact |

---

# LiGHT GitHub Repository Checklist

Use this checklist before publishing or updating a GitHub repository for the lab. The goal is to make every repository easy to understand, easy to run, scientifically credible, reusable by others, and maintainable over time.

## A good GitHub repository general README should clearly define

1. What the project is.
2. Why the project exists.
3. How to install and run it.
4. What data, models, or external resources it depends on.
5. How results can be reproduced or verified.
6. What the repository can be used for as well as a disclaimer of what it should not be used for.
7. How the work should be cited.
8. Who maintains the repository.

## Extremely important things to keep in mind

Do not publish to the lab's official GitHub repository until a skeptical external reader can answer these eight questions without contacting the authors:

1. What does this repository do, and who is it for?
2. What problem does it solve, and what is the expected use case?
3. How do I install it and run the main example?
4. What are the required dependencies, data files, model weights, API keys, or external services?
5. How can I reproduce the main results, figures, experiments, or outputs?
6. What license governs the code, data, models, and derived use?
7. What are the known limitations, risks, or inappropriate uses?
8. Who maintains the repository, how should it be cited, and how can problems be reported?

However, GitHub repos that should be made public but are not ready to be unveiled on the lab's official GitHub repo can be placed on the lab's `LiGHT-playground` GitHub repo.

## GitHub repository checklist

The table below provides checklist items intended to standardize the GitHub repositories for the lab. Kindly ensure that the checklist items here are satisfied as much as possible. If for any reason you decide not to follow a checklist item, there should be a documented strong and reasonable justification for doing so.

| Done | Requirement | What excellent looks like | Evidence to include |
|---|---|---|---|
| [ ] | Repository name and description are clear | The repository name, GitHub description, and topics make it immediately clear what the project does and whether it is research code, a package, a demo, a dataset utility, or a production-oriented tool. | Repository name, GitHub description, topics/tags |
| [ ] | README has a strong opening summary | The first screen explains what the project is, why it exists, who it is for, and what users can do with it. A new visitor should understand the repository within one minute. | Opening section of `README.md` |
| [ ] | Repository structure is easy to navigate | Files and folders are organized logically, with clear names. Avoid unexplained scripts, scattered notebooks, duplicate files, or unclear experimental leftovers. | Folder structure, repository tree |
| [ ] | Installation instructions are complete and tested | Users can create the environment and install dependencies using clear instructions. Include Python version if Python is the programming language being used, package manager, dependency file, and any system-level requirements. | `README.md`, `requirements.txt`, `environment.yml`, `pyproject.toml`, Dockerfile if applicable |
| [ ] | Quickstart example works end-to-end | The README includes a minimal command or notebook that users can run to confirm the repository works. The example should use small sample data or clearly described inputs. | Quickstart section, example script, demo notebook |
| [ ] | Main usage instructions are clear | The repository explains the main commands, scripts, configuration files, expected inputs, expected outputs, and common options. Users should not need to inspect the code to know how to run the project. | Usage section, CLI examples, config examples |
| [ ] | Dependencies and external resources are documented | The repository clearly states required datasets, model weights, APIs, credentials, environment variables, hardware, GPU/CPU requirements, and download instructions where applicable. | Dependencies / Resources section |
| [ ] | Data and model handling are responsible | If the repository contains any data or needs any data to run, it explains in the main README what data or model files are required, whether they are included or external, their licenses and/or access restrictions, and whether sensitive, clinical, personal, or restricted data should not be committed. | Data / Models section, `.gitignore`, download scripts |
| [ ] | Reproducibility instructions are included | Users can reproduce the main results, tables, figures, experiments, or outputs using documented commands, seeds, configuration files, and expected outputs. If full reproducibility is not possible, the reason is clearly explained. | Reproducibility section, scripts, configs, expected results |
| [ ] | Code quality is sufficient for reuse | Code is readable, modular, and reasonably documented. Important functions, scripts, and configuration options are named clearly. Dead code, unused files, hard-coded personal paths, and local machine assumptions are removed. A CI and pre-commit are put in place to check formatting with ruff. | Source code, comments, docstrings |
| [ ] | Testing or validation is provided where feasible | The repository includes basic tests, sanity checks, example outputs, or validation scripts to help users verify that the code is working correctly. Tests should be written to work with `pytest`. | `tests/`, sanity-check script, example output |
| [ ] | License is clear and appropriate | The repository includes a license and clearly states what it covers. If code, data, model weights, documentation, or third-party assets have different licenses, these are explicitly distinguished. | `LICENSE`, License section in `README.md` |
| [ ] | Limitations and appropriate use are stated | The README explains what the repository is intended for and any important limitations, risks, or unsafe use cases. High-stakes domains such as health, humanitarian work, law, or public policy require especially clear boundaries. | Limitations / Intended Use section |
| [ ] | Citation information is included | Users are told how to cite the repository, associated paper, dataset, model, or software package. Include BibTeX where applicable. | Citation section, `CITATION.cff` if available |
| [ ] | Maintainers and contact information are provided | The repository identifies the release organization, contributors, maintainers, and how users should ask questions, report bugs, or disclose concerns. | Maintainers / Contact section, Issues, Discussions |
| [ ] | Versioning and release history are documented | Important updates, breaking changes, paper versions, dataset/model changes, and deprecated functionality are recorded. Use releases and tags to mark any new important uploads and changes. | `CHANGELOG.md`, GitHub releases, tags |
| [ ] | Repository hygiene is complete before publication | The repository does not contain secrets, API keys, passwords, private data, personal file paths, unnecessary large files, temporary outputs, cache files, or unreviewed notebooks. Secrets can be used in GitHub Actions related to tests or deployment; in this case, those secrets have to be documented in a README in the GitHub Actions folder of the repository. | `.gitignore`, clean commit history, repository scan |
| [ ] | Maintenance status is stated | The README indicates whether the repository is actively maintained, experimental, archived, frozen for paper reproducibility, or superseded by another repository. | Maintenance section |
