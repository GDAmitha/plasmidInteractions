# plasmidInteract: AI Interface for Plasmid Design

# Introduction

The design of plasmids lies at the forefront of countless biological research endeavors, encompassing gene therapy, protein expression, and synthetic biology. However, present plasmid design methods remain laborious, time-consuming, and susceptible to errors, particularly for more complex projects. This necessitates the development of innovative tools to streamline and enhance the design process, addressing the growing need for complexity management as highlighted by research on CRISPR systems and Pf Ago/AREs[^1][^2]. Specifically, I propose an AI algorithm that can analyze vast amounts of data (GeneBank in this case), predict outcomes by comparing designs against a standard library, and optimize plasmid designs for specific research goals. 


## Comparison with Existing Platforms

- **PlasmidMaker**: An end-to-end platform for automated plasmid construction.
- **PlatformLi**: A platform for genome editing in mammalian cells.
- Our solution aims to integrate AI more efficiently into the plasmid design process.

Platforms like PlasmidMaker and the genome editing platform developed by Li et al, demonstrate the potential of computation and machine learning to improve time, cost, and efficiency of plasmid development, and focus on end-to-end development[^3][^4]. PlasmidMaker, is an end-to-end platform for automated plasmid construction that follows a four-step pipeline: Design, Build (2), and Test. Li et al propose an automated platform for genome editing (we will call it PlatformLi) in mammalian cells consists of four modules: (1) Computer-aided design for gRNAs targeting pathogenic variants, (2) Automated construction of gRNA plasmids, (3) Base editing within mammalian cells, and (4) Machine learning to predict base editing outcomes. Although both include design phases, their plasmid design protocols lack efficient integration with AI. 

