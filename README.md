
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


3. What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value
```sql
-- What is the monthly distribution of trial plan start_date values for our dataset - use the start of the month as the group by value
select plan_id,month(start_date) as month,count(month(start_date)) as count from subscriptions
group by month(start_date),plan_id
having plan_id=0;
```
4. What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name

5. What is the customer count and percentage of customers who have churned rounded to 1 decimal place?

6. How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?

7. What is the number and percentage of customer plans after their initial free trial?

8. What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?

9. How many customers have upgraded to an annual plan in 2020?

10. How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?

11. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)

12. How many customers downgraded from a pro monthly to a basic monthly plan in 2020?





