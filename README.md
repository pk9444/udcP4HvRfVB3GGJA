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
![Apziva_P2_Methodology](https://github.com/user-attachments/assets/c20fda10-f2c6-4fde-b4bb-a614b1995bae)

## MACHINE LEARNING PIPELINE
![Apziva_P2_Pipeline](https://github.com/user-attachments/assets/28b122de-7546-46ee-91b7-c12501615110)

## RESULTS

### INSIGHTS FROM THE PERFORMANCE METRICS AND MODEL EVALUATION

- The best model - LightBGM has a 93.2% accuracy on the training data and a 92.3% accuracy on the test data - which means the best model neither underfitting nor overfitting.
- For target y=1, the model has a strong recall of ~66% on the training data and ~60% on the test data - which implies significant generalization.
- The ability to generalize can be further validated by the F-1 score which are very close in both training and test data
- However, the precision is moderate, meaning there are some false positives, because the model is prioritizing recall.
- Overall, the model balances high recall for the positive class with a reasonable drop in precision and maintains generalization on unseen data. And most importantly, it exceeds the expected accuracy of 81% - one of the main objectives.

### POST-HOC ANALYSIS
- The AUC score of ~0.93 indicates very strong separability between y=1 and y-0. An ideal classifier would score 1.0, which means the model is quite good at distinguishing between customers who will and won’t subscribe. 
- The curve rises sharply to the top-left corner i.e. the True Positive Rate (TPR or Recall) increases rapidly with only a small increase in False Positive Rate, which is a good predictive indicator for subscribers.
- AUC-PR score of 0.46 is modest. It implies moderate ability to balance precision and recall - something expected in imbalanced classification problems like this one - where its a 93% to 7% imbalance.
- Precision goes down with increment in recall. As the model tries to capture more positives (higher recall), it also predicts more false positives, thereby reducing precision. Therefore, there is a trade-off between Precision and Recall.


## BUSINESS RECOMMENDATIONS

## PRIORTY CUSTOMER SEGMENTS

#### CONTINUOUS FEATURES

1. **DURATION**

- Most likely subscribers (prob >= 70%) tend to have call durations between 500 and 1200 seconds.
- The distribution of the duration is skewed rightwatds, with fewer subscribers as we go beyond 2000-second call duration mark.

    **Implications:**

    - So, it can implied that call durations that go on longer, correlate to a greater likelihood of the customer getting converted to a subscriber. 
    - It may also imply that longer calls may have more detailed and engaging conversations, hence the better conversion rate.

    **Recommendation(s):** 
    - Target customers segments with the potential for longer interactions. Train call center agents to provide as much as insight as possible to the customer.
    - However, the agents must be very on-point, because extremely longer calls can lead to customer attrition and the customer may treat it as a overbearing.

2. **BALANCE**

- The majority of likely subscribers (prob >= 70%) have balances close to 0, but there is a noticeable tail toward higher balances.
- The peak is sharp at low balances, indicating that a higher balance is not necessarily a requirement for a customer converting to subscriber.

    **Implications:**

    - Therefore, a high balance is not a prerequisite for subscription, as many lower-balance clients still get converted to subsribers. 
    - Though customers with some positive or incremental balance still form a substantial part of likely subscribers.

    **Recommendation(s):** 
    - Avoid the assumption that only wealthy customers are like subscribers. The signalling should be inclusive across income levels.
    - Building tailored investment products might even attract greater number of customers such as loyalty programmes, discounted offers on long-term subscription and other similar offerings.

3. **BALANCE**

- The majority of likely subscribers (prob >= 70%) have balances close to 0, but there is a noticeable tail toward higher balances.
- The peak is sharp at low balances, indicating that a higher balance is not necessarily a requirement for a customer converting to subscriber.

    **Implications:**

    - Therefore, a high balance is not a prerequisite for subscription, as many lower-balance clients still get converted to subsribers. 
    - Though customers with some positive or incremental balance still form a substantial part of likely subscribers.

    **Recommendation(s):** 
    - Avoid the assumption that only wealthy customers are like subscribers. The signalling should be inclusive across income levels.
    - Building tailored investment products might even attract greater number of customers such as loyalty programmes, discounted offers on long-term subscription and other similar offerings.


