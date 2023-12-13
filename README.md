# plasmidInteractions: AI Interface for Plasmid Design

# Introduction and Goals

[Plasmid Interactions on GitHub](https://github.com/GDAmitha/plasmidInteractions)

[GeneSeq Data Repository on Colab](http://tinyurl.com/GeneseqDataRepColab)

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

- Benchling: Cloud-based platform for sequence editing and molecular biology tools.
- CRISPOR: Designs CRISPR guide RNAs and analyzes off-target effects.
- BLAST: Compares an input sequence against a database of sequences.
- ... and many more.

### 3. Machine Learning Algorithms for Intelligent Plasmid Design

The automated design process will begin with user input, parsed using LLM integration. Specifications will be mapped to our database for optimal gene design.

#### Algorithms

- Sequence & Property Prediction: CNN, RNN/LSTM
- CRISPR gRNA Design & Efficiency Prediction: Deep Learning Models
- Genetic Design Optimization: Genetic Algorithms (GA), Simulated Annealing (SA)
- Data Integration & Feature Extraction: PCA, t-SNE, Random Forests
- Experimental Outcome Prediction: Regression Models, NLP Algorithms

## Conclusion

Integrating LLMs for automated Plasmid Design requires a combination of computational power and biological expertise. Our goal is to develop a large-scale AI platform for gene editing technologies, currently focusing on optimizing gene sequence representation for synthetic biology applications.
