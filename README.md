# sbp-hd-ridge-regression

Ridge Regression for SBP Prediction with Apache Spark
This assignment uses Ridge Regression to predict Systolic Blood Pressure (SBP). It leverages Apache Spark and PySpark MLlib for distributed processing and includes essential steps like feature scaling, Principal Component Analysis (PCA), and cross-validation for robust model evaluation.

Files Included
Ensure the following files are in your working directory:
	•	Preprocessing.py: Prepares raw data and outputs preprocessed data for the Ridge Regression model.
	•	RidgeRegression.py: The main Python script for training and evaluating the Ridge Regression model.
	•	hd_eda.py (Optional): A script for basic statistical exploratory data analysis (EDA).
	•	d1.csv: Input data file.
	•	idp.csv: Input data file.
	•	vip.csv: Input data file.

Setup Instructions
1. Extract Files
If your project files are in a ZIP archive, first unzip them:

	unzip Ridge_Regression.zip


2. Copy Files to Hadoop
Copy all project files (RidgeRegression.py, Preprocessing.py, hd_eda.py, d1.csv, idp.csv, vip.csv) to your Hadoop local file system. 
Replace /path/to/ridge_regression_files, username, cluster_address, and /your/hadoop/folder with your specific details:

	scp -r /path/to/ridge_regression_files username@cluster_address:/your/hadoop/folder


Running the Program
1. Log In to ECS Machine and Hadoop Cluster
	First, SSH into your ECS machine, then connect to your Hadoop cluster and navigate to the directory where you copied the project files.
2. Run Preprocessing.py
	This script cleans and transforms your raw data, preparing it for the Ridge Regression model.

spark-submit \
  --master yarn \
  --deploy-mode cluster \
  Preprocessing.py \
  --input1 /user/**********/input/d1.csv \
  --input2 /user/**********/input/idp.csv \
  --input3 /user/**********/input/vip.csv \
  --output /user/**********/<output-dir>

Note: The preprocessed dataset will be saved as a Parquet file in the specified output directory.

3. Run RidgeRegression.py
This script trains and evaluates the Ridge Regression model.


spark-submit \
  --master yarn \
  --deploy-mode cluster \
  RidgeRegression.py \
  --input /user/*********/<output-dir-of-preprocessed-dataset> \
  --scale \
  --pca \
  --regParams 0.001,0.01,0.1,1.0 \
  --numfolds 10

Command-line Options for RidgeRegression.py:
	•	To run the model with raw data (no scaling or PCA), remove --scale and --pca.
	•	To run the model with scaled data, include --scale before the --regParams argument.
	•	To run the model with PCA, include both --scale and --pca.

4. (Optional) Run hd_eda.py
This program performs basic statistical EDA and outputs summaries.

spark-submit \
  --master yarn \
  --deploy-mode cluster \
  hd_eda.py \
  --input /user/**********/<output-dir-of-preprocessed-dataset> \
  --output /user/*********/<output-dir-of-eda>


After Running
After executing the programs, you will see logging messages in your console, including:
	•	Data loading and cleaning statistics.
	•	Model training runtime.
	•	The coefficients and intercept of the best model found through cross-validation.
	•	The best regularisation parameter.
	•	Evaluation metrics (RMSE, MAE, R2) for both the training and test sets.

Additionally:
	•	A CSV file with predictions will be saved to your Hadoop local file system.
	•	A Parquet file will be created as output for the preprocessed dataset.


Important Notes
	•	Ensure Spark and Hadoop are properly configured on your cluster.
	•	The script uses patient PIDs for splitting data into training and test sets to prevent data leakage.
