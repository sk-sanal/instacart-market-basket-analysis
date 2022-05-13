# Instacart Market Basket Analysis

## Introduction
---------------------------
As per a Mckinsey report, 60%+ Europeans tried a new shopping experience during the pandemic, and most are expected to continue with new habits. The Instacart project gave me an opportunity to gain more insights into consumer buying behavior in the gig economy against the backdrop of current economic environment, apart from applying my newly learned data science skills. Moreover, being an Instacart customer myself (while I was in Toronto), I was excited to see my real life experience being engineered as a feature!!

## Business Problem
------------------
The business requirement here is to predict which previously purchased products will be in customer’s next order. Our aim here is to predict if a user will reorder previously purchased products, it does not involve suggesting new products to customers. The final aim is to drive higher sales for the company via improved customer experience, increased customer interaction and suggesting relevant products to consumers, which saves time. Net/net maximize their return on investment (ROI) based on the information gathered from customers' purchases and preferences. I must say making customers switch to online grocery shopping is not an easy proposition, while the pandemic provided tailwinds, the key for companies like Instacart is to retain existing customers and drawing news consumers to the platform. Hence, a recommender system helps in improving the shopping potential of an existing user. 

***What's in it for Instacart?***
In my opinion this is Instacart's first attempt to develop a customized taste profile of its customers (though not explicitly stated). Further, customer insights can also help retailers to advertise and sell their products in a better way on the platform.

## Business Model
------------------
Instacart provides on-demand grocery delivery by connecting customers with personal shoppers, who purchase products from the retailer specified by the customer and deliver it to the doorstep within a stipulated time. Instacart does not own any grocery store, it provides a platform for grocery retailers to sell their products on the platform. Instacart earns from the delivery fees and placement fees from the companies. 

## ML Formulation of the Problem
------------------
We have to recommend users the products, the user has already purchased. We are going to pose this problem as supervised learning binary classification problem. With binary labels being whether the previously ordered product will be reordered or not. We have to come up with features which summarizes the information about the user’s previous orders and the products contained in them.
This problem is different from the classical recommendation problem (like Netflix recommendation system), where we generally recommend users, products (or movies) that are similar to the products that the users have already purchased( or viewed.

0 - indicates that the user did not reorder  
1 - indicates that the user reordered

## Data Source
------------------
The data that Instacart opened up include orders of 200,000 Instacart users with each user having between 4 and 100 orders. Instacart indicates each order in the data as prior, train or test. Prior orders describe the past behaviour of a user while train and test orders regard the future behaviour that we need to predict. For the train orders Instacart reveals the results (i.e. the ordered products) while for the test orders we do not have this piece of information. Moreover, the future order of each user can be either train or test meaning that each user will be either a train or a test user.
We are provided with 6 corelated CSV files/tables, namely:
1) Orders: This table includes all orders, namely prior, train, and test. 
2) Order_products_train: This table includes training orders and indicates whether a product in an order is a reorder or not (through the reordered variable) 
3) Order_products_prior: This table includes prior orders. It indicates whether a product in an order is a reorder or not (through the reordered variable). 
4) Products: This table includes all products and their related information. 
5) Aisles: This table includes all aisles and their related information
6) Departments: This table includes all departments and their related information.

## Other repository contents
------------------
Jupyter notebooks:  

* InstacartAnalysis_EDA: contains the code for data cleansing, EDA  
* InstacartAnalysis_feature_engineering: contains the code for feature engineering  
* InstacartAnalysis_Models: contains the code for model training and evaluation

## Exploratory Data Analysis
------------------
A few insights gathered from performing EDA:
* We can note there that the maximum number of reorders are for the standardized products, such such as produce, dairy egss, snacks, frozen. Conversly, items which are more personalized such as household, babies, personal care, alcohol tend to have less reorders. 
* Add to cart order is negatively correlated with reordered. This implies, if add to cart order is lower then the probability of product being reordered is more.  
* Order number is positively correlated with reordered. The higher the number of orders, probability of the order having products to be reordered is higher. 
* Order number is negatively correlated with days since prior order. This implies, the more the gap between orders being placed less is the number of orders placed.

![alt text](./images/days_since_prior_order.png)

![alt text](./images/reorder_vs_add%20to%20cart.png)


## Feature Engineering
------------------
I created 3 types of features:
* User Features: What is an user like?
* Product Features: What is the product like?
* User_x_Product Features: What is an user's buying pattern regarding product?

![alt text](./images/feature%20importance.png))

To explain some top features:
* uxp_order_rate: This is the total orders per product divided by total orders of the user
* user_average_basket: count of a particular product ordered by an user/total number of orders
* prod_reordered_ratio: the reordered ratio of each user and product combination
* user_order_starts_at: At what number does the user's order number starts at ? This tells us if the user is an old customer or a new customer 
of instacart
* reordered_ratio: users reordered ratio
* mean_days_since_prior_order: average days between each order of users
* std_cart_position: standard deviation of product's add to cart position
* avg_cart_position: average of product's add to cart position
* total_num_orders: total number of orders placed by the user
* max_cart_position: maximum of product's add to cart position
* avg_no_prds_each_purchase: Average number of products brought by user in each purchase

## Modeling
------------------
This is a binary classification problem. The dataset which we have for training and testing is highly imbalanced, with 7,645,837 products with no-reorders (90%) and 828,824 products with reorders (10%). I used logistic regression method because it comes a built-in method of handling imbalanced classes, which is using the class_weight parameter to weight the classes to make certain we have a balanced mix of each class.

## Evaluation
------------------
Accuracy would not be the right measure to evaluate the model due to imbalanced data set. We need to use the F1 score. I achieved a F1 score of 0.18 on the training and the test dataset

## Next Steps
------------------
The goal is to further improve the F1 score. I want to create some more features to improve the predictive performance of the model. Further, explore other options to handle imbalanced data set, which includes hyperparameter tuning and exploring other supervised classification models



