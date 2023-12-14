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

The automated design process begins with a user input, which is then parsed using integration with an LLM. Key specifications are extracted and inputted to our designer algorithm. These specifications are mapped to our database of genes and corresponding features. The below algorithms will be implemented to determine an optimal gene design.

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

### Overall Specification: Putting it all together.

#### 1. Function Details:
- **Function Name**: plasmidInteract
- **Function Description**: This function designs a CRISPR-based plasmid using user-provided specifications, including DNA sequences, target sites, and desired genetic modifications.

#### 2. Approach and Algorithm:
- **Approach**:
  - **Dependent Functions**:
    - query handling
    - DNA sequence analysis
    - CRISPR target prediction
    - plasmid visualization
  - **Function Interaction**: Interacts with these functions to validate inputs, generate CRISPR targets, and construct plasmid maps.
  - **Data Flow**: User inputs are passed to an LLM integration function; outputs from these are two-fold: the genetic data we want to deal with, and the actions we want to take with those genetic objects.
  - **Error Handling**: Checks for invalid logic in the message or incompatible genetic elements.
  - **Required State**: Assumes latest versions of the above mentioned functions for accurate analysis.
- **Algorithm**:
  1. Validate user inputs for sequence correctness.
  2. Use sequence analysis to identify potential CRISPR target sites.
  3. Design guide RNAs for specified targets.
  4. Simulate the insertion of CRISPR components into the plasmid.
  5. Visualize the final plasmid construct.

#### 3. Input Specification:
- Any string of text that can be processed by an LLM.
  - query handler (which integrates an LLM) extracts the following information:
    - **Name**: UserInputData
    - **Type**: Structured Object (containing strings, integers, etc.)
    - **Description**: Contains all user-provided data, such as DNA sequences, target locations, and desired modifications.
    - **Constraints**: Sequences must be valid nucleotide sequences; targets should be within the provided sequences.

#### 4. Output Specification:
- **Type**: Plasmid Object
- **Description**: Contains the designed plasmid map, CRISPR target sites, guide RNA sequences, a list of features, and a summary of the design.

#### 5. Error Handling and Validation:
- Invalid sequences or incompatible targets trigger an error with a descriptive message.
- Input validation checks for correct sequence formats and logical consistency in the requested modifications.

#### 6. Examples and Test Cases:

##### A. Typical Scenario

- **Input**:
  - 'Given the sequence ATGGCCATTGTAATGGGCCGCTGAAAGGGTGCCCGATAG, can you add a GFP at the 12th base?'
- **query-result**: 
  - DNA sequence: `ATGGCCATTGTAATGGGCCGCTGAAAGGGTGCCCGATAG`
  - Target site: Position 10-15
  - Genetic modification: Insertion of a GFP gene at position 12
- **Expected Output**:
  - Plasmid map: Shows the vector with the GFP gene inserted at the specified position.
  - Guide RNA: Sequence targeting the specified site.
  - Edited sequence: The sequence showing the GFP gene successfully inserted.
  - Message: "Plasmid design successful. GFP gene inserted at position 12."

##### B. Edge Case: Overlapping Target Sites
- **Input**:
  - 'Given the sequence ATGGCCATTGTAATGGGCCGCTGAAAGGGTGCCCGATAG, can you remove the 6 base x (TTGTAA) feature and add GFP at the 12th base?'
- **query-result**:
  - DNA sequence: `ATGGCCATTGTAATGGGCCGCTGAAAGGGTGCCCGATAG`
  - Target sites: Position 8-13 and 10-15 overlapping
  - Genetic modification: Deletion at position 8-13 and insertion at position 10-15
- **Expected Output**:
  - Error message: "Overlapping target sites detected. Please revise target positions."
  - Suggestion: "Consider separate modifications or adjusting target ranges to avoid overlap."

##### C. Edge Case: Multiple Editing Requests
- **Input**:
  - 'Given the sequence ATGGCCATTGTAATGGGCCGCTGAAAGGGTGCCCGATAG, can you add x, at a, remove y at b, and substitute z at c?'
