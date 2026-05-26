---
id: a7ab7a47-c744-4746-9855-7462729d6df3
title: ICU Machine Learning Project Documentation
---

- **Project Directory** - ~/Documents/jethro/ML<sub>NN</sub>

# Data directory

- **0<sub>Data</sub>/** : Directory for <u>unprocessed data</u> (data directly pulled from QMH cedars in xlsx format, outliers removed stored in data<sub>filtered</sub>.db)
    - xlsx files - data directly pulled from QMH cedars (w/o outlier removal)
    - data<sub>filtered</sub>.db - data with outlier removal (all data access will be from this sqlite db)
- **Data/** : Directory for processed data, which is processed into training data and testing data (each train test group contains acute data in a particular sliding window), chronic health status, and various outcomes (see manuscript)

# Packages and Modules

Description of <u>key packages and modules</u> for understanding the workflow:

1.  **ICUDataGrbber.py** - module offering various classes (Patient, Labs, Vitals, Drugs etc.) to obtain patient level data by providing encounterNumber (HN), admission and discharge time (str or pd.datetime format), must point towards sqlite db path (i.e. 0<sub>Data</sub>/data<sub>filtered</sub>.db)
2.  **Preprocessing.py** - module that performs train test split by date of admission, and generates the processed data (sliding windows and outcomes) for model training.
3.  **model<sub>utils</sub>/** - package containing modules for training and evaluating model using PyTorch Lightning framework
    - <u>dataset.py</u> - defines ICUDataset and and DataModule fit for training in Lightning framework
    - <u>models.py</u> - Particularly defines LSTM<sub>BinaryClassifier</sub>, the core model framework for this paper
    - <u>config.py</u> - contains variables such as acute<sub>components</sub> ,simple<sub>modelacutecomponents</sub>, chronic<sub>healthcomponents</sub> which are used in the training process in data-loading
    - <u>train.py</u> - python script that initates the training process
4.  **evaluation.py** - module that enables loading data and making evaluation using the LSTM model (generates confusion matrix, TP, TN, FN, FP, accuracy, precision, recall, F1, AUROC, average precision (saves the run into model<sub>evaluation</sub>/deep<sub>modelevaluation</sub>.csv)
5.  **evaluation<sub>xgboost</sub>.py** - module that enables loading data and making evaluation using the xg<sub>boost</sub> model (generates confusion matrix, TP, TN, FN, FP, accuracy, precision, recall, F1, AUROC, average precision (saves the run into model<sub>evaluation</sub>/xg<sub>modelevaluation</sub>.csv)
6.  **utils.py** (not mistakened for utility.py) - module containing quality of life functions that simplify commonly used operations on patient data (utilised in various modules):
    - extract<sub>fromepisode</sub> - pass in dataframe with chartTime column, and filters based on start time (admission time) and stop time
    - roundup<sub>datetime</sub> - roundup datetime (str) and returns nearest pd.Timestamp object to nearest hour
    - rounddown<sub>datetime</sub> - rounddown datetime (str) and returns nearest pd.Timestamp object to nearest hour
    - extract<sub>patientmetadata</sub> - takes a row from patient list, created from df.iterrows(), and returns tuple containing encounter number, admission<sub>time</sub>, discharge<sub>time</sub> \[str, str, str\]
    - one<sub>hotencodetimeseriesoncolumnsbasedonlist</sub> - custom OHE function that groups categorical data into different groups

# Outputs

- models/ - directory for saving models after training process (each model named based on {ARCHITECTURE}<sub>PREDS<sub>WINDOW</sub>OUTCOME</sub>)
- model<sub>evaluation</sub>/ - model evaluation metrics are stored in this folder
- notebooks/ - archival of past jupyter notebooks documenting the project

# Rough Data preparation Workflow

1.  **Data outlier removal** - based on clinical expertise (preset outlier defined by arbitrarily set limits), performed in Jupyter notebooks which are archived in notebooks/
2.  **Filtered data storage** - after removal, stored in sqlite database for patient level processing
3.  **Data preprocessing** - using preprocessing.py and ICUDatagrabber to generate finalised data for training

See work on <u>outlier definitions and modelling</u> in manuscript

# Future work

1.  New Data Format (stored in /media/ccmu/ICU<sub>Data</sub>) - created after I completed this project, aims to complete the research loop with a fully fledged python library for easy updating of data, feature engineering and analysis
2.  Updated python package for interacting with database - currently stored in PROJECT<sub>DIRECTORY</sub>/data<sub>migration</sub> (if possible, please kindly safe a copy of this folder for me in harddrive if you wish to work on this)
