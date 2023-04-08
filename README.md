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
    SELECT
        	ROUND(
        	100
            *CAST((SELECT COUNT(event_type) FROM clique_bait.events where event_type = 3) as numeric)
            / COUNT(DISTINCT visit_id)
            , 2) AS percentage_purchase_per_visit
        FROM clique_bait.events;
````
| percentage_purchase_per_visit |
| ----------------------------- |
| 49.86                         |

### 6. What is the percentage of visits which view the checkout page but do not have a purchase event?
````sql
    -- where page id 12 is the check out page, page id 13 is the confirmation page 
    ---and event type 3 is the event when customer make a purchase

    -- first calculate the percentage of how many people make a purchase after view the purchase page 
    -- then subtract 1 from it to get the percentage of customer who viewed the checkout page but didn't make a purchase
    SELECT 
    	100 - 
        ROUND(
        (100*CAST(COUNT(*) as numeric) / 
        (SELECT COUNT(*) FROM clique_bait.events WHERE page_id = 12 GROUP BY page_id))
        , 2) as PCT_NO_PURCHASE
    FROM clique_bait.events
    WHERE page_id = 13 and event_type = 3
    GROUP BY page_id;
````
| pct_no_purchase |
| --------------- |
| 15.50           |

#7. What are the top 3 pages by number of views?
````sql
    SELECT 
    	ph.page_name,
        COUNT(*)
    FROM clique_bait.events AS e 
    JOIN clique_bait.page_hierarchy AS ph ON e.page_id = ph.page_id
    WHERE e.event_type = 1 
    GROUP BY ph.page_name
    ORDER BY 2 DESC
    LIMIT 10;
````
| page_name      | count |
| -------------- | ----- |
| All Products   | 3174  |
| Checkout       | 2103  |
| Home Page      | 1782  |
| Oyster         | 1568  |
| Crab           | 1564  |
| Russian Caviar | 1563  |
| Kingfish       | 1559  |
| Salmon         | 1559  |
| Lobster        | 1547  |
| Abalone        | 1525  |

> As shown above, the top 3 pages by number of view are "ALL PRODUCTS", "CHECKOUT", and "HOME PAGE".  However, the top 3 viewed PRODUCT page would be "Oyster", "Crab" and "Russian Caviar)

#8. What is the number of views and cart adds for each product category?
````sql
    SELECT 
    	ph.product_category,
        SUM(CASE WHEN e.event_type = 1 THEN 1 ELSE 0 END) as view_count, -- if event type is 1 (page viewed) then we assign a 1 and sum them together
        SUM(CASE WHEN e.event_type = 2 THEN 1 ELSE 0 END) as added_to_cart_count, -- if event type is 2 (add to cart) then we assign a 1 and sum them together
        ROUND(CAST(SUM(CASE WHEN e.event_type = 2 THEN 1 ELSE 0 END) as numeric)/SUM(CASE WHEN e.event_type = 1 THEN 1 ELSE 0 END), 2) as rate
    FROM clique_bait.events as e 
    JOIN clique_bait.page_hierarchy as ph on e.page_id = ph.page_id
    WHERE ph.product_category is NOT NULL
    GROUP BY ph.product_category
    ORDER BY 3 DESC;
````
| product_category | view_count | added_to_cart_count | rate |
| ---------------- | ---------- | ------------------- | ---- |
| Shellfish        | 6204       | 3792                | 0.61 |
| Fish             | 4633       | 2789                | 0.60 |
| Luxury           | 3032       | 1870                | 0.62 |

> Note that each view has a 60 percent chance that the product will get added to cart for all 3 category
---



