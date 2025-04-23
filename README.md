# My Calories Tracker Project  
**Self-tracking dashboard for calorie intake and weight management**

![logo](https://github.com/Andrii-Klipailo/My_Calories_project/blob/main/logo.jpg)

---

## Overview  

This is a personal data analytics project focused on **calorie consumption tracking** and **weight management**, whether the goal is to gain or lose weight. I began by logging the calories I consumed during each meal and weighing myself weekly to monitor how my body responded over time.  

To analyze this data more effectively, I created an **interactive Power BI dashboard** that enables:  
- Monitoring calorie consumption trends  
- Analyzing eating habits  
- Estimating calorie needs  
- Aligning intake with fitness goals  

All data is stored in **Excel** and imported into Power BI for visualization and analysis.



## Project Goals  

- Track calorie consumption across different **time intervals**  
- Break down intake by **meal type** (e.g., breakfast, lunch, dinner)  
- Compare daily consumption on **weekdays vs weekends**  
- Calculate **TDEE (Total Daily Energy Expenditure)** — the estimated daily calorie needs to maintain body weight  
- Enable goal-driven planning for **weight loss or gain**  


## Dashboard Preview  

![Dashboard](https://github.com/Andrii-Klipailo/My_Calories_project/blob/main/screenshot1.jpg)

The dashboard includes:  
- A visual comparison between **actual daily intake** and **TDEE**  
- Meal contribution percentages  
- Time period filters (This Week, Previous Month, etc.)  
- Weight progression overview  
- Daily/weekly/monthly averages



## DAX Code Example  

To enable dynamic filtering by time period, I used **DAX** (Data Analysis Expressions). Here's a simplified version of the DAX formula that calculates **average calorie consumption** based on a selected period:

```DAX
AVG by period = 
VAR AVG_period = 
SWITCH(TRUE(),
    SELECTEDVALUE('Time Periods'[Period]) = "This Week",
        CALCULATE( 
            AVERAGE(Calories[daily_consumption]), 
            'Calendar'[Date] <= MAX('Calendar'[Date]) && 'Calendar'[Date] >= MAX('Calendar'[Date]) - WEEKDAY(MAX('Calendar'[Date]), 2) + 1
        ),
    SELECTEDVALUE('Time Periods'[Period]) = "Previous Week",
        CALCULATE(
            AVERAGE('Calories'[daily_consumption]), 
            'Calendar'[Date] <= MAX('Calendar'[Date]) - WEEKDAY(MAX('Calendar'[Date]), 2) && 
            'Calendar'[Date] >= MAX('Calendar'[Date]) - WEEKDAY(MAX('Calendar'[Date]), 2) - 7
        ),
    SELECTEDVALUE('Time Periods'[Period]) = "This Month",
        CALCULATE(
            AVERAGE('Calories'[daily_consumption]),
            FILTER(
                'Calendar',
                'Calendar'[Date] >= DATE(YEAR(MAX('Calendar'[Date])), MONTH(MAX('Calendar'[Date])), 1) && 
                'Calendar'[Date] <= MAX('Calendar'[Date])
            )
        ),
    SELECTEDVALUE('Time Periods'[Period]) = "Previous Month",
        CALCULATE(
            AVERAGE('Calories'[daily_consumption]),
            FILTER(
                'Calendar',
                MONTH('Calendar'[Date]) = MONTH(MAX('Calendar'[Date]))-1 && 
                YEAR('Calendar'[Date]) = YEAR(MAX('Calendar'[Date]))
            )
        ),
    SELECTEDVALUE('Time Periods'[Period]) = "Last 3 Month",
        CALCULATE(
            AVERAGE('Calories'[daily_consumption]),
            FILTER(
                'Calendar',
                'Calendar'[Date] >= DATE(YEAR(MAX('Calendar'[Date])), MONTH(MAX('Calendar'[Date])) - 2, 1) && 
                'Calendar'[Date] <= MAX('Calendar'[Date])
            )
        ),
    SELECTEDVALUE('Time Periods'[Period]) = "Last Year",
        CALCULATE(
            AVERAGE('Calories'[daily_consumption]),
            FILTER(
                'Calendar',
                'Calendar'[Date] >= DATE(YEAR(MAX('Calendar'[Date])) - 1, MONTH(MAX('Calendar'[Date])), 1) && 
                'Calendar'[Date] <= MAX('Calendar'[Date])
            )
        ),
    AVERAGE('Calories'[daily_consumption])
)
RETURN COALESCE(AVG_period, 0)
```

## Key Insights & Findings

- **Dinner** contributes the highest percentage of daily calories  
- **Weekends** often result in higher-than-average intake  
- Visualizing **TDEE (Total Daily Energy Expenditure)** vs actual consumption highlights under- or over-eating days  
- **Weight gain patterns** align well with consistent calorie surpluses over time

---

## Flexibility

This project is fully **reusable and customizable**. You can simply:

- Replace the **Excel source file** with your own (using the same structure)  
- **Power BI** will update all visualizations automatically — *no code changes needed!*

---

## Conclusion

With **Power BI and DAX**, I turned my simple logs into a powerful **self-awareness and tracking tool**. As I continue logging, this dashboard will evolve and provide new insights.

---

#### Thank you for taking the time to explore my project!
If you have any feedback, questions, or would like to connect — feel free to reach out.

