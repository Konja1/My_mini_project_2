Computational Simulation of MicroRNA (miR-122) Biogenesis & RISC Loading

This repository contains a Python-based pipeline designed to simulate the canonical microRNA biogenesis pathway in silico. Using primary structural data and biophysical constraints, the workflow models transcription, Drosha cleavage, Exportin-5 nuclear translocation, cytoplasmic Dicer dicing, thermodynamic asymmetry-based strand selection, and Argonaut-2 (AGO2) RISC loading.

Pipeline Overview

The simulation structurally tracks a target sequence (consult ncbi_dataset/gene.fna) from an initial primary transcript down to an active, functionally loaded RNA-induced Silencing Complex (RISC).

[ DNA / pri-miRNA Sequence ]
                │
                ▼ (ViennaRNA Folding)
     [ Secondary Structure ]
                │
                ▼ (Drosha Cleavage: 11 bp Rule)
     [ pre-miRNA Hairpin Cargo ]
                │
                ▼ (Exportin-5 Nuclear Transport Check)
    [ Cytoplasmic Workspace Entry ]
                │
                ▼ (Dicer Processing: 21 bp Rule)
     [ Functional miRNA Duplex ]
                │
                ▼ (Thermodynamic Asymmetry Rule)
  [ Guide Strand Selection / RISC Assembly ]

Technical Features & Implementation

1. Sequence Ingestion & Secondary Structure Prediction

- Biopython Integration: Parsed and extracted structural records from primary Genomic FASTA datasets (gene.fna).
- Thermodynamic Folding: Utilized the ViennaRNA (RNA module) binding to compute secondary structures via Minimum Free Energy (MFE) optimization.

Metrics:

- Initial sequence length: 85 nt (view in pri-miRNA folder).
- Computed MFE: -45.70 kcal/mol.

2. Drosha Nuclear Cleavage Sim

- Implements the Drosha biophysical constraint, executing a structural cut of exactly 11 base pairs up from the basal junction of the un-cleaved primary stem.
- Automatically isolated and cleaved the 60 nt pre-miRNA hairpin product (view in pre-miRNA folder) from flanking genomic sequences.

3. Exportin-5 Nuclear Export Validation

- Evaluates structural criteria required for transport across the nuclear pore complex.
- Evaluates the presence of the classic 2-nucleotide 3' overhang.
Note: The simulation gracefully accommodates non-canonical variances, forcing the experimental product into the cytoplasmic matrix for processing downstream.

4. Cytoplasmic Dicer Processing

- Models the molecular ruler mechanism of the Dicer enzyme utilizing a modified 21-nucleotide footprint rule tailored to structural pocket configurations.
- Cleaves the pre-miRNA loop into three individual products:
    Mature 5p Strand (24 nt)
    Terminal Loop Product (12 nt)
    Mature 3p Strand (24 nt)

5. Thermodynamic Asymmetry & RISC Assembly

- Evaluates terminal stability rules at the first 4 nucleotides of both the 5' and 3' ends to predict strand selection inside the RNA-induced Silencing Complex (RISC).
- Profiling Metric Results:
    * 5' terminus stability (UGUG): GC count = 2
    * 3' terminus stability (CAAA): GC count = 1
- Verdict: The 3p strand is prioritized as the functional GUIDE (view in mature-miRNA folder) due to its lower 5' terminal stability, while the 5p passenger strand is marked for degradation.

6. AGO2 Complex Anchor Allocation

- Maps the structural sequence anchoring across key Argonaute-2 protein domains (consult the included AGO2_loaded_guide.fasta file):
    * 5' MID Domain Anchor Base: Cytosine (C)
    * 3' PAZ Domain Anchor Base: Adenine (A)
    * Exposed Regulatory SEED Region (nt 2–8): AAACGCC

7. Core Simulation Outputs

   Consult the My_mini_project_2 Jupyter Source file

Generated Data Files

The workflow saves the functional transcripts to standard formats for subsequent downstream annotation or target alignment studies:
- mature_mirna.fasta: Holds the isolated active 3' guide sequence.
- AGO2_loaded_guide.fasta: Fully annotated functional guide sequence containing seed metadata, primed for target mRNA lookup.


Dependencies & Requirements

- Python 3.11+
- Biopython
- ViennaRNA Python Bindings


Developed by J. H. MBIA
-----------------------
