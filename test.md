# SQL Case Study #1 - Danny's Diner

Hello data nerds, have you exhausted all the SQL questions on leet code and want to practice some advance concepts based on real world scenario's then you have come to the right place. In this blog, I'm going to walk you through Challenge #1 of [Danny Ma's](https://www.linkedin.com/in/datawithdanny/) comprehensive [SQL 8 week challenge](https://8weeksqlchallenge.com/case-study-1/). Let's start right away without much adieu.



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

##
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

## 

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

## 

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


You may be using [Markdown Live Preview](https://markdownlivepreview.com/).

## Blockquotes

> Markdown is a lightweight markup language with plain-text-formatting syntax, created in 2004 by John Gruber with Aaron Swartz.
>
>> Markdown is often used to format readme files, for writing messages in online discussion forums, and to create rich text using a plain text editor.

## Tables

| Left columns  | Right columns |
| ------------- |:-------------:|
| left foo      | right foo     |
| left bar      | right bar     |
| left baz      | right baz     |

## Blocks of code

```
let message = 'Hello world';
alert(message);
```

## Inline code

This web site is using `markedjs/marked`.
