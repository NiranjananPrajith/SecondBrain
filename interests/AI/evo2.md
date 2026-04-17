---
title: Evo2
created: 2026-04-10
tags: [AI, biology, genomics, foundation-models, model-cards]
---

# Summary: Genome Modelling and Design Across All Domains of Life with Evo 2

**Authors:** Garyk Brixi, Matthew G. Durrant, Jerome Ku, Mohsen Naghipourfar, Michael Poli, Gwanggyu Sun, Greg Brockman, et al. *(Corresponding authors: Patrick D. Hsu & Brian L. Hie)*
**Publication:** Nature (Published online: March 04, 2026)

---

## 1. Overview and Introduction

The paper introduces **Evo 2**, a biological foundation model trained on DNA sequences spanning all domains of life (archaea, bacteria, eukaryotes, and phage/organelles). With up to 40 billion parameters and a 1 million token context window at single-nucleotide resolution, Evo 2 generalizes across the central dogma of molecular biology (DNA, RNA, proteins).

Evo 2 demonstrates state-of-the-art zero-shot predictive capabilities for mutational effects, excels at predicting clinical variant pathogenicity (including challenging non-coding and insertion/deletion variants), and can generate highly complex, functional sequences ranging from promoters to entire small genomes. Furthermore, inference-time guidance allows the model to design sequence segments with programmable, cell-type-specific chromatin accessibility profiles, which the authors successfully validated in vivo.

---

## 2. Model Architecture, Training, and Data

**Dataset (OpenGenome2):** Compiled from curated, non-redundant nucleotide sequences comprising 9.3 trillion tokens (base pairs). To mitigate biosecurity risks, sequences of viruses that infect eukaryotic hosts were explicitly excluded.

**Model Sizes:**
- **Evo 2 7B:** 7 billion parameters, trained on 2.4 trillion tokens.
- **Evo 2 40B:** 40 billion parameters, trained on 9.3 trillion tokens.

**Architecture (StripedHyena 2):** Instead of standard Transformers, Evo 2 utilizes a multi-hybrid architecture combining attention with three variants of input-dependent convolution operators: short explicit (SE), medium regularized (MR), and long implicit (LI) operators. This setup dramatically improves training efficiency, throughput (up to 3× speedup at a 1M context length compared to Transformer baselines), and loss scaling.

**Training Phases:**
- **Pretraining:** Trained at an 8,192-token context length to capture local functional genetic elements.
- **Midtraining:** Context length was progressively extended to 1 million tokens to learn long-range genomic interactions. The 1M token context was validated using a synthetic "needle-in-a-haystack" retrieval task, demonstrating near-perfect recall.

---

## 3. Key Capabilities and Findings

### A. Evolutionary Constraint and Zero-Shot Predictions

Because Evo 2 is trained on diverse genomes, its predicted sequence likelihoods effectively capture evolutionary constraints without any task-specific fine-tuning.

- **Mutational Effects:** The model naturally recognizes start/stop codons and translation initiation sites (Shine-Dalgarno in prokaryotes, Kozak in eukaryotes). Synonymous mutations are assigned higher likelihoods than non-synonymous or frameshift mutations.
- **Alternative Genetic Codes:** Evo 2 accurately adjusts to alternative genetic codes (e.g., the mycoplasma or ciliate codes) purely based on surrounding sequence context.
- **Deep Mutational Scanning (DMS):** Zero-shot likelihoods correlate well with empirical fitness across proteins and non-coding RNAs (tRNAs, rRNAs).
- **Genomic Architecture:** A lightweight classifier trained on Evo 2 embeddings accurately identifies exon-intron boundaries at single-nucleotide resolution across diverse eukaryotic species, outperforming specialized tools like AUGUSTUS and SegmentNT.

### B. Human Variant Effect Prediction

Evo 2 represents a major leap in predicting human clinical variants, a historically challenging task for whole-genome multi-species models.

