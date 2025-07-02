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
![Apziva_P2_Methodology](https://github.com/user-attachments/assets/d4d17678-5c7c-4fa9-af74-72f7f37099db)


## MACHINE LEARNING PIPELINE
![Apziva_P2_Pipeline](https://github.com/user-attachments/assets/28b122de-7546-46ee-91b7-c12501615110)

## RESULTS

### INSIGHTS FROM THE PERFORMANCE METRICS AND MODEL EVALUATION

#### BASELINE MODEL TESTING
- The XGBoost is marginally more consistent in terms of the accuracy compared to the LightBGM.
- The LightBGM greatly overfits - the Precision and Recall are very hight on the training set but very low on the test set.
- The XGBoost has higher precision and lower recall, in both training and test set - we need high recall, especially on test/unseen data.

#### POST HYPERPARAMETER TUNING AND THRESHOLDING
- At a threshold value of 0.22, we achieve the highest possible F-1 measure of 56% i.e. the best balance between the precision and recall.
- At this F-1 measure and threshold, the precision is ~42% and recall is ~67% for y=1 i.e. subscribers.
- This means that at this configuration, the tuned XGB correctly predicts almost 2/3rd customers that will subscribe to the investment product.
- This is a fairly good performance metric and on top of that, even the accuracy has reached back to ~92%, higher than what we recorded previously.
- Hence, this is our final model - the tuned XGBoost with a threshold of 0.22.

### POST-HOC ANALYSIS

#### IMPORTANT FEATURES

![Apziva_P2_Feature_Importance](https://github.com/user-attachments/assets/e5b0b147-46f1-4691-8310-7600a53de53a)

- Based on the best model's feature importances, the duration is by far the strongest predictor of the subscription likelihood.
- After the duration, the contact i.e. the mode of communication via the client reaches out to the customer is deterministic of the target.
- Month is also a moderately predictive, meaningly there is also some sort of seasonality that affects the likelihood of subscription.
- The other remaning features like Housing, age group, loan, balance, job group are less deterministic. 

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
