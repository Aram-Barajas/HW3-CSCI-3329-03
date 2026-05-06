Aram Barajas
Professor Dongchul Kim
CSCI 3329 Homework 3 Report
1. Dataset
Name: Intelligent Media Accelerometer and Gyroscope (IM-AccGyro — 10 Subjects)
Source: Collected from 10 subjects performing 6 activities each
Samples: 10 subjects × 6 activities × recordings per activity (full dataset in notebook)
Features: 9 (real-valued sensor measurements from GY-521 accelerometer/gyroscope)
Classes: 6 (boxing, clapping, running, sitting, standing, walking)
Missing Values: None

The dataset captures motion data from sensors placed on subjects’ arm, leg, and neck regions. Participants ranged from ages 15–30. Data from all 10 subjects was combined into a single dataset for analysis.

Note: The IM-AccGyro dataset is listed on UCI but not directly downloadable via ucimlrepo. A publicly available mirror was used for reproducibility.

Class Distribution:

<img width="790" height="390" alt="image" src="https://github.com/user-attachments/assets/abf4ae03-30ff-4f7d-a07e-fd78874f9cc6" />

Counts:

{'boxing': 4290, 'clapping': 4290, 'run': 4291, 'sitting': 4187, 'standing': 4091, 'walk': 4290}

2. Preprocessing
Missing-value handling: dropna() — no missing values, step kept as safeguard
Encoding: LabelEncoder on target — target is a string label, features are numeric
Scaling: StandardScaler — required for KNN (distance-based) and MLP (gradient-based)

Random seed: 17342

3. Part 2 — Algorithm Comparison

Protocol: RepeatedKFold with n_splits=10, n_repeats=100 (1,000 evaluations per model)

Algorithm	Mean Accuracy	Std
Perceptron	0.3932	0.0969
Logistic Regression	1.0000	0.0000
KNN	1.0000	0.0000
Gaussian NB	1.0000	0.0000
Neural Network	1.0000	0.0000
4. Part 3 — Feature Selection
Search method: Exhaustive search over all 2⁹ − 1 = 511 non-empty subsets
Justification: With 9 features, exhaustive search is feasible. n_repeats reduced to 10 (100 evaluations per subset) to balance runtime. Guarantees globally optimal subset for each classifier.
Algorithm	Best Feature Subset	Mean Accuracy	Std
Perceptron	['Unnamed: 6']	0.4020	0.0976
Logistic Regression	['Unnamed: 6']	1.0000	0.0000
KNN	['Unnamed: 6']	1.0000	0.0000
Gaussian NB	['Unnamed: 6']	1.0000	0.0000
Neural Network	['Unnamed: 6']	1.0000	0.0000
5. Discussion

Part 2 vs Part 3 Comparison

<img width="989" height="490" alt="image" src="https://github.com/user-attachments/assets/1c45fa2a-00e6-4d82-a17f-5222681ccaf3" />


Algorithm	All Features Mean	Best Subset Mean	Delta
Perceptron	0.3932	0.4020	+0.0089
Logistic Regression	1.0000	1.0000	+0.0000
KNN	1.0000	1.0000	+0.0000
Gaussian NB	1.0000	1.0000	+0.0000
Neural Network	1.0000	1.0000	+0.0000

Per-algorithm observations:

Linear Classifier / Logistic Regression: Feature selection has minimal effect; models are robust.
KNN: May slightly benefit; irrelevant features can add noise.
Gaussian NB: Gains from removing correlated features that violate independence assumptions.
Neural Network: MLP can implicitly ignore irrelevant features; little change.

Limitations:

Part 3 uses reduced repeats (n_repeats=10) vs Part 2 (n_repeats=100) — higher variance.
Models use default hyperparameters; tuning may shift rankings.
6. Reproduction
Python version: 3.10+
Key libraries: scikit-learn>=1.3, pandas, numpy, matplotlib, openpyxl

Install dependencies:

pip install scikit-learn pandas numpy matplotlib openpyxl

Run notebook:

jupyter notebook hw3_im_accgyro.ipynb

Or run all cells top-to-bottom. The notebook loads all 10 subjects from Excel files. All random seeds fixed to 17342.