- **ClinVar Database:** Evaluated on both coding and non-coding variants. Evo 2 40B achieved leading zero-shot performance on non-SNVs (insertions, deletions, duplications), which many protein-specific models (like AlphaMissense) cannot score.
- **BRCA1 & BRCA2:** The model accurately differentiated loss-of-function variants from functional/intermediate ones. Using Evo 2 embeddings to train a simple supervised ridge-regression classifier yielded near-perfect variant effect prediction for BRCA1.
- **Splicing Variants (SpliceVarDB):** Showed strong zero-shot performance on exonic and intronic variants affecting splice sites, remaining competitive with supervised splicing predictors like SpliceAI.

### C. Mechanistic Interpretability

Using a technique called Sparse Autoencoders (SAEs), the authors decomposed the model's dense internal representations (specifically at Layer 26) into sparse, human-interpretable features without providing any biological labels during training.

**Discovered Features:** The model internally learned specific features that fire precisely on:
- Protein secondary structures (α-helices, β-sheets).
- Exon/intron boundaries and coding sequences.
- Transcription factor (TF) binding motifs in promoter regions.
- Mobile genetic elements, such as prophages. Interestingly, a phage-associated feature fired accurately on CRISPR spacer arrays, indicating the model learned the biological link between phage DNA and CRISPR viral immunity.

### D. Unconstrained Genome-Scale Generation

Evo 2 was prompted to generate highly complex genetic sequences from scratch across various taxonomic scales:

- **Human Mitochondria (16 kb):** Generated full organelle genomes containing the correct synteny of coding regions, tRNAs, and rRNAs. Generated proteins were predicted by AlphaFold 3 to form realistic multimeric mitochondrial complexes.
- **Prokaryotic Genomes (M. genitalium - 580 kb):** Generated multi-hundred-kilobase sequences with proteins exhibiting natural secondary structures and significant Pfam hits.
- **Eukaryotic Genomes (S. cerevisiae - 330 kb):** Generated massive genomic segments featuring promoters, intronic structures, and realistic tetranucleotide usage.

### E. Inference-Time Guided Epigenomic Design

To demonstrate programmable design, the authors coupled Evo 2 with a scoring function (an ensemble of Enformer and Borzoi models) using a beam search algorithm during sequence generation.

- **Goal:** Design multi-kilobase DNA sequences with precise, user-defined chromatin accessibility profiles (peaks).
- **Morse Code Epigenetics:** They successfully generated sequences that encoded Morse code patterns for the words "EVO2", "ARC", and "LO" into the predicted chromatin accessibility landscape.
- **In Vivo Experimental Validation:** The designed sequences were synthesized, assembled, and integrated into mouse embryonic stem cells (mESCs) and human cell lines (HEK293T, K562). ATAC-seq measurements confirmed that the sequences successfully induced the targeted chromatin accessibility peaks in living cells.
- **Cell-Type Specificity:** The model successfully designed elements that possessed differential accessibility across different cell types (e.g., active in K562 but inactive in HEK293T).

---

## 4. Biosecurity and Ethics

Aligned with responsible AI practices, the team conducted safety testing:

- **Data Filtration:** Excluded genomes from viruses infecting eukaryotes to prevent the model from learning to manipulate or generate human pathogens.
- **Safety Verification:** Post-training tests proved that Evo 2 fails to accurately predict mutational fitness in human viral proteins and generates random, non-functional sequences when prompted to design human viral proteins.
- **Ancestry Bias:** Analyzed variant predictions to ensure the model does not exhibit outsized biases toward specific human population ancestries compared to established population-free baselines.

---

## 5. Open Source Contributions

To accelerate computational biology research, the authors have released:

- **Model Weights:** Parameters for the 7B and 40B models (on Hugging Face).
- **Code:** Distributed training, inference, and beam-search guidance code on GitHub.
- **Data:** The full OpenGenome2 dataset.
- **Interactive Tools:** Web interfaces for sequence generation/scoring (Evo Designer) and exploring mechanistic interpretability features (Evo Mech Interp Visualizer).

---

Paper link: https://drive.google.com/file/d/1imtDeK2bWbcC0hrp7cCUVLD3duZ9lg2T/view?usp=sharing
