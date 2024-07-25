# Yammer Case Study

**Yammer** was founded in **2008** as a **freemium enterprise social networking service** used for private communication within organizations. It was acquired by **Microsoft for $1.2 billion in 2012** and is now available in all **Office 365 products**.

## Table of Contents
- [Problem](#problem)
- [Key Metrics and Dimensions](#key-metrics-and-dimensions)
- [Tables](#tables)
- [Summary and Insight](#summary-and-insight)

## Problem
The head of the Product team walks over to your desk and asks you what you think about the latest activity on the user engagement dashboards. You fire them up, and **something immediately jumps out**.

## Key Metrics and Dimensions
- **Engagement:** Any interaction done by users in the server
- **Period:** 28 April 2014 to 25 August 2014

## Summary and Insight
<img src="Yammal/weekly_engage.png" alt="User Engagement Chart" width="800" />

- **Problem:** What caused the dip at the end of the user engagement chart?

```sql
SELECT DATE_TRUNC('week', e.occurred_at) AS week_date,
       COUNT(DISTINCT e.user_id) AS weekly_active_users
  FROM tutorial.yammer_events e
 WHERE e.event_type = 'engagement'
   AND e.event_name = 'login'
 GROUP BY 1
 ORDER BY 1;
```

## 1. Initial Analysis: What can we understand from the graph and data

The graph depicts the aggregate number of weekly active users who logged in and interacted with various app features, such as commenting, sending emails, and searching, as detailed in the event table. User engagement demonstrated a consistent upward trend through mid-2014. However, a significant drop occurred at the end of July, with a reduction of approximately 200 users, reverting engagement levels to those observed in June. This lower level of engagement persisted through August.

Although the graph does not explicitly explain the cause of the engagement decline, the accompanying data tables—which include information on demographics such as location, device usage, email interactions, and platform sign-ups—may offer valuable insights into the underlying reasons for this decrease.




## 2. Identify Potential Problems

- **Broken Feature:** There may be a malfunction within the application that is preventing users from utilizing certain features. Identifying such issues can be challenging, as different parts of the application might impact metrics in various ways. For instance, if a critical component of the signup process fails, it could lead to a reduction in new user registrations and overall growth. Similarly, if the mobile app experiences instability or crashes, it may only affect engagement on that specific device type.

- **Device Incompatibility:** The Yammer app or website might not function optimally on certain devices, which could prevent users from accessing various tools and features. This incompatibility could contribute to decreased user engagement and satisfaction.

- **Product Growth:** A potential decline in new user sign-ups could be contributing to lower engagement levels. If the sign-up process is experiencing issues or bugs, it might prevent new users from registering, thereby affecting overall engagement with the Yammer app.

- **Time of Year:** Engagement levels may be impacted by seasonal factors such as public holidays or vacations. These periods can lead to lower user activity due to reduced availability or engagement from users in different regions.

- **Location-Based Factors:** As a global application, Yammer may face competition from local alternatives in specific regions. Local competitors could potentially attract users away from Yammer, affecting its overall engagement and traction in those areas.



## 3. Investigate Data

### 3.1 Weekly Active Users
![Active User Table](Yammal/Active_users.png)

- **Investigate User Trends:** Conduct a detailed analysis of active user trends to understand their correlation with user engagement levels. Initial observations suggest that the number of active users aligns consistently with engagement counts. However, this correlation alone does not fully account for the decline in engagement. To gain a complete understanding, further investigation is necessary to identify any additional factors or underlying issues that may be contributing to this decline. This may involve examining specific user behaviors, feature usage patterns, and potential external influences affecting engagement.


### 3.2 Product Growth

![Active User Table](Yammal/user_growth.png)

- **Definitions:**
  - **Active User:** A user who has signed up, activated their profile, and is actively engaging with the product.
  - **Pending User:** A user who has signed up for an account but has not yet completed authentication or engaged with the product's content.
  - **New User:** A user who has recently created an account but may not yet be fully activated or engaged.

- **User Growth:** The user base for the product is expanding at a healthy rate, with no apparent issues in the sign-up process. There are no indications of a decline in the number of newly activated users. This suggests that the user acquisition process is functioning well and that new user activation rates remain stable.





## Appendix


### SQL Problem

```sql
SELECT DATE_TRUNC('week', e.occurred_at) AS week_date,
       COUNT(DISTINCT e.user_id) AS weekly_active_users
  FROM tutorial.yammer_events e
 WHERE e.event_type = 'engagement'
   AND e.event_name = 'login'
 GROUP BY 1
 ORDER BY 1;
```


### 3.1 Weekly Active Users

```sql
SELECT DATE_TRUNC('week', e.occurred_at) as week_date,
       COUNT(DISTINCT e.user_id) AS weekly_active_users
  FROM tutorial.yammer_events e
 WHERE e.event_type = 'engagement'
   AND e.event_name = 'login'
 GROUP BY 1
 ORDER BY 1;

```


### 3.2 Product Growth


```sql
SELECT DATE_TRUNC('week', e.occurred_at) AS week_start_date,
      COUNT(DISTINCT(CASE WHEN e.event_type = 'engagement' THEN user_id END)) AS active_users,
      COUNT(DISTINCT(CASE WHEN e.event_type = 'signup_flow' THEN user_id END)) AS signup_users,
      COUNT(DISTINCT user_id) AS total_users
FROM tutorial.yammer_events e
GROUP BY week_start_date
ORDER BY week_start_date;

```


### 3.1 
 Excel


## Tables

### User Table
<img src="Tables/users.png" alt="User Table" width="400" height="300" />

### Email Table
<img src="Tables/email.png" alt="Email Table" width="400" height="300" />

### Event Table
<img src="Tables/events.png" alt="Event Table" width="400" height="300" />

