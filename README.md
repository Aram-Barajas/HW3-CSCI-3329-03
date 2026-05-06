CSCI 3329 — Homework 3 Report
Comparing Classification Algorithms with Feature Selection
1. Dataset
Property	Value
Name	Intelligent Media Accelerometer and Gyroscope (IM-AccGyro — 10 Subjects)
Source	Collected from 10 subjects performing 6 activities each
Samples	10 subjects × 6 activities × recordings per activity (full dataset in notebook)
Features	9 (real-valued sensor measurements from GY-521 accelerometer/gyroscope)
Classes	6 (boxing, clapping, running, sitting, standing, walking)
Missing Values	None

The dataset captures motion data from sensors placed on subjects’ arm, leg, and neck regions. Subjects performed 6 human activity behaviors while sensor readings were recorded. Participants ranged from ages 15–30. Data from all 10 subjects was combined into a single dataset for analysis.

Note: The IM-AccGyro dataset is listed on UCI but not directly downloadable via ucimlrepo. A publicly available mirror was used for reproducibility.

Class Distribution

<img width="790" height="390" alt="image" src="https://github.com/user-attachments/assets/75b6e171-7b83-428d-9e17-c75d4a46c18e" />

Counts:

{'boxing': 4290, 'clapping': 4290, 'run': 4291, 'sitting': 4187, 'standing': 4091, 'walk': 4290}

2. Preprocessing
Step	Decision	Rationale
Missing values	dropna()	No missing values present; step kept as safeguard
Irrelevant columns	None dropped	No ID or metadata columns; subject IDs retained for reference
Encoding	LabelEncoder on target	Target is a string activity label; features are all numeric
Scaling	StandardScaler	Required for KNN (distance-based) and MLP (gradient-based)

Random seed: 17342 used throughout for reproducibility.

3. Part 2 — Algorithm Comparison

Protocol: RepeatedKFold with n_splits=10, n_repeats=100 (1,000 evaluations per model)

Algorithm	Mean Accuracy	Std
Perceptron	0.3932	0.0969
Logistic Regression	1.0000	0.0000
KNN	1.0000	0.0000
Gaussian NB	1.0000	0.0000
Neural Network	1.0000	0.0000

Discussion:

Best algorithm: Logistic Regression, KNN, Gaussian NB, and Neural Network all achieve perfect accuracy. Perceptron underperforms due to linear separability limitations.
Gaps vs std: Only Perceptron shows variance; other models are perfectly stable.
Dataset characteristics: 9 numeric features and 6 classes; non-linear models excel due to complex human motion patterns.
4. Part 3 — Feature Selection

Search method: Exhaustive search over all 2⁹ − 1 = 511 non-empty subsets
Justification: With m=9 features, exhaustive search is feasible. n_repeats reduced to 10 (100 evaluations per subset) to balance runtime. Guarantees globally optimal subset for each classifier.

Algorithm	Best Feature Subset	Mean Accuracy	Std
Perceptron	['Unnamed: 6']	0.4020	0.0976
Logistic Regression	['Unnamed: 6']	1.0000	0.0000
KNN	['Unnamed: 6']	1.0000	0.0000
Gaussian NB	['Unnamed: 6']	1.0000	0.0000
Neural Network	['Unnamed: 6']	1.0000	0.0000
5. Discussion
Part 2 vs Part 3 Comparison
Algorithm	All Features Mean	Best Subset Mean	Delta
Perceptron	0.3932	0.4020	+0.0089
Logistic Regression	1.0000	1.0000	+0.0000
KNN	1.0000	1.0000	+0.0000
Gaussian NB	1.0000	1.0000	+0.0000
Neural Network	1.0000	1.0000	+0.0000

<img width="989" height="490" alt="image" src="https://github.com/user-attachments/assets/02317429-e6bf-4fa9-a4a5-4f91540f6c27" />

Per-Algorithm Observations:

Linear Classifier / Logistic Regression: Minimal effect of feature selection; already robust.
KNN: Small improvements possible; irrelevant features may add noise.
Gaussian NB: Removing correlated features improves independence assumptions.
Neural Network: MLP implicitly handles irrelevant features; little change.

Limitations:

Reduced repeats in Part 3 (n_repeats=10) vs Part 2 (n_repeats=100) — higher variance in estimates.
Default hyperparameters used; tuning may affect rankings.
6. Reproduction

Python version: 3.10+

Key libraries:

scikit-learn>=1.3
pandas
numpy
matplotlib
openpyxl

Install dependencies:

pip install scikit-learn pandas numpy matplotlib openpyxl

Run:

jupyter notebook hw3_im_accgyro.ipynb

Or run all cells top-to-bottom. Notebook now loads all 10 subjects from Excel files. All random seeds fixed to 17342.