PlasmidMaker’s design phase prompts users to create plasmid designs and select templates from a database. Once a design is submitted, it is quality-checked by a technician and approved sequences are organized into picklists for the construction process. My proposed solution automates the template selection and validation with options for manual validation if preferred by the researcher using my platform.
PlatformLi specifically used to create gRNAs to introduce specific single nucleotide variants (SNVs) into cells. It uses ClinVar data to identify SNVs, retrieves DNA segments containing these SNVs via the seqseek Python package, and selects an optimal 20-nucleotide sequence as the gRNA spacer by querying a public primer3 server (https://bioinfo.ut.ee/primer3/). My proposed solution streamlines the reliance on external python scripts through integration with LLMs.

[^1]: Swarts DC, van der Oost J, Jinek M. Structural Basis for Guide RNA Processing and Seed-Dependent DNA Targeting by CRISPR-Cas12a. Mol Cell. 2017 Apr 20;66(2):221-233.e4. doi: [10.1016/j.molcel.2017.03.016](https://doi.org/10.1016/j.molcel.2017.03.016). PMID: 28431230; PMCID: PMC6879319.

[^2]: Zetsche B, Gootenberg JS, Abudayyeh OO, Slaymaker IM, Makarova KS, Essletzbichler P, Volz SE, Joung J, van der Oost J, Regev A, Koonin EV, Zhang F. Cpf1 is a single RNA-guided endonuclease of a class 2 CRISPR-Cas system. Cell. 2015 Oct 22;163(3):759-71. doi: [10.1016/j.cell.2015.09.038](https://doi.org/10.1016/j.cell.2015.09.038). Epub 2015 Sep 25. PMID: 26422227; PMCID: PMC4638220.

[^3]: Enghiad, B., Xue, P., Singh, N. et al. PlasmidMaker is a versatile, automated, and high throughput end-to-end platform for plasmid construction. Nat Commun 13, 2697 (2022). [https://doi.org/10.1038/s41467-022-30355-y](https://doi.org/10.1038/s41467-022-30355-y).

[^4]: Li, S., An, J., Li, Y. et al. Automated high-throughput genome editing platform with an AI learning in situ prediction model. Nat Commun 13, 7386 (2022). [https://doi.org/10.1038/s41467-022-35056-0](https://doi.org/10.1038/s41467-022-35056-0).


## Platform Design

### 1. Genetic Sequence Analysis for Optimal Data Complexity Management

For this solution, I plan to utilize GeneBank’s database. For the first proof-of-concept I am utilizing a subset of ecoli genes. In order to effectively manage complexity of sequence data and interface with LLMs, I propose a data management structure that stores individual gene files from GeneBank into a dictionary. Each file is represented in multiple formats: including full sequence, unique identifiers and regions of interest within each gene like the p20N31 plasmid or specific mutations can be labeled with shorter codes or tags, full feature-based representation of the sequence such as genes, promoters, restriction sites, and other functional elements. 

#### Feature Table

| Feature                   | Description                                                  |
|---------------------------|--------------------------------------------------------------|
| `full_sequence`           | The complete nucleotide sequence.                            |
| `feature_list`            | Features annotated in the GenBank file.                      |
| `gene_structure`          | Information about the gene structure.                        |
| `kmer_distribution`       | Distribution or count of k-mers within the sequence.         |
| `conservation_scores`     | Conservation scores for different regions of the sequence.   |
| `physicochemical_properties` | Summary of properties like GC content.                     |
| `compressed_sequence`     | A compressed version of the sequence.                        |
| `sketch`                  | A compact sketch of the sequence for comparisons.            |

Biopython is a powerful tool to use for the Full Sequence, Feature List, Gene Structure, and Physicochemical Properties. The other representations require locally defined functions. This function will also allow for an in-house sequence database to be used to specialize my design interface for specific use-case labs and researchers.

Please refer to the ipython notebook for the development of this functionality.

### 2. Integration with Bioinformatics and Molecular Biology Tools

We will integrate various tools for internal plasmid design:

| Tool | Description |
|------|-------------|
| Benchling | Cloud-based platform for sequence editing and molecular biology tools. |
| CRISPOR | Designs CRISPR guide RNAs and analyzes off-target effects. |
| BLAST | Compares an input sequence against a database of sequences. |
| CHOPCHOP | Designs CRISPR guide RNAs and analyzes off-target effects. |
| Clustal Omega | Performs multiple sequence alignments. |
| Geneious | Provides graphical representation of DNA sequences, annotations, and modifications. |
| MUSCLE | Performs multiple sequence alignments. |
| Artemis | Genome browser and annotation tool. |
| NEBcutter | Identifies restriction enzyme sites. |
| APE (A Plasmid Editor) | Edits and analyzes plasmid maps. |
| Webcutter | Analyzes restriction enzyme sites. |
| Gene Construction Kit | Manipulates DNA sequences and constructs plasmids graphically. |
| RestrictionMapper | Analyzes restriction enzyme sites. |
| Entrez Programming Utilities (E-utilities) | Searches and retrieves data from NCBI databases. |
| Primer3 | Designs primers for PCR and other experiments. |
| UniProt API | Accesses protein sequence and functional information. |
| OligoCalc | Calculates properties of oligonucleotide sequences. |
| SnapGene | Visualizes and edits DNA sequences, simulates cloning. |


### 3. Machine Learning Algorithms for Intelligent Plasmid Design

The automated design process will begin with user input, parsed using LLM integration. Specifications will be mapped to our database for optimal gene design.

#### Algorithms

| Category                          | Algorithm/Method                | Description                                           |
|-----------------------------------|---------------------------------|-------------------------------------------------------|
| **Sequence & Property Prediction**| CNN                             | Pattern recognition, motif identification, DNA-protein interaction prediction |
|                                   | RNN/LSTM                        | Predict effects of sequence alterations              |
| **CRISPR gRNA Design & Efficiency Prediction** | Deep Learning Models          | Predict gRNA efficiency and specificity              |
|                                   | Off-target Prediction Models    | Predict potential off-target sites of CRISPR editing |
| **Genetic Design Optimization**   | Genetic Algorithms (GA)         | Iteratively improve genetic sequences                |
|                                   | Simulated Annealing (SA)        | Find optimal design in large search spaces           |
| **Data Integration & Feature Extraction** | PCA, t-SNE                      | Reduce data complexity while retaining features      |
|                                   | Random Forests, Gradient Boosting Machines | Feature importance analysis & classification    |
| **Experimental Outcome Prediction**   | Regression Models               | Predict continuous or categorical outcomes           |
|                                   | Natural Language Processing (NLP) Algorithms | Sequence-to-sequence models for specific tasks     |


## Conclusion

Integrating LLMs for automated Plasmid Design requires a combination of computational power and biological expertise. Our goal is to develop a large-scale AI platform for gene editing technologies, currently focusing on optimizing gene sequence representation for synthetic biology applications.
