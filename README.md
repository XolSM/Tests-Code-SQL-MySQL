# Collection of SQL/ MySQL code developed by me while I practice coding skills

Content:
* [# MySQL - Weekly SQL Challenge #27 by Data in Motion LLC on LeetCode](#sql-27)
    - [Question 1](#question1)
    - [Question 2](#question2)
* [# MySQL - Daily Challenges on LeetCode](#mysql-daily)
    - [1873. Calculate Special Bonus](#1873)
    - [196. Delete Duplicate Emails](#196)    
    

<a id="sql-27"></a>
## MySQL - Weekly SQL Challenge #27 by Data in Motion LLC on LeetCode
<a id="question1"></a>
### Question 1]

Given the following Tables:

Table: Users
Column Name     | Type    
--------------- | --------
 user_id        | int     
 join_date      | date    
 favorite_brand | varchar 

user_id is the primary key of this table.
This table has the info of the users of an online shopping website where users can sell and buy items.
 

Table: Orders
 Column Name   | Type    
---------------|---------
 order_id      | int     
 order_date    | date    
 item_id       | int     
 buyer_id      | int     
 seller_id     | int     

order_id is the primary key of this table.
item_id is a foreign key to the Items table.
buyer_id and seller_id are foreign keys to the Users table.
 

Table: Items
 Column Name   | Type    
---------------|---------
 item_id       | int     
 item_brand    | varchar 

item_id is the primary key of this table.
 

Write an SQL query to find for each user, the join date and the number of orders they made as a buyer in 2019.
Return the result table in any order.

#### My answer:
```{}
SELECT 
    Users.user_id as buyer_id,
    Users.join_date,
    Count(order_id) AS orders_in_2019
FROM Users
LEFT JOIN Orders ON Users.user_id = Orders.buyer_id
AND year(order_date) = 2019
GROUP BY Users.user_id
```

#### Results

**Input**
Users =
| user_id | join_date  | favorite_brand |
| ------- | ---------- | -------------- |
| 1       | 2018-01-01 | Lenovo         |
| 2       | 2018-02-09 | Samsung        |
| 3       | 2018-01-19 | LG             |
| 4       | 2018-05-21 | HP             |

Orders =
| order_id | order_date | item_id | buyer_id | seller_id |
| -------- | ---------- | ------- | -------- | --------- |
| 1        | 2019-08-01 | 4       | 1        | 2         |
| 2        | 2018-08-02 | 2       | 1        | 3         |
| 3        | 2019-08-03 | 3       | 2        | 3         |
| 4        | 2018-08-04 | 1       | 4        | 2         |
| 5        | 2018-08-04 | 1       | 3        | 4         |
| 6        | 2019-08-05 | 2       | 2        | 4         |

Items =
| item_id | item_brand |
| ------- | ---------- |
| 1       | Samsung    |
| 2       | Lenovo     |
| 3       | LG         |
| 4       | HP         |

**Output**
| buyer_id | join_date  | orders_in_2019 |
| -------- | ---------- | -------------- |
| 1        | 2018-01-01 | 1              |
| 2        | 2018-02-09 | 2              |
| 3        | 2018-01-19 | 0              |
| 4        | 2018-05-21 | 0              |



<a id="question2"></a>
### Question 2]  

Table: Stocks
 Column Name   | Type    
---------------|---------
 stock_name    | varchar 
 operation     | enum    
 operation_day | int     
 price         | int     

(stock_name, operation_day) is the primary key for this table.
The operation column is an ENUM of type ('Sell', 'Buy')
Each row of this table indicates that the stock which has stock_name had an operation on the day operation_day with the price.
It is guaranteed that each 'Sell' operation for a stock has a corresponding 'Buy' operation in a previous day. It is also guaranteed that each 'Buy' operation for a stock has a corresponding 'Sell' operation in an upcoming day.
 

Write an SQL query to report the Capital gain/loss for each stock.
The Capital gain/loss of a stock is the total gain or loss after buying and selling the stock one or many times.
Return the result table in any order.   

#### My answer:
```{}
SELECT 
    stock_name,
    SUM(CASE WHEN operation = 'Sell' THEN price ELSE price * -1 END) AS capital_gain_loss
FROM 
    Stocks
GROUP BY stock_name;
```

#### Results:
**Input**
Stocks =
| stock_name   | operation | operation_day | price |
| ------------ | --------- | ------------- | ----- |
| Leetcode     | Buy       | 1             | 1000  |
| Corona Masks | Buy       | 2             | 10    |
| Leetcode     | Sell      | 5             | 9000  |
| Handbags     | Buy       | 17            | 30000 |
| Corona Masks | Sell      | 3             | 1010  |
| Corona Masks | Buy       | 4             | 1000  |
| Corona Masks | Sell      | 5             | 500   |
| Corona Masks | Buy       | 6             | 1000  |
| Handbags     | Sell      | 29            | 7000  |
| Corona Masks | Sell      | 10            | 10000 |

**Output**
| stock_name   | capital_gain_loss |
| ------------ | ----------------- |
| Leetcode     | 8000              |
| Corona Masks | 9500              |
| Handbags     | -23000            |


<a id="1873"></a>
### 1873. Calculate Special Bonus](#1873)

Table: Employees

| Column Name | Type    |
|-------------|---------|
| employee_id | int     |
| name        | varchar |
| salary      | int     |

employee_id is the primary key for this table.
Each row of this table indicates the employee ID, employee name, and salary.
 

Write an SQL query to calculate the bonus of each employee. The bonus of an employee is 100% of their salary if the ID of the employee is an odd number and the employee name does not start with the character 'M'. The bonus of an employee is 0 otherwise.
Return the result table ordered by employee_id.

#### My answer:
```{}
SELECT
    employee_id,
    CASE WHEN (employee_id % 2) > 0 AND LEFT (name, 1) <> 'M' THEN salary ELSE 0 END AS bonus
FROM
    Employees
ORDER BY employee_id
```

#### Results:
**Input**
Employees =

| employee_id | name    | salary |
| ----------- | ------- | ------ |
| 2           | Meir    | 3000   |
| 3           | Michael | 3800   |
| 7           | Addilyn | 7400   |
| 8           | Juan    | 6100   |
| 9           | Kannon  | 7700   |

**Output**
| employee_id | bonus |
| ----------- | ----- |
| 2           | 0     |
| 3           | 0     |
| 7           | 7400  |
| 8           | 0     |
| 9           | 7700  |



<a id="196"></a>
### 196. Delete Duplicate Emails

Table: Person
| Column Name | Type    |
|-------------|---------|
| id          | int     |
| email       | varchar |

id is the primary key column for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.
 

Write an SQL query to delete all the duplicate emails, keeping only one unique email with the smallest id. Note that you are supposed to write a DELETE statement and not a SELECT one.

#### My answer:
```{}
DELETE p2 
FROM Person as p2, Person as p1 
WHERE p2.Email = p1.Email AND p2.Id > p1.Id
```

#### Results:
**Input**
Person =
| id | email            |
| -- | ---------------- |
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |

Output
| id | email            |
| -- | ---------------- |
| 1  | john@example.com |
| 2  | bob@example.com  |


### To be continued..😉
