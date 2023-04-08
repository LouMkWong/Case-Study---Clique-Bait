
````sql
    
    WITH SUMMARY as (
    WITH cart1 as (
    SELECT 
       	ph.page_name as Product_name,
        COUNT(*) as added_to_cart
    FROM clique_bait.events AS e 
    JOIN clique_bait.page_hierarchy AS ph ON e.page_id = ph.page_id
    WHERE e.event_type = 2 
    GROUP BY ph.page_name),
    
    view1 as (
    SELECT 
       	ph.page_name as Product_name,
        COUNT(*) as view_count
    FROM clique_bait.events AS e 
    JOIN clique_bait.page_hierarchy AS ph ON e.page_id = ph.page_id
    WHERE e.event_type = 1 
    GROUP BY ph.page_name),
    
    purchased1 as (    
      WITH purchase as (
          SELECT
          	visit_id,
          	event_type
          FROM clique_bait.events
          WHERE event_type = 3)
    
        SELECT
        	ph.page_name as Product_name,
      		ph.product_category as category,
            COUNT(e.page_id) as Purchase_made
        FROM clique_bait.events AS e
        JOIN purchase as p on e.visit_id = p.visit_id
        JOIN clique_bait.page_hierarchy as ph on e.page_id = ph.page_id
        WHERE e.event_type = 2 
        GROUP BY ph.page_name, ph.product_category)
        
    SELECT 
    	c.Product_name,
        p.category,
        v.view_count,
        c.added_to_cart,
        p.Purchase_made,
        c.added_to_cart - p.Purchase_made as abandoned,
        ROUND(100*CAST(p.Purchase_made as numeric)/v.view_count, 2) as view_to_purchase_pct,
        ROUND(100*CAST(c.added_to_cart as numeric)/v.view_count, 2) as view_to_cart_add_pct,
        ROUND(100*CAST(p.Purchase_made as numeric)/c.added_to_cart, 2) as cart_add_to_purchase_pct
    FROM cart1 as c 
    JOIN view1 as v on c.Product_name = v.Product_name
    JOIN purchased1 as p on v.Product_name = p.Product_name
    ORDER BY 2, 3 DESC)
    
    SELECT
    	ROUND(AVG(view_to_cart_add_pct), 2) as Average_Conversion_rate_from_view_to_cart_add,
        ROUND(AVG(cart_add_to_purchase_pct), 2) as Average_Conversion_rate_from_cart_add_to_purchase
    FROM SUMMARY;
````
