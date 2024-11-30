# P1 -- Problem Solving

# Customer Churn Problem

## Question: 

How can we predict which customers are likely to churn, and what actions
should we take to retain them?

## Solution:

### Step 1: Understand the Problem

Churn occurs when customers stop using a product or service. Predicting
churn allows the company to intervene and retain these customers, saving
revenue and improving long-term relationships.

Set meeting with associated team Define churn clearly:

> **Step 1: Understand the Problem (Expanded)**

1.  **Initiate Discussions with Stakeholders**:

    -   Meet with the **customer support team** fully grasp the scope of
        the problem.

    -   Key questions to ask:

        -   What is the desired outcome? For example:

            -   Are they looking for a ranked list of high-risk
                customers?

            -   Do they need actionable insights to design retention
                strategies?

        -   What intervention options are available once churn risks are
            identified?

        -   Are there any specific business constraints (e.g., budget
            limits for retention offers)?

2.  **Align Expectations**:

    -   Ensure all stakeholders are aligned on:

        -   The deliverables: Is it a report, dashboard, or predictive
            model?

        -   Timelines: When is the result needed?

        -   Success metrics: How will the solution be evaluated? For
            example:

            -   Reduction in churn rate by X%.

            -   Increase in retention campaign ROI.

3.  **Collaboratively Define Churn**:

-   Is churn when a subscription has not been renewed,

-   Is churn when a subscription is canceled, or

-   When a customer hasn't interacted with the product for 30 days (or
    any other certain period of time).

4.  **Understand the Available Data**:

    -   Set meeting with **Data Engineers, Database Administrators, or
        customer support team** and discuss what data is available with
        the teams.

    -   Ask:

        -   Is customer engagement data being tracked?

        -   Are there historical records of churned customers?

        -   Where can I find the data related to customer activity,
            transactions, and support interactions?

        -   What format is the data in (e.g., SQL tables, CSV files)?

        -   Are there any data pipelines already set up for this type of
            analysis?

### Step 2: Collect and Explore Data

-   **Data Sources**:

    -   Customer demographics: age, gender, location, etc.

    -   Subscription:

        -   Subscription Status: Indicates if the plan
            is *Active*, *Expired*, or *Canceled*.

        -   Subscription Start and End Date

        -   Renewal Status: Indicates if the subscription is set to
            auto-renew.

        -   Subscription Type: The type of plan purchased (e.g.,
            Monthly, Annual, Premium, etc.).

    -   Usage data: frequency of usage, time spent, features used.

    -   Transactional data: purchase frequency, average spend, payment
        delays.

    -   Support interaction data: frequency of complaints, resolution
        times, satisfaction ratings.

    -   Feedback or reviews: sentiment analysis on feedback.

-   **Exploratory Analysis**:

    -   Identify patterns among churned customers (e.g., low usage
        frequency, high complaints).

    -   Visualize churn trends (e.g., churn rates by demographics or
        behavior).

### Step 3: Feature Engineering

> Create meaningful features that can help predict churn:

**1. Recency**

-   **Definition:** Time since the customer last interacted with the CRM
    platform.

-   **Example:**

    -   Client A logged in yesterday → **Recency = 1 day**.

    -   Client B hasn't logged in for 30 days → **Recency = 30 days**.\
        **Why it matters:** If customers are not logging into the CRM,
        they may not find it useful or may be considering alternatives.

**2. Frequency**

-   **Definition:** How often the customer interacts with the CRM
    platform over a specific period.

-   **Example:**

    -   Client A logged in 25 times in 30 days → **Frequency = 25
        logins**.

    -   Client B logged in only 5 times → **Frequency = 5 logins**.\
        **Why it matters:** Frequent logins suggest active engagement,
        whereas low frequency may indicate dissatisfaction or
        underutilization.

**3. Monetary Value**

-   **Definition:** Total revenue generated by the customer within a
    specific time frame.

