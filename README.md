# TERM DEPOSIT MARKETING

## CONTEXT

The client is an early stage startup that focuses on providing Machine Learning solutions like Fraud Detection, Sentiment Analysis and Customer Intention Prediction and Classification, primarily for the EU banking market. 
They want to build a robust ML solution that leverages incoming call center data and uses to predict whether a customer will subsrcribe to a term deposit. 

## DATA DESCRIPTION

The data comes from direct marketing efforts of a European banking institution. The marketing campaign involves making a phone call to a customer, often multiple times to ensure a product subscription, in this case a term deposit. Term deposits are usually short-term deposits with maturities ranging from one month to a few years. The customer must understand when buying a term deposit that they can withdraw their funds only after the term ends. All customer information that might reveal personal information is removed due to privacy concerns.

### ATTRIBUTES

- **age** : age of customer (numeric)
- **job** : type of job (categorical)
- **marital** : marital status (categorical)
- **education** level of customer's education (categorical)
- **default**: has credit in default? (binary)
- **balance**: average yearly balance, in euros (numeric)
- **housing**: has a housing loan? (binary)
- **loan**: has personal loan? (binary)
- **contact**: contact communication type (categorical)
- **day**: last contact day of the month (numeric)
- **month**: last contact month of year (categorical)
- **duration**: last contact duration, in seconds (numeric)
- **campaign**: number of contacts performed during this campaign and for this client (numeric, includes last contact)

**Output (desired target)**:

- **y** - has the client subscribed to a term deposit? (binary)


## PROJECT GOALS
- To build a Machine Learning model that predicts if the customer will subscribe (yes/no) to a term deposit (variable y).
- Achieve an accuracy of **>= 81%** by evaluating with 5-fold cross validation and reporting the average performance score.
- Determine the segment(s) of customers that the client should prioritize i.e. who are more likely to buy the investment product.
- Find out the features that contribute the most towards guaging what the customers buy and the client should be focusing more on.

## METHODLOGY
![Apziva_P2_Methodology](https://github.com/user-attachments/assets/8d5e75dd-d39a-4851-9ccd-4bb8b57c041d)


## MACHINE LEARNING PIPELINE
![Apziva_P2_Pipeline](https://github.com/user-attachments/assets/28b122de-7546-46ee-91b7-c12501615110)

## RESULTS

### INSIGHTS FROM THE PERFORMANCE METRICS AND MODEL EVALUATION

- The model has the almost same accuracy of ~83% on both training and test sets, almost an ideal scenario - which means it is neither underfitting nor overfitting.
- For target y=1, the model has a strong recall of ~92% on the training set and ~90% on the test set - which implies very strong generalization on unseen data.
- The ability to generalize can be further validated by the F-1 scores which are very close in both training and test sets.
- The model has a much higher recall (which is what we actually need) and lower precision, hence, there is some kind of trade-off that we need to delve further into.

### POST-HOC ANALYSIS

#### IMPORTANT FEATURES

![Apziva_P2_Feature_Importance](https://github.com/user-attachments/assets/3443f374-833e-46ee-8387-122b176c5525)

- Based on the best model's feature importances, the duration is by far the strongest predictor of the subscription likelihood by a large margin.
- After the duration, the contact i.e. the mode of communication via the client reaches out to the customer is deterministic of the target.
- Month is also a moderately predictive, meaningly there is also some sort of seasonality that affects the likelihood of subscription.
- The other remaning features like Housing, age group, loan, balance, job group, education level and marital status are less deterministic. 

#### PRECISION RECALL CURVE 

![Apziva_P2_PR_Curve](https://github.com/user-attachments/assets/41ff9419-6925-470a-a739-7d00fbcb882d)

- AUC-PR score of ~0.54 is modest. It implies moderate ability to balance precision and recall - something expected in imbalanced classification problems like this one - where its a 93% to 7% imbalance.
- Precision goes down with increment in recall. As the model tries to capture more positives (higher recall), it also predicts more false positives, thereby reducing precision. So, there is a trade-off between Precision and Recall.
- For this problem, customers that are potential subscribers are the leads. So, we need to get them rightly predicted. Hence, higher recall or the True Positive Rate (TPR) must be prioritized.

#### PRECISION-RECALL TRADE-OFF

![Apziva_P2_PRT_Curve](https://github.com/user-attachments/assets/a286b75f-8326-4d71-92d9-0d91917638cd)


- Recall stays near 1.0 until threshold ~0.5, indicating the model captures nearly all True positives at lower thresholds.
- Precision steadily improves with increasing threshold, crossing recall around threshold ~0.8, suggesting this is the near trade-off zone for balanced performance.
- Optimal threshold depends on priority of business goals — low threshold favors recall, while higher threshold favors precision. 



