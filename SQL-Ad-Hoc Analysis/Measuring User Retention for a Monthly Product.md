# Business Problem

Our product is designed for monthly usage (e.g., a budgeting app where users log in once a month). The company wants to understand user retention trends: how many users continue to return month after month.

The challenge: traditional daily/weekly retention metrics are not meaningful here, since users are not expected to be active every day or week.

## Key Metric: Monthly Retention Rate 

Definition:
Percentage of users who were active in the previous month and also active in the current month.

Formula:

MRR = # of users active in both the previous month and the current month / # of users active in the previous month}} \times 100

Why Monthly?
Since the product is meant for monthly use, a user who consistently returns every month is considered “retained.” Daily or weekly retention would incorrectly classify active users as “churned.”

## Step-by-Step SQL Approach
1. Identify monthly active users

Extract month and year from activity_date, group by user and month.

2. Find retained users

Use a self-join on the user_activity table:

ua1 → users active in the previous month

ua2 → users active in the next month

Join on user_id to find common users.

3. Calculate retention rate

Count retained users

Divide by total active users in the previous month

## SQL Implementation
WITH previous AS (
    SELECT DATE_TRUNC('month', activity_date) AS previous_month,
           COUNT(DISTINCT user_id) AS previous_month_users
    FROM user_activity
    GROUP BY DATE_TRUNC('month', activity_date)
),


retained AS (
    SELECT DATE_TRUNC('month', ua1.activity_date) AS previous_month,
           DATE_TRUNC('month', ua2.activity_date) AS current_month,
           COUNT(DISTINCT ua1.user_id) AS retained_users
    FROM user_activity ua1
    JOIN user_activity ua2
      ON ua1.user_id = ua2.user_id
     AND DATE_TRUNC('month', ua2.activity_date) = DATEADD(month, 1, DATE_TRUNC('month', ua1.activity_date))
    GROUP BY DATE_TRUNC('month', ua1.activity_date),
             DATE_TRUNC('month', ua2.activity_date)
)


SELECT r.previous_month,
       r.current_month,
       p.previous_month_users,
       r.retained_users,
       ROUND(100.0 * r.retained_users / p.previous_month_users, 2) AS monthly_retention_rate
FROM retained r
JOIN previous p
  ON r.previous_month = p.previous_month
ORDER BY r.previous_month;
