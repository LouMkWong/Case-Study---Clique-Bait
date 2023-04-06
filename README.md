# Case-Study---Clique-Bait
8weeksqlchallenge.com

<a href="url"><img src="https://8weeksqlchallenge.com/images/case-study-designs/6.png" height="750" width="750" ></a>

## **Task**

Clique Bait is an online seafood store
</BR></BR>
In this case study - you are required to support Danny’s vision and analyse his dataset and come up with creative solutions to calculate funnel fallout rates for the Clique Bait online store.
</BR></BR>
## **Tables Relationship Diagram**

<img width="825" alt="134619326-f560a7b0-23b2-42ba-964b-95b3c8d55c76" src="https://user-images.githubusercontent.com/86810684/230500020-e0112031-2ef6-4d61-95d4-cac31872cc33.png">

---
## **A. Digital Analysis**
### 1. How many customers does Clique Bait has?
````sql
    SELECT 
    	COUNT(DISTINCT user_id) as user_num
    FROM clique_bait.users;
````

| user_num |
| --- |
| 500 |


### 2. How many cookies does each user have on average?
````sql
    -- Creating a CTE to get the total cookies per customers
    -- using it to calculate the average per customers later
    WITH cookies as (
    SELECT
    	user_id,
        COUNT(cookie_id) as num_cookies
    FROM clique_bait.users
    Group by user_id)
    
    -- Calculating the average cookie per customer
    SELECT 
    	AVG(num_cookies) as Avg_cookies_per_customer
    FROM cookies;
````

| avg_cookies_per_customer |
| ------------------------ |
| 3.5640000000000000       |

### 3. What is the unique number of visits by all users per month?
````sql
    SELECT 
    	EXTRACT(MONTH FROM event_time) as Month,
        COUNT(DISTINCT visit_id) as Visit_count
    FROM clique_bait.events
    GROUP BY EXTRACT(MONTH FROM event_time);
````

| month | visit_count |
| ----- | ----------- |
| 1     | 876         |
| 2     | 1488        |
| 3     | 916         |
| 4     | 248         |
| 5     | 36          |

### 4. What is the number of events for each event type?
````sql
    -- Joining event table with event identifier to get the corresponding event name
    -- group by event type, event name to get the total count of each event
   SELECT 
    	e.event_type,
        ei.event_name,
        COUNT(*) as count
    FROM clique_bait.events as e
    LEFT JOIN clique_bait.event_identifier as ei 
    ON e.event_type = ei.event_type
    GROUP BY e.event_type, ei.event_name
    ORDER BY e.event_type;
````
| event_type | event_name    | count |
| ---------- | ------------- | ----- |
| 1          | Page View     | 20928 |
| 2          | Add to Cart   | 8451  |
| 3          | Purchase      | 1777  |
| 4          | Ad Impression | 876   |
| 5          | Ad Click      | 702   |

### 5. What is the percentage of visits which have a purchase event?
````sql
    --divide total number purchases by the total number of distinct visit 
    SELECT
    	100
        *CAST((SELECT COUNT(event_type) FROM clique_bait.events where event_type = 3) as int)
        / COUNT(DISTINCT visit_id) AS percentage_purchase_per_visit
    FROM clique_bait.events;
````
| percentage_purchase_per_visit |
| ----------------------------- |
| 49                            |

### 6. What is the percentage of visits which view the checkout page but do not have a purchase event?
---



