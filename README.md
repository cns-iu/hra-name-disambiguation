# Independent Study Report: Name Disambiguation in the Human Reference Atlas Scientific Literature (HRA Lit) Knowledge Graph

**By:** Aishwarya Mocherla  
**Professor:** Michael Ginda  
**Summer:** 2024

---

## Course Outline

- **Credit Hours:** 2 credit hours
- **Number of Hours Expected to Work Each Week:** 8 hours
- **Meetings:** Weekly meetings to discuss progress
- **Contact Information:**
  - **Aishwarya Mocherla:** aimoch@iu.edu
  - **Michael Ginda (Instructor):** mginda@indiana.edu

## Learning Objectives

The primary objective of this independent study is to address one of the major challenges in developing the Human Reference Atlas Scientific Literature (HRA Lit) knowledge graph: identifying unique sets of persons and organizations associated with research publications. Accurately identifying individuals and organizations enhances the fidelity of research literature searches, aiding in expert identification for collaborations, reviews, consultations, advice, and research evaluation activities.

Name disambiguation will be the core focus, with an emphasis on implementing accurate and reliable data processing workflows to uniquely identify authors and their affiliated organizations using advanced machine learning methods from relevant literature. The project will utilize citation data from the PubMed database maintained by the Cyberinfrastructure for Network Science Center.

The final project outcome will include:
- Dataset of authors with unique IDs
- GitHub repository with all the resources required to execute the project
- Documentation of the methodology and results for reproducibility and future research

## Weekly Breakdown

- **Week 1 (17th June - 23rd June):** Literature Review
  - Onboarding and introductory presentation
  - Conduct an extensive literature review to understand current name disambiguation research, methodologies, findings, and gaps.

- **Week 2 (24th June - 30th June):** Methodology Design
  - Refine the research question and methodology based on the literature review. Finalize the project approach.

- **Weeks 3 & 4 (1st July - 14th July):** Algorithm Development
  - Start coding and implementing machine learning algorithms for name disambiguation. Document initial findings.
  - Intermediate project results presentation

- **Week 5 (15th July - 21st July):** Testing and Refinement
  - Test various name disambiguation methods against each other. Analyze results and draft documentation on methodologies and findings.
  - Final Presentation I

- **Week 6 (22nd July - 26th July):** Final Analysis and Documentation
  - Complete the final analysis and consolidate all findings into a comprehensive report. Prepare for the presentation and discuss potential improvements.
  - Final Presentation II

## Data Sources

