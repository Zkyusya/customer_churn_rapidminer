## **1. Import dataset from the local computer** 

## **2. Add "Declare Missing Value" Search.** 

- Configure "Declare Missing Value" 

- Parameters attribute filter type: subset. 

- Attributes: Click the Select Attributes button.  Move payment method, age, and monthly spend from the left side to the right side. 

- Select mode parameter and change to nominal value: Type a single question mark: ? This tells Rapid Miner: "Look inside these three columns, find the text character ?, and treat it as a real missing value. 

## **3. Configure Filter Examples Operator** 

- The Filter Examples should be placed after the Declare Missing Value operator. 

- Click Filter Examples and click Add Filters in the parameters panel. 

- Change the dropdown from document to payment_method and set the next dropdown to is not missing. 

- Click Add Condition and do the same for age and monthly_Spend. 

## **4. Let’s see how many customers churned using the visualization tab** 

- Before proceeding to the visualization, it is good to understand that RapidMiner treats 1 and 0 as continuous numerical scales rather than distinct categories. Hence, we need to convert the churn column into categories before running it. 

- On the design view, search for Numerical to Binomial operator in the processes and drag it before the final results port. 

- Set the attribute filter type to single and select the churn column. Run the process. 

- Go to results tab on the top. 

- Select visualizations on the left column. 

- Select plot type as pie/donut then click aggregate data. 

- Group by churn, aggregation function (churn). 

- Select Plot style, then labels style, click show labels the input {point.y:,.0f} on the text button then enter. 

- Next, we can do a few process like data visualization. 

- For example we can visualize subscription plan VS monthly Spend and group this data by churn type using a bar plot. 

- We first need to aggregate the values (find the mean) before the visualization. 

- In the Design view, search for and drag the Aggregate operator into your process right before the results port. 

- Configure its parameters on the right panel like this:aggregation attributes: Click Edit List. Set Attribute to Monthly_Spend and Aggregation function to average. 

- Click Apply.group by attributes: Click Select Attributes. Move Churn and Subscription_Plan from the left list to the right list. Click Apply. 

Next, lets configure the bar plot. Go to results, visualization then change plot type to bar, value colum to average(Monthly_Spend). Then click the aggregate data function to activate hidden features. Put group by:Subscription_Plan, aggregate function:average, color group:churn. If you want to add title, scroll down to title and change the title appropriately. 

You can also perform other visualization techniques just to see how your data looks like. 

For now, we will move forward to training machine learning algorithms for prediction. Let’s start by splitting the data to training and testing sets. In this practical, we will use logistic regression, decision tree, and random forest and XGBoost algorithms. Under the classification report, we will evaluate on the effectiveness of these algorithms by checking the precision, recall, F1-score and accuracy. 

First, Rapid Miner needs to know which column we will be predicting. 

- a) Set Role – We will drag the set role operator after the cleaned dataset. Set attribute name:churn and target role: label. 

- b) Next, we will split the data. Drag split data operator. Click Edit Enumeration in the parameters. Under the ratio button, add two rows and put 0.7(training dataset) and 0.3(testing dataset) on each row. 

- c) We will then test our ML algorithms. Let’s start with Logistic Regression: Search and drag the Logistic Regression operator. 

- d) Drag in an Apply model operator. 

- e) Drag in performance (classification) operator. 

   - Configure the operator and check the boxes in the parameters panel on the right panel to match your required classification report. In my case, I will check the accuracy, precision, recall and f1-score. 

Now, ensure that you have the right connections by following the below steps: 

- i. From numerical to binomial, connect to split data. 

- ii. Connect the split data(top par) with logistic regression(tra) 

- iii. Logistic regression(mod) to apply model (mod) 

- iv. Split data(second par) to apply model(unl) 

- v. Apply Model(lab) to Performance(lab) 

Your final connection should look like this: Please note that I have disabled the aggregate value so it is not executed. 

After running the connection, this is the output: 

Now, instead of running one ML at a time, we will have all the four algorithms run at the exact same time and get their perfromance outputs at once. We will use the Compare ROCs Operator. Save the current process and make a duplicate of the same instead of clearing the modeling operator. In our duplicate operator, we will delete the logistic regression, apply model and performance operators from the main canvas. 

## **Connectng ROCs** 

Search for compare ROCs in the operators panel and drag it to the canvas. 

Connect the roc (Compare ROCs) operator to the final res port. 

Double click the compare ROCs operator to open its hidden inside workspace. 

Drag in the four algorithms and connect them like this. 

Now click back to the main process canvas. Click the compare ROCs operator to view its parameters on the right. On the performance metric parameter, select what you want to see on the results like accuracy, precision etc. Your final connection should look like this:- 

Now run the final connection. And click results. 

## **SUMMARY** 

From the results, we can conclude that Decision Tree has the highest predictive power for my churn dataset compared to other clustering modes like logistic regression, gradient boosted trees and random forest which are bunched together near the diagonal center line. 

For now, we will wrap this task here. I will be updating this task soonest to be able to display all the performance metrics. 

