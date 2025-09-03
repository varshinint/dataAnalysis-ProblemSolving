# Case Study: Root Cause Analysis of a Sudden Spike in Feature Usage

# Business Problem

Over the last 5 days, the usage of the “Schedule Send” feature spiked by 200% compared to the previous two weeks. The spike could be due to a backend change, promotion, or real behavioral shift. The PM asked:
"Can you investigate what’s causing this sudden increase?"

# Objective:
Identify possible causes of the spike and quantify their impact using SQL and data analysis.

# Understanding the Feature

Purpose of “Schedule Send”:

Supports work-life balance by allowing users to compose emails at night but send during work hours.

Accounts for time zones so teams in different geographies can send emails at optimal times.

Enables strategic timing for marketing or outreach to improve email open rates.

# Initial Hypothesis Framing:
Potential drivers of the spike:

Promotional banners or campaigns highlighting the feature.

Positive customer reviews increasing adoption.

Bug fixes or latency improvements making the feature usable for previously blocked users.

Social media mentions, blogs, or word-of-mouth.

# Step 1: Clarifying Questions & Initial Checks

Before jumping into SQL, these are the questions and metrics I considered:

Any recent promotional events for this feature?

Did a new version or update make the feature easier to access?

Are there any changes in review ratings?

What is the CTR (click-through rate) for the feature this week vs previous week?

Number of users who adopted the feature vs total eligible users.

Metrics from promotion tables: adoption by promotion type, date, etc.

Check for external factors (e.g., social media mentions, blog posts).

This step ensures the analysis is aligned with business goals and user behavior.

# Step 2: SQL Analysis

# 1. Promotion Users Count by Type

SELECT promotion_type,
       COUNT(DISTINCT user_id) AS promotion_counts
FROM promotion_table
WHERE feature_id = 16
  AND date BETWEEN 'start_date' AND 'end_date'
GROUP BY promotion_type
ORDER BY promotion_counts DESC;


# 2. Top Feature Usage Time per User

SELECT user_id,
       DATE,
       EXTRACT(EPOCH FROM (end_time - start_time)) / 60 AS feature_usage_time_mins,
       ROW_NUMBER() OVER (PARTITION BY user_id, feature_id ORDER BY (end_time - start_time) DESC) AS rank
FROM feature_usage
WHERE feature_id = 16
  AND date BETWEEN 'start_date' AND 'end_date';


# 3. Adoption Rate per Week

SELECT DATE_TRUNC('week', f.date) AS week,
       (COUNT(DISTINCT f.user_id) * 100.0 / COUNT(DISTINCT e.user_id)) AS adoption_rate
FROM eligible_users e
LEFT JOIN (
    SELECT user_id,
           DATE,
           ROW_NUMBER() OVER (PARTITION BY user_id, feature_id ORDER BY (end_time - start_time) DESC) AS rank
    FROM feature_usage
    WHERE feature_id = 16
) f ON e.user_id = f.user_id AND f.rank = 1
WHERE f.date BETWEEN 'start_date' AND 'end_date'
GROUP BY week;


# 4. High Ratings by Week

SELECT DATE_TRUNC('week', date) AS week,
       COUNT(user_id) AS high_rating_count
FROM customer_reviews
WHERE feature_id = 16
  AND rating >= 4
  AND date BETWEEN 'start_date' AND 'end_date'
GROUP BY week;

# Thought Process & Analytical Approach

## Hypothesis Framing:

I considered promotions, feature updates, user sentiment, external events, and technical changes.

## Metric Definition:

CTR: Number of users who interacted with feature ÷ Total eligible users.

Adoption Rate: % of eligible users who used the feature.

Review Ratings: % of high ratings (≥4) to gauge sentiment.

## Sequential Analysis:

Promotion → Usage → Adoption → Reviews


# Insights

The spike can be attributed to:

Higher adoption rate after promotion — campaigns positively influenced usage.

Positive reviews and sentiment — increased trust and trial of the feature.

Real behavioral shift — users actively scheduling emails, not bots.

Push notifications / email campaigns — drove traffic to the feature.

Word-of-mouth / social media mentions — organically increased awareness.


# Recommendations

Monitor feature adoption trends weekly to detect anomalies early.