- **GitHub Repository:** [HRA Lit Data](https://github.com/x-atlas-consortia/hra-lit)
- **Data Modeling:** [SQL Data Modeling HRA Lit](https://github.com/x-atlas-consortia/hra-lit/tree/sql-datamodeling/sql/30-data-modeling-hralit)

## Prior Work

- **Kiki's QSS Paper:** [DOI: 10.1162/qss_a_00299](https://doi.org/10.1162/qss_a_00299)
- **Nature Paper (Accepted) Preprint:** [BioRxiv](https://www.biorxiv.org/content/10.1101/2023.10.21.563417v1)
- **Building a PubMed Knowledge Graph:** Jian Xu, Sunkyu Kim, Min Song, et al. Scientific Data, 2020. [Nature Article](https://www.nature.com/articles/s41597-020-0543-2)
- **Precision Medicine Knowledge Graph:** Chandak P, Huang K, Zitnik M. Sci Data, 2023. [PubMed](https://pubmed.ncbi.nlm.nih.gov/36732524/)
- **Author Name Disambiguation Techniques for PubMed:** Sanyal, D. K., Bhowmick, P. K., & Das, P. P. Journal of Information Science, 2021. [DOI: 10.1177/0165551519888605](https://doi.org/10.1177/0165551519888605)

## Data Dictionary

### `id.csv`
- **Columns:**
  - `ident`: Unique identifier for the author (String)
  - `orcid`: ORCID ID of the author (String)
  - `source`: Source of the identifier (String)
  - `identifier`: Another identifier for the author (String)

### `name.csv`
- **Columns:**
  - `auth_id`: Author identifier (String)
  - `author_type`: Type of author (String)
  - `initials`: Initials of the author (String)
  - `fore_name`: First name of the author (String)
  - `last_name`: Last name of the author (String)

### `affiliation.csv`
- **Columns:**
  - `pmid`: PubMed ID of the publication (String)
  - `auth_id`: Author identifier (String)
  - `affiliation`: Affiliation of the author (String)

## Methodology

### Data Merging and Preprocessing

**Loading Data:**  
Data from three CSV files (`id.csv`, `name.csv`, `affiliation.csv`) was loaded into Pandas DataFrames. This step brought all necessary information into a workable format.

**Standardizing and Filling Missing Data:**  
Author names were standardized to lowercase to ensure consistency, and missing ORCID entries were filled with a placeholder (`'unknown'`). This preprocessing step handled variations and missing values in the dataset effectively.

**Data Merging:**  
The DataFrames were merged on common identifiers (`auth_id`, `pmid`). This step integrated various aspects of the authors’ information into a comprehensive dataset for further processing.

**Unique Identifier Creation:**  
A unique identifier for each author entry was created by combining `pmid` and `auth_id`. To manage length and ensure uniqueness, a hash function was used to shorten these identifiers.

**Handling Missing Values:**  
Remaining missing values were filled with `'unknown'` to ensure the dataset was complete and ready for model training.

**Removing Duplicates:**  
Duplicates were removed based on the shortened unique ID to ensure the integrity of the unique identifiers.

### Feature Engineering and Model Training

**Feature Engineering:**  
Feature engineering involves creating new features or modifying existing ones to improve the performance of machine learning models. LabelEncoder was used to transform categorical features (`auth_id`, `affiliation`, `fore_name`, `last_name`) into numerical values, which was necessary for the machine learning model to process the categorical data effectively.

**Feature Selection:**  
Feature selection is the process of identifying the most relevant features for the model, which helps in improving model accuracy and reducing complexity. The selected features for the model were `auth_id`, `affiliation`, `fore_name`, `last_name`. These features were chosen based on their relevance to the author disambiguation task.

**Splitting Data:**  
Data was split into training and test sets using a 70-30 split. This split allowed the model to be trained and evaluated on unseen data.

**Model Selection:**  
The Random Forest classifier was chosen for its robustness to overfitting, ability to handle a large number of input variables, and its ensemble learning nature. This model combines multiple decision trees to improve accuracy and robustness, making it suitable for the complex dataset.

**Model Training:**  
The Random Forest model was trained with 100 estimators. This training involved fitting the model to the training data to learn patterns and relationships within the features.

**Evaluation:**  
The model was evaluated using accuracy score and classification report. These metrics provided insights into the model’s performance, highlighting areas where the model excelled and where improvements were needed.

**Saving Results:**  
The predicted unique identifiers for the authors were saved to a CSV file, including relevant author details. This ensured that the processed data was documented and ready for further analysis or use in the HRA Lit knowledge graph.

### Challenges and Solutions

**Handling Large Data:**  
Initially, work was limited to 100 entries to establish a base model due to the large dataset size. This approach allowed the methodology to be tested and refined before scaling up using chunk processing.

**Machine Learning Model Selection:**  
A custom Random Forest model was chosen due to its ability to handle diverse and complex features relevant to author disambiguation. This model’s ensemble nature and robustness to overfitting were crucial for the project’s success.

**Performance Metrics Warnings:**  
Warnings about precision and recall being ill-defined for certain labels were encountered. These warnings often occur when some classes have no true or predicted samples. This was addressed by focusing on improving data preprocessing and feature selection to ensure balanced classes and accurate model predictions.

**Integration of Additional Methods:**  
Exploration of using Large Language Models (LLMs) for data cleaning was conducted to enhance input data quality. Techniques like tokenization, noise removal, normalization, and lemmatization were considered to improve the overall performance of the model.

**ORCID Consideration:**  
Initially, ORCID identifiers were considered for better accuracy. Testing with data that included ORCID IDs showed better accuracy, but due to the limited availability (only about 30% of the authors had ORCID IDs), it was decided to proceed without relying on them extensively. This decision ensured the approach could be generalized to the entire dataset.

## Future Work

**Scaling Up:**  
Chunk processing will be implemented to handle the entire dataset, testing model scalability and efficiency to ensure robustness.

**Advanced Model Development:**  
More advanced machine learning models such as Graph Neural Networks (GNNs) and Heterogeneous Information Networks (HINs) will be explored to improve disambiguation accuracy.

**LLM Integration:**  
Data cleaning will be refined using LLM
