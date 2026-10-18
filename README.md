# Investigation of Users’ Behaviour Online: Focusing on Online Scams

## HIT401 Capstone Project – Group 31

This repository contains the source code, prepared datasets, experimental results and research prototype developed for the HIT401 capstone project.

The project investigates whether machine-learning models trained on familiar email sources can identify scam emails from unfamiliar sources. It also examines selected persuasion cues in scams that were caught or missed by the models.

## Project Overview

The project compares two text representations:

1. **TF-IDF features with Logistic Regression**
2. **Frozen all-MiniLM-L6-v2 sentence embeddings with Logistic Regression**

The main experiment uses source-separated data:

| Purpose | Scam source | Legitimate source |
|---|---|---|
| Model development | Nazario | Enron |
| External testing | Nigerian Fraud | SpamAssassin |

This design tests whether the models can transfer to email sources that were not used during training.

The project also examines eight behavioural language cues:

- Urgency
- Authority
- Fear or threat
- Reward or gain
- Trust or credibility
- Financial request
- Credential request
- Relationship or emotion

## Repository Contents

### Source Code

| File | Description |
|---|---|
| `HIT401_Simple_Scam_Project_Part1.ipynb` | Loads and prepares the source datasets, removes empty records and exact duplicates, constructs balanced source-separated groups and exports the prepared data. |
| `HIT401_Simple_Scam_Project_Part2.ipynb` | Trains and evaluates the TF-IDF and Logistic Regression baseline on internal validation and external-test data. |
| `HIT401_Simple_Scam_Project_Part3.ipynb` | Compares the eight behavioural cues in caught and missed external scams. |
| `HIT401_Simple_Scam_Project_Part4.ipynb` | Compares the traditional TF-IDF baseline with frozen MiniLM sentence embeddings. |
| `HIT401_Simple_Scam_Project_Part5_False_Alarm_Improvement.ipynb` | Evaluates alternative decision thresholds and examines the trade-off between false alarms and missed scams. |
| `HIT401_Simple_Scam_Prototype.ipynb` | Creates an interactive Gradio prototype that predicts whether an email is a scam and displays selected behavioural cues. |

### Prepared Data and Results

| File | Description |
|---|---|
| `hit401_prepared_data.zip` | Contains the prepared `training_data.csv` and `external_test_data.csv` files. |
| `hit401_part2_results.zip` | Contains baseline metrics, external predictions, caught scams, missed scams and influential TF-IDF features. |
| `hit401_part3_results.zip` | Contains the final eight-cue behavioural-analysis results. |
| `hit401_part4_results.zip` | Contains the TF-IDF and MiniLM comparison results and predictions. |

## Original Dataset

The original dataset is not stored in this repository because of its large file size.

**Dataset:** Phishing Email Dataset  
**Source:** https://doi.org/10.34740/kaggle/ds/5074342  
**Licence:** Creative Commons Attribution–ShareAlike 4.0 International  
**Kaggle page:** https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset

The following source files are required by Part 1:

