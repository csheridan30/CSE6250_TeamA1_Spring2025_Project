# CSE6250_TeamA1_Spring2025_Project
Final Project for Team A1 for GA Tech CSE 6250 Big Data For Health Spring 2025

Video presentation: https://youtu.be/UembQgz2m1Y


Please note: This code was designed in and intended for use in Google Colab.
To run this code in Google Colab, you will need at least 8 GB worth of space in your drive.  7.21 of the 8 GB will be taken by the raw data file.
If you wish to skip the data preprocessing step, we provide the preprocessed data for seed 28 at https://drive.google.com/drive/folders/111w-Jw_LchYf1MPTUIuVb60EPUVi53fN?usp=sharing.  This reduces the total file size to approximately 1GB.

The project is stored in the following file structure:

BD4H_Project: Default Folder containing the ProjectCode.jpynb Jupyter Notebook, the ReadMe.txt, and all subfolders for this project.

- For reference: mp stands for mortality prediction.  ci stands for causal inference.  These acronyms are used to denote [cause] in file names.
- [seed] denotes the seed used in preprocessing the dataset.  The pre-tested seeds are 5, 24, 28, 50, and 76.

 -> Outputs: The saved outputs from running the machine learning models, saved in the format of [model]_[purpose]_[seed].txt.  For the mortality prediction model, the training and validation metrics are saved as mp_train_val_metrics_[seed].txt.  For the causal inference model the training and validation metrics are saved as ci_train_val)metrics_[seed].txt and the average treatment effect using root mean survival time is saved as ci_ATE_[seed].txt.
  -->>The Graph subfolder contains the training and validation loss curves and mean average error graphs for the mortality prediction and causal inference models.  The confusion matrix is also saved for the mortality prediction model.  


 -> Models: The saved pre-trained machine learning models for mortality prediction and causal inference.  Models are saved both fully and as dictionaries in the form of [cause]_model_[seed].pth and [cause]_model_dict_[seed].pth respectively.  
 
 -> Documents: Relevant documents for the project.  Contains the research paper to replicate, project proposal, team formation, and final papers for the project.
 
 -> Data: The folder containing the raw and processed data used in the machine learning models.  Folder contains the raw data, all_hourly_data.h5, and a csv version of the dataframe for each seed, saved as mimic3_df_[seed].csv.  Folder also contains the preprocessed subfolder for storing preprocessed data outputs from the code.
	-->> preprocessed: Folder containing the outputs of the preprocess data functions of the code, separated by purpose.  .npy files for the vitals, treatment variables, survival times, patient index, hourly indexes, hazard function calculations, demographics, treatment codes, the code lookup, and the code index are contained in this folder.  Folder contains additional backup copies of the dataframe csvs.




How to obtain the Raw Data:
To obtain the data from MIMIC-III required for this project, you may download and run MIMIC Extract with the default parameters from https://github.com/MLforHealth/MIMIC_Extract
or a pre-processed copy of the data from MIMIC Extract is available at console.cloud.google.com/storage/browser/mimic_extract?invt=Abt9-A 

The all_hourly_data.h5 file must be placed in the Data folder.


How to run the code:
All project code is contained in the ProjectCode.jpynb file.
Run the environment setup section and specify a seed for the dataset.  Seed 28 is used by default.  Using one of the noted tested seeds will allow you to skip the model training step if desired.

After environment setup, mount the jupyter notebook to Google Drive.  Be sure to change the path to the working directory of your drive where the project files are stored.  By default, this is "/content/gdrive/My_Drive/BD4H_Project"
The debug block following will allow you to determine if the file structure is correct, but is not required to run the rest of the code.

The Preprocess Data block must be the first time the code is executed and each time the seed is changed.  This block takes approximately 15 minutes to run and is unaffected by the use of a GPU.
To ensure that the dataset matches that from the research paper, and seeing as preprocessing the data is not the goal of the project, the preprocess data section was taken from the paper's original code source.  

Once the Preprocess Data block has completed, run the Read Processed Data block to get the dataframes used in the machine learning code.

The Dataset Statistics block can be used to get summary statistics of the dataset.  Running this block is not required.

Initialize the Performance Metrics by running the Performance Metrics Block.

The Machine Learning Model block is separated into two subblocks of code, one for the mortality prediction and one for causal inference.  Running either block will create, train, plot, save, and evaluate the model.  Running either block takes approximately 2.5 hours when using the Google Colab T4 GPU accelerator.  
To save time and prevent having to retrain the model, at the end of both blocks the saved model can be loaded from the .pth file in the Models folder.  To load a pre-trained model, run the initialize model subblock, then the second code block of the Save and Load Model subblock to load a pre-trained model.  Models for seeds 5, 24, 28, 50, and 76 are included with this project.  
After running the load model code, you may run the model evaluation block and the Obtain Treatment ATE block for the Causal Inference model.

For Causal Inference the Obtain Treatment ATE block will obtain the effect of simulated treatment on patient survivability.  This is included both as outcome regression for the effect on each patient for a random subset of 10 patients and as the effect on the root mean survival time for the same random subset of patients.  Outcome Regression and Root Mean Survival Time may be run separately, though running them together in any order will ensure that the same random subset of patients is used in both calculations.
