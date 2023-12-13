# plasmidInteract: AI Interface for Plasmid Design

# Introduction and Goals


The design of plasmids is critical in biological research, including gene therapy, protein expression, and synthetic biology. Current design methods are often laborious and error-prone, especially for complex projects. This project aims to develop innovative tools to streamline the plasmid design process, using AI to manage the complexity of large data sets, as demonstrated in CRISPR systems and Pf Ago/AREs research.

We propose an AI algorithm to analyze data (from GeneBank), predict outcomes by comparing designs against a standard library, and optimize plasmid designs for specific research goals.

## Comparison with Existing Platforms

- **PlasmidMaker**: An end-to-end platform for automated plasmid construction.
- **PlatformLi**: A platform for genome editing in mammalian cells.
- Our solution aims to integrate AI more efficiently into the plasmid design process.

## Platform Design

### 1. Genetic Sequence Analysis for Optimal Data Complexity Management

Utilizing GeneBank’s database, we plan to store individual gene files in a dictionary, representing them in multiple formats for effective complexity management and LLM interfacing.

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

Biopython will be used for various representations, with custom algorithms for more complex tasks.

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
