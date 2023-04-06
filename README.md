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


## **A. Digital Analysis**
---

    SELECT 
    	COUNT(DISTINCT user_id) as user_num
    FROM clique_bait.users;

| user_num |
| --- |
| 500 |

2. How many cookies does each user have on average?

    WITH cookies as (
    SELECT
    	user_id,
        COUNT(cookie_id) as num_cookies
    FROM clique_bait.users
    Group by user_id)
    
    
    SELECT 
    	AVG(num_cookies) as Avg_cookies_per_customer
    FROM cookies;

| avg_cookies_per_customer |
| ------------------------ |
| 3.5640000000000000       |

---
