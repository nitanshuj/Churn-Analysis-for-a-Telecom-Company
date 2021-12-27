# Case Study 3 - Telecom Churn - Identifying Variables Responsible for Churn

----------------------------------------------------------------------
----------------------------------------------------------------------

This was a Case Study, I completed as a part of my Post Graduate Diploma in Data Science from Upgrad & IIIT-Bangalore. The dataset and the Problem Statement was provided to me by Upgrad.

Group Size - 2

The Datasets for this Case Study were provided by Upgrad:

Telecom Churn Data: 
https://drive.google.com/file/d/1_MCMxSQgKITBADky7eQHspAtRlvnmlHF/view?usp=sharing


`The report that follows is my explanation of what was performed in the Case Study.`

-----------------------------------------------------------------------

## Business Understanding 
---------------------------
In the telecom industry, customers are able to choose from multiple service providers and actively switch from one operator to another. In this highly competitive market, the telecommunications industry experiences an average of 15-25% annual churn rate. Given the fact that it costs 5-10 times more to acquire a new customer than to retain an existing one, customer retention has now become even more important than customer acquisition. 

For many incumbent operators, retaining high profitable customers is the number one business goal. 

To reduce customer churn, telecom companies need to predict which customers are at high risk of churn. 

### Defining Churn
There are two main models of payment in the telecom industry – <br>
- 1) postpaid (customers pay a monthly/annual bill after using the services) and 
- 2) prepaid (customers pay/recharge with a certain amount in advance and then use the services).

In the postpaid model, when customers want to switch to another operator, they usually inform the existing operator to terminate the services, and you directly know that this is an instance of churn. 

However, in the prepaid model, customers who want to switch to another network can simply stop using the services without any notice, and it is hard to know whether someone has actually churned or is simply not using the services temporarily (e.g. someone may be on a trip abroad for a month or two and then intend to resume using the services again). 

Thus, churn prediction is usually more critical (and non-trivial) for prepaid customers, and the term ‘churn’ should be defined carefully.  Also, prepaid is the most common model in India and Southeast Asia, while postpaid is more common in Europe in North America.

This Case Study is based on the Indian and Southeast Asian market.

There are various ways to define churn, such as:
> **Revenue-based churn**: Customers who have not utilized any revenue-generating facilities such as mobile internet, outgoing calls, SMS etc. over a given period of time. One could also use aggregate metrics such as ‘customers who have generated less than INR 4 per month in total/average/median revenue’.<br>
The main shortcoming of this definition is that there are customers who only receive calls/SMSs from their wage-earning counterparts, i.e. they don’t generate revenue but use the services. For example, many users in rural areas only receive calls from their wage-earning siblings in urban areas.


> **Usage-based churn**: Customers who have not done any usage, either incoming or outgoing - in terms of calls, internet etc. over a period of time.<br>
A potential shortcoming of this definition is that when the customer has stopped using the services for a while, it may be too late to take any corrective actions to retain them. For e.g., if you define churn based on a ‘two-months zero usage’ period, predicting churn could be useless since by that time the customer would have already switched to another operator.


`**For this case study we will use the Usage-Based definition to define churn.**`

### Other important Concepts
In the Indian and the Southeast Asian market, approximately 80% of revenue comes from the top 20% customers (called high-value customers). Thus, if we can reduce the churn of the high-value customers, we will be able to reduce significant revenue leakage. 

In this project, you will define high-value customers based on a certain metric (mentioned later below) and predict churn only on high-value customers. 

----------------------------------------------------------------

## Business Objectives of the Case Study
----------------------------------------------------------------
In this project, the aim will be analyze customer-level data of a leading telecom firm, build predictive models to identify customers at high risk of churn and identify the main indicators of churn. 

The dataset contains customer-level information for a span of four consecutive months - June, July, August and September. The months are encoded as 6, 7, 8 and 9, respectively. 

`The business objective is to predict the churn in the last (i.e., the ninth) month using the data (features) from the first three months. To do this task well, understanding the typical customer behavior during churn will be helpful.`

### Customer behavior during churn
Customers usually do not decide to switch to another competitor instantly, but rather over a period of time (this is especially applicable to high-value customers). In churn prediction, we assume that there are three phases of customer lifecycle:

- 1) `The ‘good’ phase`: In this phase, the customer is happy with the service and behaves as usual.