- **query-result**:
  - DNA sequence: `ATGGCCATTGTAATGGGCCGCTGAAAGGGTGCCCGATAG`
  - Multiple targets: Positions 5-10, 15-20, 25-30
  - Genetic modifications: Insertion, deletion, and substitution at the respective targets
- **Expected Output**:
  - Plasmid map: Shows the vector with all requested modifications.
  - Guide RNAs: Sequences for each target site.
  - Edited sequence: The sequence showing all modifications.
  - Message: "Multiple edits applied. Review the plasmid map for accuracy."

##### D. Error Scenario: Invalid DNA Sequence
- **Input**:
  - 'Given the sequence ATGXCCATTXTAATGGGCCGCTGAAXGGGTGCCCGATAG, can you add x at y?'
- **query-result**:
  - DNA sequence: `ATGXCCATTXTAATGGGCCGCTGAAXGGGTGCCCGATAG` (invalid nucleotides 'X')
  - Target site: Position 10-15
  - Genetic modification: Insertion of a sequence
- **Expected Output**:
  - Confirmation message: "Invalid DNA sequence detected. Please compare with our closest sequence and confirm we can proceed?"

##### E. Error Scenario: Incompatible Modification Request
- **Input**:
  - 'Given the sequence ATGXCCATTXTAATGGGCCGCTGAAXGGGTGCCCGATAG, can you insert the gene 'ATGXCCATTXTAATGGGCCGCTGAAXGGGTGCCCGATAG' to result in a 50 base pair sequence?'
- **query-result**:
  - DNA sequence: `ATGGCCATTGTAATGGGCCGCTGAAAGGGTGCCCGATAG`
  - Target site: Position 5-10
  - Genetic modification: Insertion of a large gene fragment that exceeds vector capacity
- **Expected Output**:
  - Error message: "Requested modification is incompatible with vector capacity."
  - Suggestion: "Consider using a larger vector or reducing the size of the insert."


## Results

### GeneBank Parsing iPython Workflow
First iteration of the GeneBank Parsing Function took more than 15 minutes to parse through 5 genebank files. This is not ideal, however this time would be an overhead cost, as demonstrated in the efficiency testing of the GeneBank Parsing notebook. The goal is to build a locally accessible list of genes that can be accessed throughout the rest of the pipeline. At the end the data was a large dictionary that captured the key features of each sequence, stored the location of the features in the sequence, and saved a copy of the full sequence. Though this is an initial first step towards implementation, This workflow allows us to build a database of sequences that can be accessed by an LLM down the line.

### The Workflow Included
1. A function that takes a given genebank file that has been read by SeqIO and parses through it, collecting different representations of the sequence.
2. A function to download and decompress the file directly from the genebank website.
3. A function to parse through files and store them in a dictionary.
4. A function to aggregate a specific set of features we want, like CDS and gene sequences.
5. A function to map these locations to sequence subsets.
6. Three tests to analyze the performance of these algorithms. The final functions are included, which are almost instantaneous.

### The Workflow Yet-To-Be-Implemented
Within genebank parser, we still require the functionality of mapping these genes with their functionality. Once this is acheived, then genebank parser is relatively complete.


## Conclusion

There is still a lot that needs to be implemented before a working prototype can be achieved. Development of this project can be broken down to the 3 overarching steps outlined above, and would provide a substantive contribution to the development of a large-scale AI platform for gene editing technologies. 

The code implemented in GeneBank Parser creates an LLM accessible genetic data structure focused on optimizing the representation of a given gene sequence from GeneBank files. This representation has several applications within the synthetic biology landscape - including curating more effective libraries of sequences and feature analysis - in addition to its use in an eventual AI interface for Plasmid Design. Researchers would be able to use the GeneBank Parser workflow to create libraries of their own experimental data to fine-tune future models to reflect their past data.

With the completion of Genbank parser, the next steps would be implementing functions for integration with the various bioinformatics tools listed above for the various actions and utilize machine learning algorithms to consistently optimize for the best plasmid design. Finally, a query handler with OpenAI API integration needs to be written. This would account for all the dependent components of the end goal PlasmidInteract Function, hence allowing for its creation.

Integrating LLMs to automate Plasmid Design requires a complex combination of computational power and a thorough understanding of the biological representation of our data.
