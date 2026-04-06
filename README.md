Zomato Sales & User Analytics Dashboard
1.Project Overview
This project provides a comprehensive analysis of Zomato's operations, including sales, orders, customer behavior, and city-wise performance. The insights are derived from transaction and user data, helping identify growth opportunities, customer retention challenges, and high-performing regions.
2. Key Metrics
	Overall Performance:
•	Total Sales Amount: ₹987M 
•	Total Orders: 150.28K 
•	Overall Rating: 148.45K 
•	Total Users: 100K 
•	Active Users: 77.93K
	Category-wise Performance:
Category	Sales	Ratings
Veg	156K	731K
Non-Veg	140K	316K
Others	14K	1K
	Yearly Performance:
•	Highest sales year: 2019 – ₹1002K 
•	Top-performing city (by sales): Electronic City, Bangalore – ₹0.36M 
	User Insights
•	Age Group with Highest Users: 23 years old (18.8K users) 
•	Gain Customers (Current Year vs Previous Year): 11.64K 
o	Male: 6.56K 
o	Female: 5.08K 
•	Lost Customers (Current Year vs Previous Year): 33.04K 
o	Male: 18.92K 
o	Female: 14.12K 
	City-Level Insights
•	City Sales Amount: ₹368K 
•	Sales Quantity: 367 
•	Orders: 0.10K 
•	Overall Rating: 148.45K 
Example: Akola
•	Sales: ₹0.37K 
•	Ratings: 0.10K 
•	Users: 0.10K 
•	Orders: 100 
•	Lost Orders: 100 
3.📷 Screenshots
 
 
 
 
4. Data Model & Features
🔗 Data Sources
o	orders table – Transactional order data 
o	users table – Customer demographic and behavior data 
o	restaurant table – Restaurant ratings 
o	Food table-Category
o	Menu table-Relatationship between Menu and Food
⚙️ Features Implemented
•	✔️ Data Modeling (Relationships) 
•	✔️ DAX Measures & Calculations 
•	✔️ Dynamic KPIs 
•	✔️ Bookmark Navigation (City) 
5.DAX Measures
Sale_Value=SUM(orders[Value])

Year = VALUE( FORMAT(orders[order_date],"yyyy"))

Rating_count=COUNT(restaurant[rating])

Order_count=COUNT(orders[order_date])

Rank Table = 
DATATABLE(
    "Sort", INTEGER,
    "Type", STRING,
    "NO", INTEGER,
    {
        {0, "All sale", 0},
        {1, "Top 5", 5},
        {2, "Top 10", 10},
        {3, "Top 20", 20},
        {4, "Top 50", 50},
        {5, "Top 100", 100}
    }
)

Dynamic_TopN_Title = 
VAR selectRank =
    SELECTEDVALUE('Rank Table'[Type], "All sale")
RETURN
selectRank & " City by Sales"

Dynamic_Subheading = "Zomato Providing Services in"&COUNT(orders[City])&"and connected with"&DISTINCTCOUNT(users[user_id])&"where got"& COUNT(orders[user_id])& "orders."

TopN_sale = 
VAR SelectedRank = SELECTEDVALUE('Rank Table'[NO], 0)
VAR RankValue =
    RANKX(
        ALLSELECTED(orders[City]),
        [Sale_Value],
        ,
        DESC,
        DENSE
    )
RETURN
IF(
    SelectedRank = 0,
    [Sale_Value],
    IF(RankValue <= SelectedRank, [Sale_Value], BLANK())
)

User_Count = DISTINCTCOUNT(users[user_id])

Active_Users = DISTINCTCOUNT(orders[user_id])

Curr_year = 2020

Prev_Year = [Curr_year]-1

Curr_Yr_Sale = VAR YR=[Curr_year] RETURN CALCULATE([Sale_Value],orders[Year]=YR)

PREV_Yr_Sale = VAR YR=[Prev_Year] RETURN CALCULATE([Sale_Value],orders[Year]=YR)

Gain_Customers = VAR Filters_users=FILTER(SUMMARIZE(users,users[user_id]),AND([PREV_Yr_Sale]<=0,[Curr_Yr_Sale]>0)) RETURN CALCULATE([User_Count],Filters_users)

Lost_Customers = VAR Filters_users=FILTER(SUMMARIZE(users,users[user_id]),AND([Curr_Yr_Sale]<=0,[PREV_Yr_Sale]>0)) RETURN CALCULATE([User_Count],Filters_users)

Rating Count by City = COUNT(orders[City])

Gain_Cust =
VAR UserTable =
    SUMMARIZE(
        users,
        users[user_id],
        "PrevSale", [PREV_Yr_Sale],
        "CurrSale", [Curr_Yr_Sale]
    )
RETURN
    COUNTROWS(
        FILTER(
            UserTable,
            [PrevSale] <= 0 && [CurrSale] > 0
        )
    )

Lost_Cust = 
VAR UserTable =
    SUMMARIZE(
        users,
        users[user_id],
        "PrevSale", [PREV_Yr_Sale],
        "CurrSale", [Curr_Yr_Sale]
    )
RETURN
    COUNTROWS(
        FILTER(
            UserTable,
            [CurrSale] <= 0 && [PrevSale] > 0
        )
    )
6.Business Insights & Recommendations
1.	Customer Retention Focus: 
o	High number of lost customers (33.04K) indicates a potential churn issue. 
o	Action: Implement loyalty programs, personalized promotions, and targeted engagement campaigns to retain users. 
2.	City-Level Optimization: 
o	Some cities like Akola show low sales and high lost orders, suggesting operational inefficiencies or low awareness. 
o	Action: Strengthen local marketing efforts, optimize restaurant partnerships, and analyze user preferences in these regions. 
3.	Category Performance Optimization: 
o	Veg category shows higher ratings than Non-Veg, suggesting user preference trends. 
o	Action: Increase promotions and offerings for Veg dishes in high-demand areas. 
4.	High-Potential Demographics: 
o	Young users (around 23 years old) form the largest segment of active users. 
o	Action: Develop targeted campaigns, mobile-first features, and referral programs for this demographic. 
5.	Top Cities Strategy: 
o	Focus on top-performing cities like Bangalore to maximize revenue growth. 
o	Consider expanding delivery network and restaurant partnerships in these regions. 
7.Conclusion 
The Zomato Sales & User Analytics Dashboard provides a comprehensive view of business performance across cities, categories, and customer segments. Key takeaways include:
•	Sales & Orders: The platform generated ₹987M in sales with 150.28K orders, indicating strong overall performance. 
•	Customer Behavior: While 11.64K new customers were gained, 33.04K users were lost, highlighting a significant churn issue. 
•	City & Category Insights: Certain cities and categories (like Veg dishes) are outperforming others, showing areas for strategic focus. 
•	Recommendations: 
o	Implement retention programs for churned customers. 
o	Optimize city-level operations in underperforming regions. 
o	Enhance promotions for high-potential categories and demographics. 
Overall, the dashboard enables data-driven decision-making, highlighting opportunities for growth, operational efficiency, and improved customer engagement.

Deepak Kumar Tekchandani
 Power BI|DAX (Data Analysis Expressions)|Data Modeling|Data Cleaning & Transformation|Interactive Data Visualization

📧 deepakkumartekchandani@gmail.com  
🔗 LinkedIn: www.linkedin.com/in/deepak-kumar-tekchandani-b7200a146
💻 GitHub: https://github.com/DTekchandani1/Zomato-Sales-User-Analytics-Dashboard.git






