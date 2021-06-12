# Retail_Customer_Analysis
Analysis of online retail customers through segmentation using RFM (Recency, Frequency, Monetary) and K-means clustering methods


## Content Page
1. Executive Summary
2. Data Preparation and Cleaning
3. Exploratory Data Analysis
4. Analysis Findings
5. RFM Analysis
6. K-means Clustering
7. Limitations and Next Steps
8. Conclusions

## 1. Executive Summary
**Objective**

Perform traditional customer segmentation on Online Retail Data and compare it to unsupervised machine learning. Analyse results and plan next steps to drive revenue based on customer segments.

**Data Source**

Obtained from [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Online+Retail+II). Data set contains all the transactions occurring for a UK-based non-store online company retail between 01 Dec 09 and 09 Dec 11.
The company mainly sells unique all-occasion gift-ware. Many customers of the company are wholesalers

**To Do**

Step 1: Perform extensive data cleaning (combining workbooks, removing null entries etc.)

Step 2: Engineer new features and visualize online purchase data

Step 3: Obtain RFM score (Recency, Frequency, Monetary) for customers

Step 4: Train a machine learning model: Perform k-means clustering on customer data to obtain customer segments



## 2. Data Cleaning
Steps
1. Import necessary libraries
2. Read both excel sheets into Pandas Dataframes 
3. Set InvoiceDate as Dataframe’s index
![Data Cleaning](https://user-images.githubusercontent.com/74403956/121776872-be7e3180-cbc1-11eb-9037-0b69ce988c2d.png)
4. Use .describe() method on dataframe to check for sanity of data

![Picture 1](https://user-images.githubusercontent.com/74403956/121776861-b1f9d900-cbc1-11eb-8dc7-02110a716f06.png)

5. Check for and remove rows that contain negative values 

6. Remove rows for Quantity < 0


## 3. Exploratory Data Analysis
**1. Frequency Distribution of Quantity, Price, Revenue**

1. Create Revenue column: multiply Price and Quantity columns

2. Plot normalised histograms for Price, Quantity, Revenue columns to observe purchase distribution patterns
![EDA png-1](https://user-images.githubusercontent.com/74403956/121777000-7ca1bb00-cbc2-11eb-8654-c99a802c3c8e.png)
![EDA png-2](https://user-images.githubusercontent.com/74403956/121777011-84615f80-cbc2-11eb-8a51-18668d327863.png)
![EDA png-3](https://user-images.githubusercontent.com/74403956/121777017-8a574080-cbc2-11eb-9550-fe3c2bea7899.png)

*Quantity values are significantly right-skewed. None of the graphs appear normally distributed.*


**2. Quantity Sold Over Time**

Plot quantity sold over time and observe pattern.

![Quantity_over_time](https://user-images.githubusercontent.com/74403956/121782130-aebf1700-cbda-11eb-9ad1-8292be49d3f1.png)

*Large spikes in quantity sold deserve more detailed analysis. As expected there are large spikes in both revenue and quantity in the last few months of the years. The gap between revenue and quantity is also wider during this period compared to other time periods, suggesting customers are buying larger ticket items. Thus, it is important for the company to have sufficient stocks of all items, especially higher value ones in the period leading up to the holiday season.*


**3. Price versus Quantity**
Plot log-scaled Quantity versus log-scaled Price

![Price_versus_Quantity](https://user-images.githubusercontent.com/74403956/121777204-76600e80-cbc3-11eb-8336-6c2ecbaec53b.png)

*The most expensive items were purchased in the smallest quantities. Items purchased in increasingly larger quantities also have increasingly lower prices.*


## 4. Analysis Findings

**Q1:** How many invoices were issued and what are invoices with the most number of items ordered? **A:** There are 40,078 invoices. Some invoices contain over a thousand orders and could be our best customers. 

**Q2:** Who are our top customers? **A:** There are 5,878 customers. The top customer contributed over GBP 609k in revenue. 

**Q3:** What items are most frequently bought? **A:** Customers love our t-light holder, cakestands, and retrospots.

**Q4:** Where do our customers come from? **A:** Unsurprisingly UK and Ireland are the 2 countries that contributed the most revenue given our dataset is from the UK. Surprisingly Australia made the top 10.

**Q5:** How much revenue did we get from the top countries? **A:** UK is our top revenue contributor, contributing GBP 17.8m. This is followed by Ireland (GBP 6.64m) and Netherlands (5.54m).

**Q6:** What are the most popular kinds of items? **A:** Customers like the following colours the most: Pink, Blue, White, Red, Green, Ivory, Black. **A:** The following types of items are also popular with customers: Small candle, Necklace, Ceramic, Wall art, Christmas-themed, Glass, Heart-shaped.

![Wordcloud](https://user-images.githubusercontent.com/74403956/121777965-461a6f00-cbc7-11eb-8c67-89b1796aeb93.png)

## 5. RFM Analysis

**Value of RFM in customer segmentation**
These RFM metrics are important indicators of a customer’s behaviour because frequency and monetary value affects a customer’s lifetime value, and recency affects retention, a measure of engagement.

1. R (Recency): How recently did this customer purchase something from the store?

2. F (Frequency): Total number of transactions

3. M (Monetary): Total transaction value

**How is RFM performed?**
Customers are ranked based on their R, F, or M metric and then scores will be assigned based on their ranking. Each customer will be assigned a score between 1-5, and the score is based on quintiles. The higher the score, the better.

1. The more recent the purchase, the more responsive the customer is to promotions

2. The more frequently the customer buys, the more engaged and satisfied they are

3. Monetary value differentiates heavy spenders from low-value purchasers

**Computing Recency**
1. Ensure that *InvoiceDate* column is a *DateTime* object

2. Calculate number of days between invoice issue date and 01 Dec 2012 for all invoices. Store information in list.

3. Append list to original *DataFrame*

4. Create a new *Dataframe*, **Recency**, by performing groupby *Customer ID*, keeping the minimum date difference (most frequent purchase)

5. Rename column as **Recency**

**Computing Frequency**
1. Drop duplicate Invoice numbers from *DataFrame*

2. Create a new *DataFrame*, **Frequency**, by performing .value_counts() on Customer ID

3. Rename column as **Frequency**

**Computing Monetary**
1. Create a new *DataFrame*, **Monetary**, by performing groupby on *Customer ID* and *Revenue* columns.

2. Rename column as **Monetary**

**Merging Recency, Frequency, Monetary DataFrames**
1. Merge **Recency**, **Frequency**, **Monetary** DataFrames

2. Compute the quintiles for each (**Recency**, **Frequency**, **Monetary**) column and assign the RFM value for each column

3. Combine the 3 quintile values into one 3-digit *RFM value* and store it in a new column
![RFM_combined](https://user-images.githubusercontent.com/74403956/121782015-250f4980-cbda-11eb-8a9c-ae729a8e4823.png)

**Findings**
Customer with larger RFM scores are our best customers. However, given that there are 125 potential segments, we will simplify our analysis by focusing on R and F metrics. 

This will simplify our analysis to 25 segments. 

a. Hibernating: 11, 12, 21, 22

b. At Risk: 13, 14, 23, 24

c. Can't Lose: 15, 25

d. About to Sleep: 31, 32

e. Need Attention: 33

f. Loyal Customers: 34, 35, 44, 45

g. Promising: 41

h. New Customers: 51

i. Potential Loyalists: 42, 43, 52, 53

j. Champions: 54, 55


## 6. K-means Clustering
**Overview**
The k-means clustering method is an unsupervised machine learning technique used to identify clusters of data objects in a dataset.

Clusters are identified based on their distances to a centroid and the main objective of the K-Means algorithm is to minimise the sum of distances between the points and their respective cluster centroid.

**Steps**
1. Normalise the RFM dataset

2. Choose the number of clusters by plotting the Elbow Graph. From the Elbow Graph, 3 or 5 clusters look promising. We will explore both numbers of clusters.

![Elbow_chart](https://user-images.githubusercontent.com/74403956/121782218-107f8100-cbdb-11eb-96ef-81e4275c7ee3.png)

3. Fit the k-means model onto the normalised dataset and predict the clusters labels

4. Obtain cluster centres

5. Plot a scatter plot of normalised RFM datasets and cluster centres
![cluster_chart_3](https://user-images.githubusercontent.com/74403956/121782327-900d5000-cbdb-11eb-894d-6d04ef958ac2.png)
![cluster_chart_5](https://user-images.githubusercontent.com/74403956/121782342-9a2f4e80-cbdb-11eb-9771-4239870b9a11.png)

*at a glance, it looks like 5 clusters is better than 3*

## 7. Limitations and Next Steps
There does not appear to be a clear-cut relationship between the RFM clusters, and the K-means clusters. This could be due to the following reasons:

1. Our data is not spherical and. K-means is designed to handle spherical data

2. We omitted M from our RFM segmentation, whereas K-means testing takes into account all RFM.

![image](https://user-images.githubusercontent.com/74403956/121782440-08741100-cbdc-11eb-8740-1a70d0855baa.png)

3. We can explore the dataset using another clustering algorithm such as DBSCAN.

4. Both RFM analysis and K-means clustering only rely on the past shopping behaviour of customers. However, this does not sufficiently address the question of whether a new customer is likely to become a Champion or will fade away.

5. Future analysis could look at building profiles through segmenting them according to demographic detail such as age, gender, salary, preferences, etc.
