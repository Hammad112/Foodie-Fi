
# SQL CASE STUDY FOODIE-FI 
## INRTODUCTION:

Subscription based businesses are super popular and Danny realised that there was a large gap in the market - he wanted to create a new streaming service that only had food related content - something like Netflix but with only cooking shows!

Danny finds a few smart friends to launch his new startup Foodie-Fi in 2020 and started selling monthly and annual subscriptions, giving their customers unlimited on-demand access to exclusive food videos from around the world!

Danny created Foodie-Fi with a data driven mindset and wanted to ensure all future investment decisions and new features were decided using data. This case study focuses on using subscription style digital data to answer important business questions.

## ERD Diagram

![ERD](https://github.com/Hammad112/Foodie-Fi/assets/95902997/722170c3-951f-44b5-9f3d-aca8617e474d)

## Poster

![FOOD-FIE Posters](https://github.com/Hammad112/Foodie-Fi/assets/95902997/1b67b1f7-2c44-4501-b735-098cb3457f3d)

## Data Analysis Questions

1. How many customers has Foodie-Fi ever had?
   ```sql
      -- How many customers has Foodie-Fi ever had...
      Select  count(distinct customer_id) as Customers from subscriptions;
   ```

3. What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value
  ```sql
      -- What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value
      select plan_id,month(start_date) as month,count(month(start_date)) as count from subscriptions
      group by month(start_date),plan_id
      having plan_id=0;
  ```
4. What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name
   ```sql
        select plans.plan_name,sp.plan_id ,year(start_date)  as year,count(year(start_date)) as Count_of_events from subscriptions as sp
        join plans on sp.plan_id=plans.plan_id
        where year(start_date)>2020
        group by plans.plan_name,plan_id,year(start_date);
   ```
5. What is the customer count and percentage of customers who have churned rounded to 1 decimal place?
  ```sql
         select plan_name,
         count((select count(distinct customer_id) from subscriptions)) as count_of_churned,
         ROUND(count(plans.plan_name)/(select count(distinct customer_id) from subscriptions),1)*100 as Percentage
         from subscriptions as sp
         join plans on sp.plan_id=plans.plan_id
         where plan_name='churn';
  ```
6. How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?
   ```sql
        select plan_id,count(customer_id) as count_customer,
        Round(count(customer_id)/(select count(customer_id) from subscriptions),3)*100 as Percentage
        from subscriptions where plan_id=4
        and customer_id in(select customer_id from subscriptions where plan_id=0);         
    ```
8. What is the number and percentage of customer plans after their initial free trial?
    ```sql
         SELECT COUNT(plan_id) AS count_cust,
         ROUND(count(customer_id)/(select count(customer_id) from subscriptions),2)*100 as Percentage
         FROM subscriptions
         WHERE plan_id != 0;
    ```
9. What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?
      ```sql
        select plan_name,count(customer_id) as count,
        ROUND(count(plan_name)/(select count(customer_id) from subscriptions),5)*100 as Percentage
        from subscriptions as sp
        join plans as p on sp.plan_id=p.plan_id 
        where start_date='2020-12-31'
        group by plan_name;
      ```
10. How many customers have upgraded to an annual plan in 2020?
     ```sql
       select plan_name,count(customer_id) as count
       from subscriptions as sp
       join plans as p on sp.plan_id=p.plan_id 
       where plan_name='pro annual' AND year(start_date)='2020';
    ```
11. How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?
    ```sql
       Select count(customer_id) as No_of_customers,AVG(DATEDIFF(
       (Select Min(start_date) from subscriptions as s1 where s1.customer_id=s2.customer_id and plan_id=3),
       (Select MIN(start_date) from subscriptions as s3 where s3.customer_id=s2.customer_id )))
       as AVG_Days_to_reach_annual_program
       from subscriptions as s2
       where plan_id !=3;
   ```
12. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)
   ```sql
       SELECT 
       CASE 
         WHEN days_difference >= 0 AND days_difference <= 30 THEN '0-30 days'
         WHEN days_difference > 30 AND days_difference <= 60 THEN '31-60 days'
         WHEN days_difference > 61 AND days_difference <= 90 THEN '61-90 days'
         ELSE 'More than 90 days'  END AS period,
         COUNT(customer_id) AS No_of_customers,
         AVG(days_difference) AS AVG_Days_to_reach_annual_program
        FROM (
            SELECT 
            s2.customer_id,
                   DATEDIFF(
                        (SELECT MIN(start_date) FROM subscriptions AS s1 WHERE s1.customer_id = s2.customer_id AND plan_id = 3),
                        (SELECT MIN(start_date) FROM subscriptions AS s3 WHERE s3.customer_id = s2.customer_id)) AS days_difference
             FROM subscriptions AS s2 WHERE plan_id != 3 AND plan_id != 4) AS differences
             GROUP BY period
             ORDER BY period;
 ```
13. How many customers downgraded from a pro monthly to a basic monthly plan in 2020?
   ```sql
       select plan_id,count(distinct customer_id)as downgraded_from_annual_to_basic from subscriptions
       where plan_id =1
       and customer_id in 
       (select distinct customer_id from subscriptions where plan_id=2 and year(start_date)=2020);
    ```




