# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Nihaa Sajid, `nihaasajid@gwu.edu` Guoyue Zhou, `guoyuezhou@gwu.edu` Salaah Khan `salaah.khan@gwu.edu` Hao Ren `hao.ren@gwu.edu`
* **Model date**: August, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC_6301_Project_Group_30.ipynb](DNSC_6301_Project_Group_30.ipynb)

### Intended Use
* **Primary intended uses**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students' project submission for DNSC 6301 - Group 30.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, email `jphall@gwu.edu` for more information
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 0.22.2.post1
* **Hyperparameters or other settings of your model**: 
```
DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                       max_depth=6, max_features=None, max_leaf_nodes=None,
                       min_impurity_decrease=0.0, min_impurity_split=None,
                       min_samples_leaf=1, min_samples_split=2,
                       min_weight_fraction_leaf=0.0, presort='deprecated',
                       random_state=12345, splitter='best')`
```
### Quantitative Analysis

#### Correlation Heatmap
![correlation heatmap](https://user-images.githubusercontent.com/112105334/186714031-bb78a137-74dc-4c59-bfc3-de7f81d8212e.png)







* Heatmap is a graphical way to visualize visitor behavior data in the form of hot and cold spots employing a warm-to-cool color scheme
* Strongest postive correlation PAY_0<>DELINQ_NEXT
* Strongest negative correlation RACE<>DELINQ_NEXT (financial data is widely corrupted with racial bias)


**Metrics used to evaluate your final model**
* Area under the curve (AUC)
* Adverse Impact Ratio/Disparate Impact (AIR)

**Final Results**

* Calculated at depth = 6

| Data set| AUC|
| ------- |---|
|**Training**| 0.783722 |
|**Validation**| 0.749610 |
|**Test**| 0.7438 |


* Race variable has the greatest negative correlation with delinquency
* Calculated Adverse Impact Ratio with a cutoff of 0.18

| **AIR** | **Hispanic** | **Black** | **Asian** |
| ------- | ------------ | --------- | --------- |
|**White**| 0.83 | 0.85 | 1.00 |



#### Accuracy cutoff map
![Accuracy map](https://user-images.githubusercontent.com/112105334/187100785-18bb8602-1789-41c9-997b-9ec9c7672262.png)







* Calculated accuracy at various cut off levels
* Positive correlation between accuracy and cut off levels
* Higher cut off allows lending more money, even if accurate
* Kept cut off at 0.18 to satisfy the AIR of above 0.80 for all groups

