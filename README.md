Power BI Dashboard Development with Snowflake Data 
Warehouse 
Objective: 
This document outlines the step-by-step process to create an interactive Power BI dashboard using data from a Snowflake data warehouse. The dashboard will showcase key performance indicators (KPIs) and visual representations of restaurant data. 
Step 1: Connecting Power BI to Snowflake Data Warehouse 
1.	Open Power BI Desktop 
2.	Navigate to Data Connection:  
a.	Go to Home → Get Data → More… → Snowflake. 
  
3.	Enter Server and Warehouse Details:  
a.	Server: You can find server details by going to ACCOUNTADMIN (bottom left) → Account → View More Details → Account/Server URL. 
b.	Warehouse: Enter the appropriate Snowflake warehouse name. 
4.	Connect and Load Data:  
a.	Choose the desired table(s) from the Snowflake data warehouse and click Load. Step 2: Identifying KPIs and Visual Concepts 
Key Performance Indicators (KPIs): 
•	Total Number of Restaurants: Total count of restaurants in the dataset. 
•	Active Restaurants: Restaurants without an "Eff End Date" or those still within the active period. 
•	Closed/Inactive Restaurants: Restaurants with an "Eff End Date" in the past. 
•	Percentage of Restaurants in Capital Cities: Proportion of restaurants located in capital cities. 
•	State with the Most Restaurants: State with the highest number of restaurants. Visual Concepts: 
•	Card Visuals: 
o	Total Restaurants o Active Restaurants o Inactive Restaurants 
o	Percentage of Restaurants in Capital Cities o State with the Most Restaurants 
•	Map Visual: o Geographical spread of restaurants by city and state 
•	Bar Chart: 
o	Distribution of restaurants across Tier 1, Tier 2, and Tier 3 cities 
•	Pie/Donut Chart: 
o Distribution of restaurants by state Step 3: Creating DAX Measures for KPIs 
To create a new measure in Power BI: 
• Go to Modeling → New Measure → Enter the respective DAX code 
  
Measure 1: Total Number of Restaurants 
TotalRestaurants = CALCULATE(COUNTROWS('Your_Table_Name'), 
REMOVEFILTERS('Your_Table_Name')) 
Measure 2: Active Restaurants 
Active_Restaurants = CALCULATE(COUNTROWS('Your_Table_Name'), 
'Your_Table_Name'[ACTIVE_FLAG] = "YES", REMOVEFILTERS('Your_Table_Name')) 
Measure 3: Inactive Restaurants 
Inactive_Restaurants = CALCULATE(COUNTROWS('Your_Table_Name'), 
'Your_Table_Name'[ACTIVE_FLAG] = "NO", REMOVEFILTERS('Your_Table_Name')) 
Measure 4: Percentage of Restaurants in Capital Cities 
%_Restaurants_Capital_Cities =  
DIVIDE( 
    CALCULATE(COUNTROWS('Your_Table_Name'), 
'Your_Table_Name'[CAPITAL_CITY_FLAG] = TRUE),     COUNTROWS('Your_Table_Name'), 
    0 
) * 100 
Measure 5: State with the Most Restaurants 
TopState =  
CALCULATE( 
    FIRSTNONBLANK( 
        SELECTCOLUMNS( 
            TOPN( 
                1, 
                SUMMARIZE( 
                    'Your_Table_Name', 
                    'Your_Table_Name'[STATE], 
                    "RestaurantCount", 
CALCULATE(COUNTROWS('Your_Table_Name'), 'Your_Table_Name'[ACTIVE_FLAG] = "YES") 
                ), 
                [RestaurantCount], DESC 
            ), 
            "State", 'Your_Table_Name'[STATE] 
        ), 
        1 
    ), 
    REMOVEFILTERS('Your_Table_Name') 
) 
  
Note: Replace 'Your_Table_Name' and column names with the actual table and column names from your dataset. 
Step 4: Creating Visualizations 
1. Card Visuals: 
a.	Total Restaurants: Use the TotalRestaurants measure 
b.	Active Restaurants: Use the Active_Restaurants measure 
c.	Inactive Restaurants: Use the Inactive_Restaurants measure 
d.	% of Restaurants in Capital Cities: Use the %_Restaurants_Capital_Cities measure 
e.	Top State: Use the TopState measure 2. Map Visual: 
a.	Location Field: CITY column 
b.	Value Field: Count of ACTIVE_FLAG 3. Donut Chart: 
a.	Legend: STATE column 
b.	Values: Count of ACTIVE_FLAG 4. Stacked Column Chart: 
a. X-Axis: CITY_TIER column 
b. Y-Axis: Count of ACTIVE_FLAG Step 5: Creating Filters (Slicers) 
1.	State Filter: 
a.	Create a slicer with the STATE column. 
2.	City Filter: 
a.	Create a slicer with the CITY column. 
3.	Managing Filter Interactions: 
a.	Select the State Filter → Go to Format → Edit Interactions → Set visual interactions to None for visuals where this filter should not apply (e.g., Total Restaurants, Active Restaurants). 
b.	Repeat the same process for the City Filter. 
Step 6: Dashboard Styling and Professional Look 
•	Choose a consistent color theme that aligns with your organization’s branding or enhances readability. 
•	Format fonts, titles, and data labels for clarity and visual appeal. 
•	Add a dashboard title and descriptive headers for each visual. 
•	Ensure visuals are aligned and evenly spaced for a clean layout. 
  
