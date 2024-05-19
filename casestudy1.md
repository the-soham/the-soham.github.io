# SQL Case Study #1 - Danny's Diner

Hey data enthusiasts! If you've mastered all the SQL questions on LeetCode and are itching to dive into more advanced concepts with real-world scenarios, you're in the right place. Welcome to an exciting journey through Challenge #1 of [Danny Ma's](https://www.linkedin.com/in/datawithdanny/) comprehensive [SQL 8-Week Challenge](https://8weeksqlchallenge.com/case-study-1/).

In this blog, we will explore:

* **JOINS**: Seamlessly combining data from multiple tables.
* **Window Functions**: Performing calculations across a set of table rows related to the current row.
* **CTEs (Common Table Expressions)**: Simplifying complex queries and improving readability.
* **CASE Expressions**: Adding conditional logic to your queries.
* **Aggregation**: Summarizing data with functions like SUM, COUNT, AVG, and more.

Ready to enhance your SQL skills with practical, real-world data scenarios? Let's dive in and start our adventure!


![This is an alt text.](https://8weeksqlchallenge.com/images/case-study-designs/1.png "This is a sample image.")

## Introduction
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.

Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

## Problem Statement
Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favourite. Having this deeper connection with his customers will help him deliver a better and more personalised experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:
* sales
* menu
* members

## Entity Relationship Diagram
![image](https://github.com/the-soham/the-soham.github.io/assets/60706236/d5383e4f-413b-4ce6-969d-fe48534e7d45)

## Schema DDL

<details open>
<summary>Click me for the schema script</summary>
<pre>CREATE SCHEMA dannys_diner;
SET search_path = dannys_diner;

CREATE TABLE sales (
  "customer_id" VARCHAR(1),
  "order_date" DATE,
  "product_id" INTEGER
);

INSERT INTO sales
  ("customer_id", "order_date", "product_id")
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');
 

CREATE TABLE menu (
  "product_id" INTEGER,
  "product_name" VARCHAR(5),
  "price" INTEGER
);

INSERT INTO menu
  ("product_id", "product_name", "price")
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');
  

CREATE TABLE members (
  "customer_id" VARCHAR(1),
  "join_date" DATE
);

INSERT INTO members
  ("customer_id", "join_date")
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');</pre>
</details>


## We will try to answer the following questions:

1. What is the total amount each customer spent at the restaurant?
2. How many days has each customer visited the restaurant?
3. What was the first item from the menu purchased by each customer?
4. What is the most purchased item on the menu and how many times was it purchased by all customers?
5. Which item was the most popular for each customer?
6. Which item was purchased first by the customer after they became a member?
7. Which item was purchased just before the customer became a member?
8. What is the total items and amount spent for each member before they became a member?
9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?


## Let's jump straight to code:

1. What is the total amount each customer spent at the restaurant?
   
    <details>
    <summary>Hint1</summary>
    <br>
    customer_id and price are columns from different tables!<br>
    What JOIN should we use?Yes, correct INNER JOIN
    </details>


    <details>
    <summary>Hint2</summary>
    <br>
    Now aggregate the amount spent by each person using the SUM() function . <br>
    Are you still getting error?<br>
    Did you use GROUP BY for the non aggregated columns?
    </details>


    <details>
    <summary>Solution</summary>
    <pre>
      SELECT s.customer_id, SUM(m.price) as amt_spent
      FROM sales s 
      INNER JOIN menu m ON s.product_id = m.product_id
      GROUP BY s.customer_id
      ORDER BY s.customer_id ASC</pre>
    </details>


2. How many days has each customer visited the restaurant?

   <details>
    <summary>Hint1</summary>
    <br>
    Select customer_id and order_date columns<br>
    Did you use the COUNT() function to find the number of visits.    
    </details>


    <details>
    <summary>Hint2</summary>
    <br>
    Did you use GROUP BY for the non-aggregated column<br>
    Are you still getting incorrect answer?<br>
    Did you use DISTINCT keyword inside COUNT() function to get the correct answer?
    </details>


    <details>
    <summary>Solution</summary>
    <pre>
      SELECT s.customer_id, COUNT(DISTINCT s.order_Date) as no_of_visits
      FROM sales s
      GROUP BY s.customer_id </pre>
    </details>




3. What was the first item from the menu purchased by each customer?
    
   <details>
    <summary>Hint1</summary>
    <br>
    Select customer_id and order_date columns<br>
    Did you use the COUNT() function to find the number of visits.    
    </details>

    <details>
    <summary>Hint2</summary>
    <br>
    Did you use GROUP BY for the non-aggregated column<br>
    Are you still getting incorrect answer?<br>
    Did you use DISTINCT keyword inside COUNT() function to get the correct answer?
    </details>

    <details>
    <summary>Solution</summary>
    <pre>
      SELECT s.customer_id, COUNT(DISTINCT s.order_Date) as no_of_visits
      FROM sales s
      GROUP BY s.customer_id </pre>
    </details>




4. What is the most purchased item on the menu and how many times was it purchased by all customers?

    
   <details>
    <summary>Hint1</summary>
    <br>
    This one is simple!<br>
    Use the COUNT() function to count how many times each item was bought    
    </details>


    <details>
    <summary>Hint2</summary>
    <br>
    Did you use GROUP BY for the non-aggregated column<br>
    Are you still getting incorrect answer?<br>
    Order by COUNT() in descending ORDER and get the top most product name.
    </details>


    <details>
    <summary>Solution</summary>
    <pre>
      SELECT m.product_name, COUNT(1) as times_bought
      FROM sales s
      INNER JOIN menu m ON s.product_id = m.product_id
      GROUP BY m.product_name
      ORDER BY times_bought DESC 
      LIMIT 1 </pre>
    </details>




5.  Which item was the most popular for each customer?

    
     <details>
    <summary>Hint1</summary>
    <br>
    At first find which customers have bought which items and their count using the COUNT() function.   
    </details>


    <details>
    <summary>Hint2</summary>
    <br>
    Did you use GROUP BY for the non-aggregated column<br>
    Are you still getting incorrect answer?<br>
    Create a CTE for this query
    </details>


    <details>
    <summary>Hint3</summary>
    <br>
    Now create another CTE to give the rank to each of the item that has been bought based on its count. <br>
    use RANK() PARTITION BY the customer_id and ORDER BY count in descending order.
    </details>


    <details>
    <summary>Hint4</summary>
    <br>
    Now you have the rankings of the items based on the count <br>
    use ARRAY_AGG() to group together items that may have the same count. <br>
    Ohh! you only need the popular item so select the one with rank=1
    </details>
    
    <details>
    <summary>Solution</summary>
    <pre>
      
    WITH purchase AS(
    SELECT s.customer_id, s.product_id, COUNT(1) as cnt
    FROM sales s
    GROUP BY s.customer_id,s.product_id
    ORDER BY s.customer_id, cnt DESC
    ), ranking AS (
    SELECT p.customer_id, m.product_name, cnt, RANK() OVER(PARTITION BY p.customer_id ORDER BY cnt DESC) AS rnk
    from purchase p
    INNER JOIN menu m ON p.product_id = m.product_id)
    SELECT r.customer_id
    	   , ARRAY_AGG(r.product_name) AS popular_item
    	   , cnt as no_of_times_bought
    from ranking r
    WHERE rnk = 1
    GROUP BY r.customer_id,cnt  </pre>
    </details>




6. Which item was purchased first by the customer after they became a member?

    
     <details>
    <summary>Hint1</summary>
    <br>
    You'll need all the three tables in this query.
    First create a CTE to get the customer_id, order_date, product_name and use the RANK() function to giving the ranking to the items based on the order date
    </details>

    <details>
    <summary>Hint2</summary>
    <br>
    In the above CTE did you use the condition that order_date should be greater than join_date.
    From the CTE fing the customer_id and the product where the rank =1 
    </details>
    
    <details>
    <summary>Solution</summary>
    <pre>
      
    WITH post_join_date AS (
    SELECT s.customer_id
    	  , s.order_date
    	  , me.product_name
    	  , RANK() OVER (PARTITION BY s.customer_id ORDER BY s.order_date) AS rnk
    FROM sales s
    LEFT JOIN members m ON s.customer_id = m.customer_id
    INNER JOIN menu me ON s.product_id = me.product_id
    WHERE s.order_date > m.join_date
    )
    
    SELECT pj.customer_id
    	  , pj.order_date
    	  , pj.product_name
    FROM post_join_date pj 
    WHERE rnk =1  </pre>
    </details>



7. Which item was purchased just before the customer became a member?

    
     <details>
    <summary>Hint1</summary>
    <br>
    Use the same logic as the prior query. 
    Just ORDER BY order_Date in descending order in the windiw function
    </details>


    <details>
    <summary>Hint2</summary>
    <br>
    use the condition that order_date should be less than join_date.
    From the CTE fing the customer_id and the product where the rank =1 
    </details>

    
    <details>
    <summary>Solution</summary>
    <pre>
      
     WITH pre_join_date AS (
    SELECT s.customer_id
    	 , s.order_date
    	 , me.product_name
    	 , RANK() OVER (PARTITION BY s.customer_id ORDER BY s.order_date DESC) AS rnk
    FROM sales s
    LEFT JOIN members m ON s.customer_id = m.customer_id
    INNER JOIN menu me ON s.product_id = me.product_id
    WHERE s.order_date < m.join_date
    )
    SELECT pj.customer_id
    	 , pj.order_date
    	 , ARRAY_AGG(pj.product_name)
    FROM pre_join_date pj 
    WHERE rnk =1
    GROUP BY pj.customer_id, pj.order_date, pj.rnk </pre>
    </details>


8. What is the total items and amount spent for each member before they became a member?

    
     <details>
    <summary>Hint1</summary>
    <br>
    This query is simple. Try one more time!
    </details>

    <details>
    <summary>Hint2</summary>
    <br>
    Read carefully, total items and total amount simply translate to the use of COUNT() and SUM() function respectively. 
    Be careful tho! order_date should be less than the join date.
    </details>
    
    <details>
    <summary>Solution</summary>
    <pre>
      
   SELECT s.customer_id
  	 , COUNT(1) as total_items
  	 , SUM(m.price) AS amount_spent
  FROM sales s
  INNER JOIN menu m ON s.product_id = m.product_id
  INNER JOIN members mem ON s.customer_id = mem.customer_id
  WHERE s.order_date < mem.join_date
  GROUP BY s.customer_id
  ORDER BY s.customer_id</pre>
    </details>

9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
    
    <details>
    <summary>Hint1</summary>
    <br>
    The only hint here: Use the CASE WHEN statements.
    Did you use SUM() to calculate the points.
    </details>
    
    <details>
    <summary>Solution</summary>
    <pre>
      
     SELECT s.customer_id
    	  ,	SUM(CASE WHEN m.product_name = 'sushi' THEN 2*m.price ELSE m.price END) as points 
    FROM sales s
    INNER JOIN menu m ON s.product_id = m.product_id
    GROUP BY s.customer_id
    ORDER BY s.customer_id
    </pre>
    </details>



10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?
    
    <details>
    <summary>Hint1</summary>
    <br>
    The only hint here: Use the CASE WHEN statements.
    Use the DATE+INTERVAL function to find the date 1 week after the joining date.
    </details>

   <br>

    <details>
    <summary>Hint2</summary>
    <br>
    Make sure you filter the order_Date for all orders before Jan 31.
    </details>
    
    <br>
    
    <details>
    <summary>Solution</summary>
    <pre>
      
      SELECT s.customer_id
            ,SUM(CASE WHEN s.order_date BETWEEN mem.join_date AND DATE(mem.join_date + INTERVAL'1 week') THEN 2*m.price ELSE m.price END) as points
      FROM sales s
      INNER JOIN members mem ON mem.customer_id = s.customer_id
      INNER JOIN menu m ON s.product_id = m.product_id
      WHERE s.order_date <= '2021-01-31'
      GROUP BY s.customer_id
    </pre>
    </details>

    <br>
## Bonus Questions

### Join All The Things


Recreate the following table output using the available data:

![image](https://github.com/the-soham/the-soham.github.io/assets/60706236/7da40a31-b166-4631-b7bc-f63836545758)

Please try it on youe own before looking up for the solution.

<details>
<summary>Solution</summary>
<pre>
      
    SELECT s.customer_id
	   , s.order_date
	   , m.product_name
	   , m.price
	   , CASE WHEN s.order_date>=mem.join_date THEN 'Y' ELSE 'N' END AS member
    FROM sales s
    LEFT JOIN menu m ON s.product_id = m.product_id
    LEFT JOIN members mem ON mem.customer_id = s.customer_id
    ORDER BY s.customer_id, s.order_date
</pre>
</details>

<br>

### Rank The Things
Recreate the following table output using the available data:

![image](https://github.com/the-soham/the-soham.github.io/assets/60706236/c00add2b-3796-4f3c-b7bd-76ebcca23ad4)

<details>
<summary>Solution</summary>
<pre>
      
  WITH cte AS(
  SELECT s.customer_id
  	   , s.order_date
  	   , m.product_name
  	   , m.price
  	   , CASE WHEN s.order_date>=mem.join_date THEN 'Y' ELSE 'N' END AS member
  FROM sales s
  LEFT JOIN menu m ON s.product_id = m.product_id
  LEFT JOIN members mem ON mem.customer_id = s.customer_id
  ORDER BY s.customer_id, s.order_date
  )
  SELECT customer_id
  	  , order_date
  	  , product_name
  	  , price
  	  , member
  	  ,  CASE WHEN member='N' THEN NULL 
  		 ELSE RANK() OVER(PARTITION BY customer_id, member ORDER BY order_date ) END AS ranking
  FROM cte
</pre>
</details>

<br>

If you liked the above content, connect with me on [LinkedIn](https://www.linkedin.com/in/soham-bhagwat/)
<br>
Credits:
[Danny Ma](https://8weeksqlchallenge.com/case-study-1/)