```text
CEAS_08.csv
Nazario.csv
Nigerian_Fraud.csv
Enron.csv
SpamAssasin.csv

After downloading the dataset:
1. Create a folder named data.
2. Place the five CSV files inside the folder.
3. Compress the folder as capstone_data.zip.
4. Upload capstone_data.zip when Part 1 requests it.
The expected structure is:
capstone_data.zip
└── data
    ├── CEAS_08.csv
    ├── Enron.csv
    ├── Nazario.csv
    ├── Nigerian_Fraud.csv
    └── SpamAssasin.csv

The spelling SpamAssasin.csv is retained because this is how the file is named in the supplied dataset.
Software Requirements
The notebooks were designed for Google Colab but can also run in a suitable Jupyter Notebook environment.
Main requirements:
Python
pandas
numpy
matplotlib
seaborn
scikit-learn
sentence-transformers
gradio

The prototype installs the following version ranges:
sentence-transformers>=3.0,<6
gradio>=4.44,<6

An internet connection is required to install packages and download the MiniLM checkpoint from Hugging Face.
A GPU is not compulsory. However, a Google Colab T4 GPU can reduce the time required to create MiniLM embeddings.
Running the Project
Run the notebooks in the following order.
Part 1 – Data Preparation
Open:
HIT401_Simple_Scam_Project_Part1.ipynb

Upload:
capstone_data.zip

Part 1 produces:
hit401_prepared_data.zip

This ZIP contains:
training_data.csv
external_test_data.csv

Part 2 – TF-IDF Baseline
Open:
HIT401_Simple_Scam_Project_Part2.ipynb

Upload:
hit401_prepared_data.zip

Part 2 trains the TF-IDF and Logistic Regression baseline and produces:
hit401_part2_results.zip

The archive contains:
model_metrics.csv
external_predictions.csv
missed_scams.csv
caught_scams.csv
important_words.csv

Part 3 – Behavioural-Cue Analysis
Open:
HIT401_Simple_Scam_Project_Part3.ipynb

Upload the result or data ZIP requested by the notebook and run every cell in order.
This notebook compares the frequency of the eight selected cues in caught and missed scam emails and produces:
hit401_part3_results.zip

Part 4 – TF-IDF and MiniLM Comparison
Open:
HIT401_Simple_Scam_Project_Part4.ipynb

Upload the requested prepared-data ZIP and run every cell from top to bottom.
This notebook compares:
TF-IDF + Logistic Regression

with:
all-MiniLM-L6-v2 embeddings + Logistic Regression

It produces:
hit401_part4_results.zip

Part 5 – Threshold Analysis
Open:
HIT401_Simple_Scam_Project_Part5_False_Alarm_Improvement.ipynb

Run the cells in order and supply the prediction or result files requested by the notebook.
This experiment compares alternative probability thresholds. The selected threshold of 0.55 was determined using internal validation and was then applied to the external test.
Prototype
Open:
HIT401_Simple_Scam_Prototype.ipynb

Upload:
hit401_prepared_data.zip

The notebook will:
1. Load the prepared training and external-test data.
2. Download the sentence-transformers/all-MiniLM-L6-v2 checkpoint.
3. Generate normalised MiniLM embeddings.
4. Train a Logistic Regression classifier.
5. Launch a temporary Gradio interface.
The user can paste an email into the interface. The prototype displays:
- Predicted class: scam or legitimate
- Estimated scam probability
- Detected behavioural cues
- Highlighted cue expressions
The Gradio public link remains available only while the Colab runtime is active.
Main Experimental Settings
Parameter	Setting
Random seed	42
Development split	80% training and 20% internal validation
Split method	Stratified
TF-IDF maximum features	10,000
TF-IDF stop words	English
TF-IDF n-gram range	Unigrams and bigrams
TF-IDF minimum document frequency	2
Baseline classifier	Logistic Regression
Maximum classifier iterations	1,000
Contextual model	sentence-transformers/all-MiniLM-L6-v2
Embedding dimensions	384
Prototype maximum sequence length	256 tokens
Prototype embedding batch size	64
Embedding normalisation	Enabled
Default threshold	0.50
Selected adjusted threshold	0.55


MiniLM remains frozen during the experiment. Only the Logistic Regression classifier is trained using the labelled development emails.
Labels
The project uses the following binary labels:
0 = Legitimate email
1 = Scam email

The meaning of the original positive label is not identical across every source. Therefore, the source identifier is retained in the prepared data.
Reproducibility Notes
- Fit TF-IDF only on the development-training records.
- Do not fit TF-IDF using internal-validation or external-test records.
- Use random_state=42 where specified.
- Run every notebook cell from top to bottom.
- Do not use the external-test results to select model parameters.
- Preserve the supplied filenames because later notebooks search for those exact names.
- Software-version differences may produce small numerical differences.
- The original raw dataset must be downloaded separately.
- The prepared data and result archives are included to support verification.
Limitations
The public corpora differ in age, collection method, writing style and label definition. Consequently, the measured external-performance difference may reflect both scam-language differences and source characteristics.
Only exact duplicates and exact text overlap were checked. Near duplicates, repeated templates and quoted material may remain.
The behavioural cues are based on selected keyword dictionaries. They can miss implied meaning and may match ordinary language that is not persuasive.
The prototype truncates emails that exceed the MiniLM input limit. Information appearing later in a long message may therefore be excluded.
Ethical and Privacy Considerations
The project uses publicly available secondary datasets. The email data should only be handled for legitimate academic research and in accordance with the original dataset licence.
Users should not paste private, confidential or personally identifying emails into a publicly shared prototype.
The prototype is a research demonstration, not a production security product. Its output must not be used as the sole basis for deleting, blocking or acting on an email.
The displayed behavioural cues are produced by separate keyword rules. They do not provide a complete explanation of the MiniLM classifier’s decision.
