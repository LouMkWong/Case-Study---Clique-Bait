-- to calculate which promotion generated the most sales
````sql
    WITH SUMMARY2 as (
    SELECT
    	u.user_id,
        e.visit_id,
        MIN(e.event_time) as time_of_visit,
        SUM(CASE WHEN e.event_type = 1 THEN 1 ELSE 0 END) as page_viewed, 
        SUM(CASE WHEN e.event_type = 2 THEN 1 ELSE 0 END) as added_cart, 
        SUM(CASE WHEN e.event_type = 3 THEN 1 ELSE 0 END) as purchased, 
        SUM(CASE WHEN e.event_type = 4 THEN 1 ELSE 0 END) as ad_impression, 
        SUM(CASE WHEN e.event_type = 5 THEN 1 ELSE 0 END) as ad_clicked, 
        ci.campaign_name
    FROM clique_bait.users as u
    INNER JOIN clique_bait.events as e ON u.cookie_id = e.cookie_id
    LEFT JOIN clique_bait.campaign_identifier as ci ON e.event_time BETWEEN ci.start_date AND ci.end_date
    GROUP BY u.user_id, e.visit_id, ci.campaign_name
    ORDER BY 1)
    
    SELECT
    	campaign_name,
    	COUNT(campaign_name)
    FROM SUMMARY2
    WHERE purchased = 1 AND campaign_name is NOT NULL
    GROUP BY campaign_name
    ORDER BY 2 DESC;
````