- 2) `The ‘action’ phase`: The customer experience starts to soar in this phase, for e.g., he/she gets a compelling offer from a competitor, faces unjust charges, becomes unhappy with service quality etc. In this phase, the customer usually shows different behavior than the ‘good’ months. Also, it is crucial to identify high-churn-risk customers in this phase, since some corrective actions can be taken at this point (such as matching the competitor’s offer/improving the service quality etc.)

- 3) `The ‘churn’ phase`: In this phase, the customer is said to have churned. You define churn based on this phase. Also, it is important to note that at the time of prediction (i.e., the action months), this data is not available to you for prediction. Thus, after tagging churn as 1/0 based on this phase, you discard all data corresponding to this phase.

In this case, since we will be working over a four-month window, the first two months are the ‘good’ phase, the third month is the ‘action’ phase, while the fourth month is the ‘churn’ phase.

----------------------------------------------------------------

## Data Dictionary
---------------------

| Acronyms | Descriptions |
| -------  | ------------ |
| MOBILE_NUMBER	| Customer phone number |
| CIRCLE_ID |	Telecom circle area to which the customer belongs to |
| LOC	| Local calls - within same telecom circle |
| STD |	STD calls - outside the calling circle |
| IC  |	Incoming calls |
| OG  | Outgoing calls |
| T2T |	Operator T to T, i.e., within same operator (mobile to mobile) |
| T2M   | 	Operator T to other operator mobile |
| T2O    |	Operator T to other operator fixed line |
| T2F    |	Operator T to fixed lines of T |
| T2C   | 	Operator T to its own call center |
| ARPU   | 	Average revenue per user |
| MOU   | 	Minutes of usage - voice calls |
| AON    |	Age on network - number of days the customer is using the operator T network |
| ONNET  | 	All kind of calls within the same operator network |
| OFFNET  |  	All kind of calls outside the operator T network |
| ROAM |	Indicates that customer is in roaming zone during the call |
| SPL  | 	Special calls |
| ISD   | 	ISD calls |
| RECH   | 	Recharge |
| NUM    |	Number |
| AMT   | 	Amount in local currency |
| MAX   | 	Maximum | 
| DATA  |  	Mobile internet |
| 3G   | 	3G network |
| AV   | 	Average |
| VOL  |  	Mobile internet usage volume (in MB) |
| 2G   | 	2G network |
| PCK  |  	Prepaid service schemes called - PACKS |
| NIGHT |   	Scheme to use during specific night hours only |
| MONTHLY |   	Service schemes with validity equivalent to a month |
| SACHET  | 	Service schemes with validity smaller than a month |
| *.6    |	KPI for the month of June |
| *.7   | 	KPI for the month of July |
|*.8  |   	KPI for the month of August |
|*.9   | 	KPI for the month of September |
|FB_USER |	Service scheme to avail services of Facebook and similar social networking sites |
|VBC    |	Volume based cost - when no specific scheme is not purchased and paid as per usage |
 
-----------------------------------------------------

## Methodology
----------------------------------------------------------------

- 1) - Reading and Understanding the Data

- 2) - Data Cleaning
		- Removing Non-Useful Columns
		- Dealing with Null values (Removal, Substitution)
		- `Finding High Valued Customers` - {explanation given below}
		- `Defining Churn` - {explanation given below}
				
- 3) - Feature Engineering & Variable Transformation
		- Feature Engineering
		- Outlier Treatment & Variable Transformation
			- Outlier Treatment - Binning

- 4) - Exploratory Data Analysis
	
- 5) - Outlier Treatment - treating remaining outliers, if any exists.

- 6) - Model Building
		- Model 1 - Logistic Regression:
			- Dummy Encoding
			- Train-Test Split (70-30)
			- Scaling data using Standard Scalar
			- Checking for Class Imbalance
			- Checking for Correlation between variables using Heatmaps
			- Recursive Feature Elimination and Logistic Regression Model Building
			- Treatment of Class Imbalance using the Weight of Class Method
			- Model Evaluation on the Train Set
			- Model Evaluation on the Test Set
			- Calculating the Important Metrics
		- Model 2 - Decision Tree Classifier with Hyperparameter-Tuning:
			- Correcting class imbalance using the weight of class method
			- Data Preprocessing (Dummy variable creation, Train-Test Split, Scaling)
			- Model Building (Hyperparameter tuning using gridsearch cv)
			- Model Evaluation on the Training Set
			- Looking at the Various Models Build using gridsearch cv
			- Model Evaluation on the Test Set
			- Calculating the Important Metrics
		- Model 3 - Random Forest with Hyperparameter Tuning:
			- Dummy Encoding
			- Train-Test Split (70-30)
			- Scaling (Min-Max Scaling)
			- Modelling Random forest with Class imbalance handling & Hyperparameter Tuning
			- Calculating the Important Metrics

