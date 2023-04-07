# Quick view of all available tables

<img width="825" alt="134619326-f560a7b0-23b2-42ba-964b-95b3c8d55c76" src="https://user-images.githubusercontent.com/86810684/230513359-a4e846cc-81cf-45a5-b494-417bf2c5f05a.png">

## USER 

| user_id | cookie_id | start_date               |
| ------- | --------- | ------------------------ |
| 1       | c4ca42    | 2020-02-04T00:00:00.000Z |
| 2       | c81e72    | 2020-01-18T00:00:00.000Z |
| 3       | eccbc8    | 2020-02-21T00:00:00.000Z |
| 4       | a87ff6    | 2020-02-22T00:00:00.000Z |
| 5       | e4da3b    | 2020-02-01T00:00:00.000Z |

## EVENTS

| visit_id | cookie_id | page_id | event_type | sequence_number | event_time               |
| -------- | --------- | ------- | ---------- | --------------- | ------------------------ |
| ccf365   | c4ca42    | 1       | 1          | 1               | 2020-02-04T19:16:09.182Z |
| ccf365   | c4ca42    | 2       | 1          | 2               | 2020-02-04T19:16:17.358Z |
| ccf365   | c4ca42    | 6       | 1          | 3               | 2020-02-04T19:16:58.454Z |
| ccf365   | c4ca42    | 9       | 1          | 4               | 2020-02-04T19:16:58.609Z |
| ccf365   | c4ca42    | 9       | 2          | 5               | 2020-02-04T19:17:51.729Z |

## PAGE_HIERARCHY

| page_id | page_name      | product_category | product_id |
| ------- | -------------- | ---------------- | ---------- |
| 1       | Home Page      |                  |            |
| 2       | All Products   |                  |            |
| 3       | Salmon         | Fish             | 1          |
| 4       | Kingfish       | Fish             | 2          |
| 5       | Tuna           | Fish             | 3          |
| 6       | Russian Caviar | Luxury           | 4          |
| 7       | Black Truffle  | Luxury           | 5          |
| 8       | Abalone        | Shellfish        | 6          |
| 9       | Lobster        | Shellfish        | 7          |
| 10      | Crab           | Shellfish        | 8          |
| 11      | Oyster         | Shellfish        | 9          |
| 12      | Checkout       |                  |            |
| 13      | Confirmation   |                  |            |

## EVENT_IDENTIFIER

| event_type | event_name    |
| ---------- | ------------- |
| 1          | Page View     |
| 2          | Add to Cart   |
| 3          | Purchase      |
| 4          | Ad Impression |
| 5          | Ad Click      |

## CAMPAIGN_IDENTIFIER

| campaign_id | products | campaign_name                     | start_date               | end_date                 |
| ----------- | -------- | --------------------------------- | ------------------------ | ------------------------ |
| 1           | 1-3      | BOGOF - Fishing For Compliments   | 2020-01-01T00:00:00.000Z | 2020-01-14T00:00:00.000Z |
| 2           | 4-5      | 25% Off - Living The Lux Life     | 2020-01-15T00:00:00.000Z | 2020-01-28T00:00:00.000Z |
| 3           | 6-8      | Half Off - Treat Your Shellf(ish) | 2020-02-01T00:00:00.000Z | 2020-03-31T00:00:00.000Z |

