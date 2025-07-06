# Kultra-Mega-Stores-Inventory-DSA-Capstone-Project

### Project Overview 
This project involves using Microsoft SQL server to analyze sales performance and customer base data of Kultra Mega Stores from 2009 to 2012. By analysing the various parameters in the data received, I present key insights and findings in different two case studies.  

### Data Source
The primary source of data used here is KMS Sql Case Study.csv and Order_Status.csv.

[Download KMS Sql Case Study here](https://github.com/user-attachments/files/21090991/KMS.Sql.Case.Study.csv)

[Download Order_Status here](https://github.com/user-attachments/files/21090994/Order_Status.csv)

### Tools Used
- Microsoft SQL server : [Download here](https://go.microsoft.com/fwlink/p/?linkid=2215158&clcid=0x409&culture=en-us&country=us)
- Microsoft SQL server Management studio 2022 : [Download here](https://aka.ms/ssms/21/release/vs_SSMS.exe)

### Data Cleaning and Preparation
1. **Data Loading and inspection:**

I wrote the following queries;

``` SQL ```

-----CREATE DATABASE DSA_db-------
    
    CREATE database Capstone_Project

-----IMPORT CSV FILES INTO DB----

-----------IMPORT FILE [KMS Sql Case Study]-----

-----Data type formatting-----

    Table [KMS ORDERS] (
  	[Row ID] INT,
  	[Order ID] INT,
  	[Order Date] DATE,
  	[Order Quantity] INT,
  	Sales DECIMAL (10, 2),
  	Discount DECIMAL (10, 2),
  	[Ship Mode] VARCHAR (50),
  	Profit DECIMAL (10, 2),
  	[Unit Price] DECIMAL (10, 2),
  	[Shipping Cost] DECIMAL (10, 2),
  	[Customer Name] VARCHAR (255),
  	Province VARCHAR (100),
  	Region VARCHAR (100),
  	[Customer Segment] VARCHAR (100),
  	[Product Category] VARCHAR (100),
  	[Product Sub-category] VARCHAR (100),
  	[Product Name] VARCHAR (MAX),
  	[Product Container] VARCHAR (100),
  	[Product Base Margin] DECIMAL (10, 2),
  	[Ship Date] DATE 
       )

Columns were NULL data was allowed
    
    [Unit Price]  
    Profit 

### Exploratory Data Analysis
EDA involved the exploring of the data to answer some questions about the data such as;

**Case Scenario I** 

1. Which product category had the highest sales? 
2. What are the Top 3 and Bottom 3 regions in terms of sales? 
3. What were the total sales of appliances in Ontario? 
4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers 
5. KMS incurred the most shipping cost using which shipping method?

**Case Scenario II**

6. Who are the most valuable customers, and what products or services do they typically 
purchase? 
7. Which small business customer had the highest sales? 
8. Which Corporate Customer placed the most number of orders in 2009 – 2012? 
9. Which consumer customer was the most profitable one? 
10. Which customer returned items, and what segment do they belong to? 
11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer.

I wrote the following queries

``` SQL ```

**QUESTION 1**

-----Product category with the highest sales----

    SELECT  
    	Product_category, sum(sales) as Total_sales
    from [KMS Sql Case Study]
    Group by Product_category
    order by Total_sales

**QUESTION 2**

-----top 3 Region in terms of sales----

    SELECT  * from [KMS Sql Case Study]
    
    SELECT  top 3 
    	Region, sum(sales) as Total_sales
    from [KMS Sql Case Study]
    Group by Region
    Order by Total_sales desc

-----Bottom 3 region in terms of sales

    SELECT  top 3 
    	Region, sum(sales) as Total_sales
    from [KMS Sql Case Study]
    Group by Region
    Order by Total_sales asc

**QUESTION 3**

-----Total sales of appliances in Ontario-----

    SELECT   
    	Region, sum(sales) as Total_sales
    from [KMS Sql Case Study]
    where Region = 'Ontario'
    Group by Region

**QUESTION 4**

------Revenue from customors asc(Bottom 10 customers)----

    SELECT  * from [KMS Sql Case Study]
    order by Sales asc
    
    SELECT  * from [KMS Sql Case Study]
    order by Sales desc

-----From this information, the revenue from the bottom 10 customers is low as a result of the low unit price of the products supplied to them.

-------it can be deduced that product sub-categories "Office Machines, copiers and fax", all in the technology category, has the highest sales according to top 5 statistics because of their unit prices.

------The company can scale up the revenue from these individuals by advertising and introducing more of the technology products to the low revenue customers.

------The company can also put a standard to the quantity of products ordered by customers (such as order lesser than a dozen quantity are not permitted) especially for customers that order products with low unit prices so as to scale up the Order quantity from these customers and avoid unwanted expenditure w.r.t. quantity ordered, during delivery that may incur loss.


**QUESTION 5**

------Most shipping cost incurred with respect to the shipping mode-----

    SELECT
    	Ship_Mode, sum(Shipping_Cost) as [Shipping cost]
    from [KMS Sql Case Study]
    Group by Ship_Mode
    Order by [Shipping cost] desc


**QUESTION 6**

-------most valuable customers and the products they typically purchase?

----Most valuable customers----

    SELECT top 5
    	Customer_Name, sum(Sales) as [Total sales]
    from [KMS Sql Case Study]
    group by Customer_Name
    order by [Total sales] desc

----Products they typically purchase----

    SELECT
    	Customer_Name, Product_Name, Product_Category
    from [KMS Sql Case Study]
    where Customer_Name in ('Emily Phan', 'Deborah Brumfield', 'Roy Skaria', 'Sylvia Foulston', 'Grant Carrol')
    order by Customer_Name asc

**QUESTION 7**

------Small business customer with highest sales-----

    SELECT top 1
    	Customer_Name, sum(Sales) as [Total sales]
    from [KMS Sql Case Study]
    where Customer_Segment = 'Small Business'
    group by Customer_Name
    order by [Total sales] desc

**QUESTION 8**

-----Corporate Customer with the most number of orders in 2009 – 2012?-----

    SELECT top 1
    	Customer_Name, count(Order_ID) as [Total Order]
    from [KMS Sql Case Study]
    where Customer_Segment = 'Corporate'
    group by Customer_Name
    order by [Total Order] desc

**QUESTION 9**

------Most profitable consumer customer-----

    SELECT top 1
    	Customer_Name, sum(Profit) as [Total Profit]
    from [KMS Sql Case Study]
    where Customer_Segment = 'Consumer'
    group by Customer_Name
    order by [Total Profit] desc
    

**QUESTION 10**

-----Customers that returned items and the segments of items returned-----
 
------IMPORT CSV FILES INTO DB-----

-----IMPORT Order_status----

    SELECT * from Order_status
    
    SELECT * from [KMS Sql Case Study]
  
    
    SELECT 
    	[KMS Sql Case Study].Customer_Name, 
    	[KMS Sql Case Study].Customer_Segment,
    	Order_status.Status
    from [KMS Sql Case Study] join Order_status
    on Order_status.[Order_ID] = [KMS Sql Case Study].[Order_ID]


**QUESTION 11**

------If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer


    SELECT 
    Order_Priority, Ship_Mode, Shipping_Cost
    	from [KMS Sql Case Study] 
    order by Shipping_Cost desc
    
    SELECT 
    Order_Priority, Ship_Mode, Shipping_Cost
    	from [KMS Sql Case Study] 
    order by Shipping_Cost asc

------Firstly, it appears more money was spent on delivery truck from the table above, as such, the company did not appropriately spend shipping costs considering the slow delivery of "Delivery truck". 

------Meanwhile based on the order priority, more shipping costs were incurred on critically high order priority according to the top 7 statistics. So with the slow delivery of "delivery truck", the company most likely did not use the best ship mode with respect to the order priority.


-----Now to the question, if delivery truck is most economical(i.e lesser money was spent using delivery truck), it'll mean the company prioritized low shipping cost above fast delivery even in critically high order priority, thereby jeopardizing the companies integrity. So the answer is No, because in that case the company did not appriopriately spend shipping cost with respect to the order priority. 

  
      	