-   **Example:**

    -   Client A subscribed to the basic plan (\$50/month) and purchased
        a \$500 integration service → **Monetary Value = \$1,100/year**.

    -   Client B subscribed to a premium plan (\$200/month) → **Monetary
        Value = \$2,400/year**.\
        **Why it matters:** High-paying customers represent greater
        revenue and are worth prioritizing for retention.

**4. Engagement Score**

-   **Definition:** A composite score measuring the depth of the
    customer's interaction with the CRM features.

-   **Example:**

    -   Logins: 10 points per weekly login → 40 points.

    -   Features used: Email campaign module = 20 points, analytics
        dashboard = 30 points.

    -   Training materials accessed: 15 points.

    -   Total Engagement Score = **105 points**.

    -   Client A has a score of 105, while Client B, who only logs in
        occasionally, has a score of 30.\
        **Why it matters:** Low engagement indicates that customers may
        not fully realize the value of the CRM and are at higher risk of
        churn.

**5. Sentiment Score (Requires NLP if scoring is not available)**

-   **Definition:** Numerical score derived from analyzing customer
    support tickets, feedback, or surveys.

-   **Example:**

    -   Client A: \"The analytics module is intuitive and saves us
        time!\" → **Sentiment Score = +2**.

    -   Client B: \"Support takes too long to respond; we're considering
        another solution.\" → **Sentiment Score = -3**.\
        **Why it matters:** Negative feedback indicates dissatisfaction,
        making it a leading indicator of churn.

**6. Tenure**

-   **Definition:** The length of time the customer has been using the
    CRM platform.

-   **Example:**

    -   Client A signed up 6 months ago → **Tenure = 6 months**.

    -   Client B has been a customer for 3 years → **Tenure = 36
        months**.\
        **Why it matters:** Long-tenured customers may churn due to
        dissatisfaction or competing offers, whereas newer customers may
        churn if onboarding is not smooth or if they fail to see value
        early.

### Step 4: Define the Criteria for Each Score

Break down customer activity into measurable categories, each
contributing to the churn score:

| Score | Definition                                                                 |
| ----- | -------------------------------------------------------------------------- |
| 1     | Highly likely to churn: No recent activity, very low engagement, or negative feedback. |
| 2     | At risk of churn: Some activity but declining trends in engagement or monetary value. |
| 3     | Neutral: Moderate activity, no significant negative trends, or stable patterns. |
| 4     | Likely active: Good engagement, consistent monetary value, or positive sentiment. |
| 5     | Highly active: High frequency, recency, strong monetary value, and high engagement. |

Example Table with 1, 2, 3, 4, 5 scale:

| Feature          | 1 (Very Low)                               | 2 (Low)                                   | 3 (Moderate)                             | 4 (High)                                  | 5 (Very High)                            |
| ---------------- | ----------------------------------------- | ---------------------------------------- | --------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| **Recency**      | > 90 days since last login               | 60–90 days since last login              | 30–60 days since last login             | 15–30 days since last login              | < 15 days since last login               |
| **Frequency**    | < 1 login in 30 days                     | 1–2 logins in 30 days                    | 3–5 logins in 30 days                   | 6–10 logins in 30 days                   | > 10 logins in 30 days                   |
| **Monetary Value** | < $100/year                             | $100–$500/year                           | $500–$1000/year                         | $1000–$1500/year                         | > $1500/year                             |
| **Engagement Score** | < 10                                  | 10–25                                    | 26–50                                   | 51–75                                    | > 75                                     |
| **Sentiment Score** | Strongly Negative (-2 or lower)        | Negative sentiment (-1)                  | Neutral sentiment (0)                   | Positive sentiment (+1)                  | Strongly Positive sentiment (+2 or higher) |
| **Tenure**       | < 3 months                               | 3–6 months                               | 6–12 months                             | 1–2 years                                | > 2 years                                |


Discuss the thresholds with customer service team and look in data to
find the best threshold value for each feature. For example, the highest
monetary value in database might be much lower or higher.

2.  Step 5: Define Churn Score based on feature score

Asda

