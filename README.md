# NYU AI-Zoning Project

The Advent of Large Language Models (LLMs) has transformed many facets of society, enabling groundbreaking applications across diverse fields. This project aims to leverage LLMs to analyze and study the zoning landscape in the United States. This current repository offers a demo model that is tested with zoning ordinances extracted from Wheaton, Illinois. 

For the latest paper please see [here](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4627587). 

For the latest data please see [here](https://www.dropbox.com/scl/fo/7ujwxl4fbzor65vu7zjku/ABezF48kL_THI_nfA35PIGQ?rlkey=0aip2l0c0hq2dvplou040hqhu&st=99ow05cq&dl=0). 

All are included in the repository besides the .env file, which you will need to create in the main AI-Zoning directory (same directory as config.yaml). A guide on what to include in you .env file, as well as documentation for all the key components of the repository, can be found in the readme folder of this repository.  

The code in this repository was containerized using Docker. To demo the code, please refer to the following guide on running containerized code: https://drive.google.com/file/d/1BiEs74T4dKHhyQI2Je3EUJxNfzEcvsD0/view?usp=sharing

# AI and Zoning: Using Large Language Models for Regulatory Analysis

This repository is dedicated to the use of Large Language Models (LLMs) for parsing zoning documents. We introduce a generative regulatory measurement approach to decode and interpret statutes and administrative documents. This project leverages LLMs to construct a detailed assessment of U.S. zoning regulations and examines the correlation between these regulations, housing costs, and construction. 

The work demonstrates the reliability of LLMs in analyzing complex regulatory datasets.

For the latest paper, please see [here](https://static1.squarespace.com/static/56086d00e4b0fb7874bc2d42/t/653b143abbdc5f5bfacf947a/1698370623319/AI_Zoning.pdf).

## Table of Contents
1. [Setup](#setup)
2. [Overview](#overview)
3. [Code Structure](#code-structure)
4. [Data Structure](#data-structure)
5. [Results](#results)
6. [Contact and License](#contact-and-license)

## Setup

### Main Dependencies
To install the Python packages required for this project, please run the following command in your terminal:

```bash
pip install -r requirements.txt
```

## Overview
Our process for using LLMs to parse zoning documents is broadly split into two steps: 1) the **Embedding Step** and 2) the **QA Step** (question-answer step). In the **Embedding Step**, we prepare the questions and relevant input text (ex. zoning codes) for the LLM inference in the **QA Step**. This entails breaking up large documents into logical chunks and embedding these chunks along with the questions for input into the LLM. Note, for a given set of questions and relevant text, this step only needs to be completed once. 

In the **QA Step**, we send each question for each municipality (question-muni pair), along with additional context if relevant, to the LLM of choice and parse its answer. The **QA Step** is wrapped by an sbatch script, which can parrallelize LLM requests and orchestrates resources, environment, and re-queuing, then delegates the actual computation to the Python file.

## Code structure
The code files that carry out the two main steps above can be split into 4 groups
1. **Configuration File** - *config.ymal* -defines paths and settings required for the embedding and LLM processes, including API keys and paths to data directories
2. **Main Code** - these files carry out the two main steps detailed above
   1. **Embedding Step** - *embeddings.py* - embeds the text and prepares it for input into the QA code
   2. **QA Step** - *QA_Code.py* - carries out the LLM answer request 
3. **Helper Functions** - these files provide the functions that are employed in the embedding and QA code
   - *helper_functions.py* - provides the main set of functions used in the QA code (this code calls on all of the ensuing helper functions)
   - *question_muni_pair.py* - establishes a class that captures the relevant data associated with and functions needed for each question-muni pair
   - *gpt_functions.py* - establishes functions necessary for interfacing with the LLMs in the embedding and QA step
   - *context_building.py* - builds the context needed to answer relevant question, such as ranking the text chunks in order of relevance to the question at hand
4. **Sbatch script** - *model_batch.sbatch* - batch processing logic for SLURM job arrays, allowing for distributed computing across nodes

## Data Structure

### Raw Data

The `raw_data` folder contains several essential datasets:

- **Sample Data.xlsx:** List of municipalities and their zoning ordinances.
- **Questions.xlsx:** List of questions used in the analysis, including binary and numerical categories.

### Processed Data

Processed data includes:

- **Embeddings:** Contains text embeddings created from raw zoning data, stored in the directory defined in `config.yaml`.
- **Model Output:** Inference results from LLMs, stored in separate folders for each model (GPT-3.5, GPT-4).
- **Enriched Sample Data:** A merged dataset of municipality characteristics, zoning regulations, and additional variables used for analysis.

## Results

The `results` folder contains the outputs of model runs and visualizations:

- **Tables Folder:** Contains Excel files with table outputs from the analysis.
- **Figures Folder:** Contains images of charts and maps generated from the zoning analysis.

Tables and figures can be recreated by running the appropriate scripts in the `Table and Figures Code` section.

## Contact and License

**Contact Information**  
For any inquiries, please contact [dm4766@stern.nyu.edu](mailto:dm4766@stern.nyu.edu).

**License**  
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

