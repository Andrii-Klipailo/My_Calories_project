# My_Calories_project
![logo](https://github.com/Andrii-Klipailo/My_Calories_project/blob/main/logo.jpg)
## Overview
I would like to present my project focused on calorie consumption analytics and weight management (either gaining or losing weight). I became interested in tracking the calories I consume, so I started recording the calories I consume with each meal and weighing myself weekly to observe how my weight fluctuates. With this data, I decided to create an interactive dashboard that helps me monitor and analyze my eating habits, calculate the necessary daily calorie intake to achieve my goals, and potentially uncover insights I hadn’t noticed before.

I record all my data in an Excel file, which is then uploaded to Power BI for analysis.

## Project Goals
The main goals of this project are to analyze calorie consumption over specific time intervals, calculate the contribution of each meal to the total daily intake, and compare consumption by day and by parts of the week (e.g., weekends vs weekdays).

The primary objective is to calculate the average daily calorie intake for each period and the total number of calories I need to consume each day to maintain my basic functions and a stable weight (TDEE). Knowing this value allows me to manage weight gain or loss in alignment with my goals and time constraints.
#### Main Page
![logo](https://github.com/Andrii-Klipailo/My_Calories_project/blob/main/screenshot1.jpg)
Here you can see the main dashboard page with all the information I wanted to analyze in this dataset.

#### DAX code
To calculate the key metrics, I used the DAX language, and here is an example of how the average calorie consumption was calculated relative to the slicer by periods. In this case, I had to use formulas to calculate calendar periods so that the result matched the slicer.
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

## Findings and Conclusion

Using Power BI's visualization capabilities and DAX for calculating key metrics, I was able to conduct an extensive analysis of my eating habits. With the ability to accumulate and update data, I will be able to continue using this dashboard for further analysis and control of my diet to achieve my goals (in this case, weight gain).

##### This project is designed to allow the input data to be replaced, making it adaptable for other users to use for their own goals.

##### I invite you to download and use this project.

#### Thank you for your time and attention!