Use a weighted formula to combine the feature scores. Assign higher
weights to the most critical features (e.g., **Recency**, **Frequency**,
and **Engagement Score**) based on their importance to churn prediction.

-   **Example Formula:**

Churn Score= 0.3×Recency Score+ 0.2×Frequency Score+
0.2×Engagement Score+

0.15×Monetary Value Score+ 0.1×Sentiment Score+ 0.05×Tenure

The churn score is calculated as a weighted sum of the feature scores,
normalized to fall between **1 and 5**.

-   Adjust the weights based on the company\'s specific priorities or
    observed correlations.

Example

| Feature            | Value         | Score (1–5) |
| ------------------ | ------------- | ----------- |
| **Recency**        | 15 days       | 5           |
| **Frequency**      | 8 logins      | 4           |
| **Monetary Value** | $800/year     | 3           |
| **Engagement Score** | 75          | 3           |
| **Sentiment Score** | Positive     | 5           |
| **Tenure**         | 12 months     | 3           |

Churn Score=(0.3×5)+(0.2×4)+(0.2×3)+(0.15×3)+(0.1×5)+(0.05×3)

Churn Score=1.5+0.8+0.6+0.45+0.5+0.15=4.

**Final Score:** 4.0 → This customer is **\"Likely Active.\"**

### Step 5: Develop a classification model

To predict churn (whether a customer will be active or inactive),
a **classification model** is the appropriate choice, as the target
variable (churn) is binary.

The possible classification models that can be used for churn prediction
include:

1)  **Logistic Regression**, 

2)  **Random Forest**, 

3)  **Gradient Boosting (e.g., XGBoost, LightGBM)**, 

4)  **Support Vector Machine (SVM)**, 

5)  **Naive Bayes**, 

6)  **K-Nearest Neighbors (KNN)**, and 

7)  **Neural Networks**.

To ensure the developed model performing well, you may need to get the
accuracy, precision, recall, F1-Score.

### Step 6: Model Evaluation and Tuning

-   **Cross-validation**: To ensure your model is generalizing well and
    not overfitting to the training data, use techniques like **k-fold
    cross-validation**. This splits the dataset into multiple folds and
    trains the model on each fold, providing a more reliable evaluation.

-   **Hyperparameter Tuning**: For more complex models like Random
    Forests or Gradient Boosting, consider tuning the hyperparameters
    (e.g., number of trees, learning rate, depth of the tree) to improve
    model performance. Use **Grid Search** or **Random
    Search** techniques for hyperparameter optimization.

-   **Threshold Adjustment**: Experiment with different classification
    thresholds (e.g., 0.3, 0.7) to see if you can improve the balance
    between precision and recall, depending on your business objectives
    (e.g., minimizing false negatives or false positives).

### Step 7: Act on Predictions (Decision Made based on result)

> Segment customers by churn probability:

-   **High-risk customers**: Offer retention incentives like discounts
    or personalized offers.

-   **Moderate-risk customers**: Increase engagement with targeted
    marketing or educational content about product benefits.

-   **Low-risk customers**: Maintain satisfaction with ongoing support
    and loyalty rewards.

### Step 8: Presentation of the result

After the predictions are made and decisions are formed based on the
churn probability, it\'s crucial to communicate these findings and
decisions to key stakeholders, such as managers, executives, or other
teams (e.g., marketing, customer support).

**Set Meeting** **to present the outcome**:

The goal of the meeting would be to explain the churn prediction
model\'s output and provide actionable insights. This would involve
presenting the model's findings, such as:

-   The number of high-risk customers identified

-   Suggested actions for retention

-   Potential impacts on business (e.g., how retention can improve
    revenue or reduce churn rate)

**Power BI/Tableau Usage**:

A detailed report, often in the form of a dashboard (using tools like
**Power BI or Tableau**), is essential for tracking, presenting, and
reviewing results.

-   This report will summarize key findings from the churn analysis,
    track predictions versus actual churn (if available), and present
    actionable insights.

-   It helps make the data-driven insights accessible to non-technical
    stakeholders, as well as for future reference when making strategic
    decisions.
