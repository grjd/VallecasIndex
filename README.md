# Selecting the most important self-assessed features for predicting conversion to Mild Cognitive Impairment with Random Forest and Permutation-based methods

This repository contains the code and the supplementary material of the paper of the paper:

Jaime Gómez-Ramírez, Marina Ávila-Villanueva, Miguel Ángel Fernández-Blázquez, **"Selecting the most important self-assessed features for predicting conversion to Mild Cognitive Impairment with Random Forest and Permutation-based method"**.
**Abstract**
*Alzheimer’s Disease (AD) is a complex, multifactorial and comorbid condition. The asymptomatic behavior in the early stages makes the identification of the disease onset particularly challenging.
Mild cognitive impairment (MCI) is an intermediary stage between the expected decline of normal aging and the pathological decline associated with dementia. The identification of risk factors for MCI is thus sorely needed. 
Self-reported personal information such as age, education, income level, sleep, diet, physical exercise, etc. are called to play a key role not only in the early identification of MCI but also in the design of personalized interventions and the promotion of patients empowerment. 
In this study we leverage on The Vallecas Project, a large longitudinal study on healthy aging in Spain, to identify the most important self-reported features for future conversion to MCI. Using machine learning (random forest) and permutation-based methods we select the set of most important self-reported variables for MCI conversion which includes among others, subjective cognitive decline, educational level, working experience, social life, and diet. Subjective cognitive decline stands as the most important feature for future conversion to MCI across different feature selection techniques.*

## Overview
This repository contains:

    All the python code used to generate the results and plots of the paper 
    Report files generated by the random forest classifier implemented in the paper

You can download all the files as a zip using the "Clone or Download" button on the main repository page, or you can click through the file listings to view the code directly in GitHub.

## Requirements

### Python Packages
The code relies on the following python3.X+ libs:
* numpy
* pandas
* sklearn
* matplotlib
* seaborn
* graphviz
* eli5
* pdpbox
* shap

 ## Usage 
 
```python
import paper
paper.main() 
```
Select a seed for reproducibility `np.random.seed(42)`. Declare in `csv_path` the dataset. The csv file pvix_shufs.csv contains a shuffled truncated version. For those who also like to have the complete dataset clone the repository and contact the developers, the eamil email address in the Git log.

Perform the first feature selection method implemented in the paper _feature_selection_filtering_
```python
# X Input features
# y Target feature 
# oneintenrule N top features 
oneintenrule = 10
selector, covmatrix = feature_selection_filtering(X, y, oneintenrule)
```
Build _Random Forest Classifer_ using Cross Validation, either grid search or random search can be used.
```python
# X Input features
test_size = 0.20 
# split data set in training and test sets in proportion test_size = 0.20 
X_train, X_test, y_train, y_test = split_training_test_sets(X+y, y.name, test_size)
grid_search = Run_GridSearchCV(X_train, y_train)
# Select the best estimator in the cross validation
rf = grid_search.best_estimator_
# Select the most important features in the Random Forest
feature_top_names = select_important_features_fitted_RF(rf,X_train, oneintenrule)
```
Build permitation based methods: 
* _eli5_
* _Partial Dependence Plots_ 
* _Shap values_

```python
# Feature importance with permutation using eli5 library
# Args: model, X,y 
# Output: sklearn.PermutationImportance object
permutation_importance_eli5(model, X, y)
```

```python
# Creates partial dependence plots
# Args: model, X
# Output: 
partial_dependence_plots(rf, X_train)
```

```python
# SHAP (SHapley Additive exPlanations) to explain the output of machine learning model.
# SHAP values represent a feature's responsibility for a change in the model output
# Args: model, X
# Output: shap values 
shap_values = shap_explanations(model, X)
```
