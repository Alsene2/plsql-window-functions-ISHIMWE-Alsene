
Business Context
top Spares Ltd is a company in Kigali that sells spare parts for cars and generators across multiple Rwandan companys. Management wants to analyze which parts generate the most revenue, track monthly sales performance, and understand customer buying behavior.

Data Challenge
The company struggles to identify their top-selling spare parts per region and quarter and to measure how sales changes and how they behave monthly. They also need insights on our most trusted customers , in order to gift them some unique offers.
Expected Outcome:
Identify the spare parts highly sold in the region region/quarter.
Track monthly price changes on the market.
Identifying our most customers and segment them
Use moving averages to forecast seasonal demand for spare parts.

Step 2: Success Criteria 
1)Top 5 spare parts per region/quarter → RANK()
2)Running monthly sales totals → SUM() OVER()
3)Month-over-month growth → LAG()/LEAD()
4)Customer quartiles → NTILE(4)
5)3-month moving averages → AVG() OVER()
Step3:Database schema
 customers: stores customer information, including a unique customer ID, name, and region.
 products: stores product information, including a unique product ID, product name, and category.
 transactions: records  transactions, linking customers and products through foreign keys, and transactions details such as date and amounts.
Relationships
customer–transaction Relationship: A customer can make multiple transactions, but each transaction  is associated with only one customer.

product–transaction Relationship: A product can appear in multiple transactions, but each transaction involves only one product.

Step4: Window Functions Implementation
1.Ranking
Explanation:
ROW_NUMBER(): Assigns a unique sequential number to each customer based on total revenue (highest first). No ties are allowed; even if two customers have the same revenue, their row numbers differ.
RANK(): Assigns ranks with gaps if there are ties. Customers with the same revenue get the same rank, and the next rank is skipped.
DENSE_RANK(): Like RANK but no gaps in ranking. Ties share the same rank, next rank continues sequentially.
PERCENT_RANK(): Shows a customer’s relative position between 0 (lowest) and 1 (highest) in the dataset. 
2.Aggregate
Explanation:
SUM(amount): Calculates cumulative revenue for each customer over time (running total).
AVG(amount): Computes the average transaction amount so far.
MIN(amount) / MAX(amount): Tracks smallest and largest transactions in the sequence.
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW: Means the window starts from the first transaction and goes up to the current row.
3. Navigation
Explanation:
LAG(amount,1): Gets the previous transaction amount per customer.
LEAD(amount,1): Gets the next transaction amount per customer.
Growth %: Compares current transaction to previous to calculate period-to-period growth
4. Distribution
Explanation:
NTILE(4): Divides customers into 4 quartiles based on revenue (top 25%, second 25%, etc.).
CUME_DIST(): Shows proportion of customers below or equal to current revenue. Value between 0–1
Step 6: Results Analysis
Descriptive – What happened?
Top-performing customers: generated the highest total revenue (e.g., Alexis Ishimwe had 550,000 across multiple transactions).
Revenue trends:identify some customers made frequent smaller purchases while others made fewer high-value purchases.
Outliers: Certain customers like John Doe had lower total revenue, indicating infrequent purchases.
Quartile segmentation shows that revenue is unevenly distributed: top 25% of customers contribute a large portion of total sales.

Diagnostic – Why did it happen?
High revenue is driven by multiple transactions or high-value products purchased.
Low revenue customers either purchase less frequently or choose lower-priced products.
Revenue accumulation patterns from running totals reveal seasonality or periods of higher sales.
Ranking and segmentation indicate customer behavior differences, showing that a small number of customers significantly impact total revenue.

Prescriptive – What next?
Target top-quartile customers with loyalty programs, premium offers, or early access to promotions to retain high-value clients.
Encourage low-quartile customers with discounts or marketing campaigns to increase engagement and spending.
Analyze transaction patterns over time to identify peak purchasing periods and optimize inventory or promotions accordingly.
Use customer segmentation insights to personalize communications and improve overall sales performance.

REFERENCES
Ivan, B.(2025).database management system - lecture 04: functions in sql.AUCA.

Maniraguha, E. (2025). Database Development with PL/SQL - Lecture 02: Introduction to GitHubs. AUCA.
SQL tutorial(2021).practice complex SQL Queries-complete guide   (video).https://www.youtube.com/watch?v=FNYdBLwZ6cE
Oracle Corporation. (2025). MySQL 8.0 Reference Manual: Window Functions.
https://dev.mysql.com/doc/refman/8.0/en/window-functions.html
MySQLTutorial.org. (2024). MySQL Window Functions.
https://www.mysqltutorial.org/mysql-window-functions/
W3Schools. (2024). SQL Window Functions.
https://www.w3schools.com/sql/sql_window_functions.asp
TutorialsPoint. (2024). SQL Window Functions.
https://www.tutorialspoint.com/sql/sql-window-functions.htm
GeeksforGeeks. (2023). Introduction to SQL Window Functions.
https://www.geeksforgeeks.org/sql-window-functions/
Oracle Corporation. (2025). MySQL 8.0 Reference Manual: Window Functions.
https://dev.mysql.com/doc/refman/8.0/en/window-functions.html
Mode Analytics. (2023). SQL Tutorial: Window Functions.
https://mode.com/sql-tutorial/sql-window-functions/
