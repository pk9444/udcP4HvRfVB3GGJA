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


## BUSINESS RECOMMENDATIONS

### WHAT MAKES THE CUSTOMERS SUBSCRIBE?

#### OBSERVATIONS
Based on post-hoc feature importance analysis, these features have the highest influence on the model’s decision-making:

1. **Call Duration is the Most Critical Factor**
- Customers who stay longer on the call are significantly more likely to subscribe.
- This implies that engagement quality and agent communication during the call is a decisive influence.
- **Strategy**: Train call agents to focus on keeping customers engaged and prolonging but meaningful conversations.

2. **Month of Campaigns Matters**
- Month features in March, Oct, April, Aug, June. Since, March and Oct have very few observations, focus on April, Aug and June. 
- This indicates that subscriptions exhibit some kind of seasonality by month in terms of campaign effectiveness.
- **Strategy**: Invest more resources and targeted campaigns during high-performing months like April, August and June.

3. **Mode of Contact is a Key Driver**
- Cellular mode of contact often appears as among higher ranked feature compared to telephone or unknown.
- Customers contacted via cellular means tend to convert to subscribers more often than telephone.
- **Strategy**: Prioritize cellular outreach to customers.

4. **Loan, Housing and Job Group Show Minor Influence**
- Features like housing, job and loan have smaller feature importances but are still relevant.
- These might reflect financial status, which could influence interest in financial products like deposits.
- **Strategy**: While not top determiners, it maybe worth considering financial flags as supporting features.

#### SUMMARY 
Focus on longer but meaningful conversations, use cellular contact methods, and time the outreach during March or October to maximize success.

Follow this quick rule to build a marketing strategy: <br>
    -> IF: call_duration is [>600s] AND contact == cellular AND month in [apr, march] -> prioritize outreach.

### WHICH CUSTOMER SEGMENTS SHOULD BE PRIORITIZED?

#### OBSERVATIONS

Based on the insights from Actual vs. Predicted % of Subscribers, the following customer segments emerge as prime targets:

**Contact Type: Cellular**
- The actual subscription count is the highest among customers contacted via cellular.
- The model also predicts a strong subscription behavior for this customer segment.

  **Strategy**: Prioritize outreach through cellular mode of communication. 

**Duration: >600 seconds (600-1000s, >1000s)**
- Longer but meaningful conversations with the customers are strongly associated with likelihood of subscription.
- The model also predicts high conversions, especially for 600-1000s.

  **Strategy**: Focus more on longer, personalized interactions but ensure conversations are meaningful or it might backfire and lead to customer churn.

**Age Group: 30-40 and <30**
- These age groups show higher actual subscription rates.
- The model predictions are aligned, especially for the 30-40 age group segment.

    **Strategy**: Target campaigns for the investment product to catered to younger demographics, mostly under 40.

**Average Yearly Balance: Low to Medium**
- Customers with low (<= 500 USD) or medium (500 USD-2000 USD) account balances actually tend to subscribe more.
- Predictions for these balance segements also closely match actual subscriptions in these groups.

  **Strategy**: While balance is not the main deterministic feature, do not exclude lower/medium balance clients - they are responsive.

#### SUMMARY

Prioritize customers below 40 years, contact them via cellular modes and have detailed yet meaningful conversations, investment product.