#### Finding High Valued Customers:
Since our goal is to predict churn only for high-value customers. High-value customers are those customers who bring the maximum profit to a company.<br>
We make the use of the Pareto Principle to do that. According to the **Pareto Principle**, the **top 20% of the customers bring 80% of a company's profit**.
We define high-value customers as follows: <br>
Those who have recharged with an amount more than or equal to X, where X is the 70th percentile of the average recharge amount in the first two months (the good phase). <br>
In this case we will take top 30% customers who have done the maximum recharges.

#### Defining Churn:
We define churn as the usage of total call minutes of usage and data usage for month 9 (September).

If a customer is not calling (incoming and outgoing minutes = 0) and is not using any data (2g data usage = 0 and 3g data usage = 0), we can say that the customer has churned.
In all the other cases, the customer has not churned.

------------------------------------------------------------------------------------------

## Observations and Inferences from the Data
------------------------------------------------------------------------------------------
- We observed that Average Revenue per user is lower for people who have Churned for both Action as well as the Good Phase.
<br><br>
- We also observed that the number of people churning have very low or nil minutes of usage, as compared to customers who have not churned.
<br><br>
- We observed that the 'Onnet Usage' of customers who are about to churn decreases significantly when moving from Good Phase to the Action Phase. 
- The same can be said for the 'Offned Usage'.
<br><br>
- We observed a decrease in the number of recharges when moving from good phase to action phase.
- Monthly 2g & 3g recharges showed a slight decrease in the frequency of recharges.
- Sachet 2g packs does not show much change, whereas Sachet 3g plans show a decrease in frequency of recharge.
<br><br>
- We also observed that the customers who are about to churn show a significant decrease in their minutes of usage of both incoming and outgoing calls.
<br><br>

-----------------------------------------------------------------------------------------------------------------------

## Conclusion
-----------------------------------------------------------------------------------------------------------------------
For this case study, I implemented the three following models - 

- 1) Logistic Regression
- 2) Decision Tree (with hyperparameter tuning)
- 3) Random Forest (with hyperparameter tuning)

The metrics for the the above models are shown in the table below. <br><br>
| Model Name | Accuracy (train) | Accuracy (test) | Recall Score (train) | Recall Score (test) | F1 Score (train) | F1 Score (test) | ROC-AUC Score (train) | ROC-AUC Score (test) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Logistic Regression | 94.9% | 95.15% | 100% | 100% | 76.78% | 76.32% | 97.26% | 97.37% |
| Decision Tree (with Hyperparameter Tuning) | 91.71% | 92.21% | 0% | 0% | 0% | 0% | 50% | 50% |
| Random Forest (with Hyperparameter Tuning) | 99.99% | 99.97% | 99.88% | 99.86% | 99.94% | 99.86% | 99.94% | 99.92% |


Looking at these metrices, we find that *Random Forest Model* performed the best with similar scores on test and train data.<br>
Whereas, the Decision Tree Model did not perform very well, since it gave a nearlt zero F1 score and a 50% Area under the Curve.

Also we then derived important features that play a major role in customer churning from the provider using the Random Forest Model. The 10 most important features were - 
- 1) total_usage
- 2) arpu_action
- 3) total_ic_mou_action
- 4) std_og_mou_action
- 5) total_rech_amt_good
- 6) total_avg_recharge_amount_8
- 7) loc_ic_mou_action
- 8) arpu_good
- 9) total_og_mou_action
- 10) total_ic_mou_good

-----------------------------------------------------------------------------------------------------------------------

## Business Insights
-----------------------------------------------------------------------------------------------------------------------

- `**1)- The telecom company should pay a lot of attention of the total minutes of usage of a customer, since if a customer has started to decrease his/her time on the phone, he may be most likely to leave the network**.`


- `**2)- If the average revenue per user of a customer suddenly starts to go down, he/she is more likely to churn.**`


- `**3)- A person who performs more incoming calls is a very valuable customer, since any decrease in his/her incoming minutes gives a strong indication of churn.**`


- `**4)- The similar can be said for his STD outgoing minutes.**`


- `**5)- The total recharge amount of the Good as well as the Action phase is a strong indicator of a customer churning. If any of these starts decreasing, there is a strong probability of churn.**`

-----------------------------------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------------------------------




























