
Yes, there is a reason why the **iterator solution is faster** than the **for loop solution**, even though it seems like the iterator-based approach performs **twice** the number of operations (one forward and one backward). Let's break it down.

---

### **Key Differences Between the Two Approaches**

#### **1️⃣ Iterator & Reverse Iterator Approach**

```cpp
auto iter = resultStr.begin();
auto rIter = resultStr.rbegin();

while (iter != resultStr.end() && rIter != resultStr.rend()) {
    if (*iter != *rIter)
        return false;

    ++iter;
    ++rIter;
}
```

- **Efficient Memory Access:** Both `iter` and `rIter` are iterators that traverse **directly in memory order**.
- **Avoids unnecessary calculations:** `rIter` internally uses pointer arithmetic instead of index-based calculations.
- **No explicit indexing operations (`.at()`).**
- **Fewer bounds checks.** Only one condition is checked: `iter != resultStr.end()`.

#### **2️⃣ For Loop Approach**

```cpp
for (int i = 0; i < resultSize / 2; ++i) {
    if (resultStr.at(i) != resultStr.at(resultSize - 1 - i))
        return false;
}
```

- **Extra Computation in Each Iteration:**
    - `resultStr.at(resultSize - 1 - i)` requires an **extra subtraction operation** (`resultSize - 1 - i`).
- **`.at()` vs Direct Iterator Access:**
    - `.at(i)` **performs an extra bounds check** to ensure `i` is within valid limits.
    - The iterator approach **avoids this overhead** since it directly increments the pointer.
- **Index-Based Memory Access:** Accessing `resultStr.at(resultSize - 1 - i)` forces **non-sequential memory access**, which could slow down execution due to potential **cache misses**.

---

### **Why Is the Iterator-Based Approach Faster?**

1. **Fewer Arithmetic Operations**
    
    - The for-loop approach calculates `resultSize - 1 - i` **in every iteration**.
    - The iterator approach **increments two pointers**, which is **faster than doing an arithmetic operation every iteration**.
2. **Avoids `.at()` Overhead**
    
    - `.at(i)` does **bounds checking** in debug mode, while `*iter` does not.
    - `resultStr.at(resultSize - 1 - i)` is **computed in every iteration**, while the reverse iterator **automatically moves backward**.
3. **Better Cache Efficiency**
    
    - Iterators traverse memory **linearly**, while index-based access requires **jumping around** (which could result in more cache misses).

---

### **Performance Comparison (Theoretical Analysis)**

|Approach|Extra Operations per Iteration|Memory Access Pattern|Cache Efficiency|
|---|---|---|---|
|**Iterator Approach**|2 pointer increments|Linear|Better|
|**For Loop Approach**|1 index calculation + 1 lookup|Non-sequential|Worse|

---

### **Final Conclusion**

Even though the iterator approach **seems to do twice the work** (moving forward and backward), it's actually **more optimized** because:

- It **avoids redundant calculations** (`resultSize - 1 - i` every loop).
- It **does not use `.at()`, avoiding bounds checking**.
- It **traverses memory in a more cache-friendly manner**.

So, the iterator approach is **faster** due to **better memory access patterns and reduced computational overhead**. 🚀

___
# Easy

big countries

```sql
SELECT
    name,
    population,
    area
FROM
    World
WHERE
	area >= 3000000
OR
	population >= 25000000
```

employees earning more than their managers

```sql
SELECT
    e.name as Employee
FROM
    Employee e
JOIN
    Employee em
ON
    e.managerId = em.id
WHERE
    e.salary >= em.salary;
```

duplicate emails

```sql
SELECT
   email as Email
FROM
    Person
GROUP BY
    email
HAVING
    COUNT(id) > 1;
```

customers who never order

```sql
SELECT
    Customers.name as Customers
FROM
    Customers
LEFT JOIN
    Orders
ON
    Customers.id = Orders.customerId
WHERE
    Orders.id IS NULL;
```

Combine two tables

```sql
SELECT
    firstName,
    lastName,
    city,
    state
FROM
    Person
LEFT JOIN
    Address
ON
    Person.personId = Address.personId
```

Delete duplicate emails

```sql
DELETE FROM  
    Person  
WHERE  
    id NOT IN  
		(  
		    SELECT  
		        (  
		            SELECT  
		                p2.id  
		            FROM  
		                Person p2  
		            WHERE  
		                p2.email = p1.email  
		            ORDER BY  
		                p2.id  
		            LIMIT  
		                1  
		        ) AS smallest_id  
		    FROM  
		        Person p1  
		    GROUP BY  
		        p1.email  
		    HAVING  
		        COUNT(p1.id) > 1  
		)  
  AND email NOT IN
	(  
	    SELECT  
	        p3.email  
	    FROM  
	        Person p3  
	    GROUP BY  
	        p3.email  
	    HAVING  
	        COUNT(p3.id) = 1  
	);
```

Rising temperature

```sql
SELECT
    w.id
FROM
    Weather w
WHERE
    temperature >
    (
        SELECT
            temperature
        FROM
            Weather w1
        WHERE
            w1.recordDate = DATE_SUB(w.recordDate, INTERVAL 1 DAY)
    );
```

Game play analysis

```sql
SELECT
    player_id,
    MIN(event_date) as first_login
FROM
    Activity
GROUP BY
    player_id;
```

Employee Bonus 

```sql
SELECT
    Employee.name,
    bonus
FROM
    Employee
LEFT JOIN
    Bonus
ON
    Employee.empId = Bonus.empId
WHERE
    bonus < 1000 
OR
    bonus IS NULL;
```

Find customer referee

```sql
SELECT
    name
FROM
    Customer
WHERE
    referee_id IS NULL 
OR
    referee_id != 2;
```

customer-placing-the-largest-number-of-orders

```sql
SELECT
    customer_number
FROM
    Orders o
GROUP BY
    customer_number
ORDER BY  
    (
        SELECT
            COUNT(*)
        FROM
            Orders o1
        WHERE
            o1.customer_number = o.customer_number
    ) DESC
LIMIT 1;
```

classes with more than 5 students

```sql
SELECT
    cass
FROM
    Courses
GROUP BY    
    class
HAVING
    COUNT(student) >= 5;
```

biggest single number

```sql
SELECT
    MAX(num) as num
FROM
    MyNumbers mn
WHERE
    (
        SELECT
            COUNT(*)
        FROM
            MyNumbers mn1
        WHERE
            mn1.num = mn.num
    ) = 1;
```

not boring movies

```sql
SELECT
    *
FROM
    Cinema
WHERE
    id & 1
AND
    description != 'boring'
ORDER BY
    rating DESC;
```


actors and directors who cooperated at least three times

```sql
SELECT
    actor_id,
    director_id
FROM
    ActorDirector ad
WHERE
    (
        SELECT
            COUNT(ad1.timestamp)
        FROM
            ActorDirector ad1
        WHERE
            ad1.actor_id = ad.actor_id
        AND
            ad1.director_id = ad.director_id
    ) >= 3
GROUP BY
    (actor_id, director_id);
```

product sales alanysis I

```sql
SELECT
    product_name,
    year,
    price
FROM
    Sales
JOIN
    Product
-- ON
--     Sales.product_id = Product.product_id
USING
    (product_id);
```

Project employees I

```sql
SELECT
    project_id,
    (
        SELECT
            ROUND(AVG(e1.experience_years), 2)
        FROM
            Project p1
        JOIN
            Employee e1
        -- ON
        --     p1.employee_id = e1.employee_id
        USING
            (employee_id)
        WHERE
            p1.project_id = p.project_id
    ) AS average_years
FROM
    Project p
GROUP BY
    project_id
ORDER BY
    project_id ASC;
```

article views I

```sql
SELECT
    author_id AS id
FROM
    Views
WHERE
    author_id = viewer_id
GROUP BY    
    author_id
ORDER BY
    author_id ASC;
```

recyclable and low fat products

```sql
SELECT
    product_id
FROM
    Products
WHERE
    low_fats = 'Y'
AND
    recyclable = 'Y';
```

invalid tweets

```sql
SELECT
    tweet_id
FROM
    Tweets
WHERE
    LENGTH(content) > 15;
```

replace employee id with the unique identifier 

```sql
SELECT
    unique_id,
    name
FROM
    Employees
LEFT JOIN
    EmployeeUNI
ON
    Employees.id = EmployeeUNI.id;
```

customer who visited but did not make any transactions

```sql
SELECT
    v.customer_id,
    (
        SELECT
            count(v1.customer_id)
        FROM
            Visits v1
        LEFT JOIN
            Transactions t1
        ON  
            v1.visit_id = t1.visit_id
        WHERE
            v1.customer_id = v.customer_id
        AND
            t1.visit_id IS NULL
    ) AS count_no_trans
FROM
    Visits v
LEFT JOIN
    Transactions
ON
    v.visit_id = Transactions.visit_id
WHERE
    amount IS NULL
GROUP BY
    customer_id;
```

average selling price 

```sql
SELECT
    Prices.product_id,
    CASE
        WHEN SUM(units) > 0 THEN ROUND(SUM(units * price) / SUM(units)::NUMERIC, 2)
        ELSE 0
    END AS average_price
FROM
    Prices
LEFT JOIN
    UnitsSold
ON
    UnitsSold.product_id = Prices.product_id
WHERE
    UnitsSold.purchase_date >= Prices.start_date
AND
    UnitsSold.purchase_date <= Prices.end_date
OR
    UnitsSold.purchase_date IS NULL
GROUP BY
    Prices.product_id
ORDER BY
    Prices.product_id;
```

percentage of users attended a contest

```sql
SELECT
    Register.contest_id,
    ROUND(
            COUNT(Register.user_id)
        /
            ((SELECT COUNT(u1) FROM Users u1)::NUMERIC / 100)
        , 2) AS "percentage"
FROM
    Register
JOIN
    Users
ON
    Register.user_id = Users.user_id
GROUP BY
    Register.contest_id
ORDER BY
    "percentage" DESC,
    Register.contest_id ASC;
```

queries quality and percentage

```sql
SELECT
    q.query_name,
    ROUND(AVG(rating / position::NUMERIC), 2) as quality,
    ROUND((
        SELECT COUNT(q1.query_name) FROM Queries q1 WHERE q1.query_name = q.query_name AND rating < 3)
    /
        ((SELECT COUNT(q1.query_name) FROM Queries q1 WHERE q1.query_name = q.query_name)::Numeric / 100)
	, 2) AS poor_query_percentage
FROM
    Queries q
GROUP BY
    q.query_name;
```

number of unique subjects taught by each teacher

```sql
SELECT
    teacher_id,
    COUNT(cnt) as cnt
FROM
    (
        SELECT
            teacher_id,
            COUNT(dept_id) as cnt
        FROM    
            Teacher
        GROUP BY    
            (teacher_id, subject_id)
    )
GROUP BY
    teacher_id;
```

find followers count

```sql
SELECT
    user_id,
    COUNT(follower_id) AS followers_count
FROM
    Followers
GROUP BY
    user_id
ORDER BY
    user_id;
```

the number of employees which report to each employee

```sql
SELECT
    e.employee_id,
    e.name,
    (
        SELECT
            COUNT(*)
        FROM
            Employees e2
        WHERE
            e2.reports_to = e.employee_id
    ) AS reports_count,
	(
        SELECT
            ROUND(AVG(e3.age))
        FROM
            Employees e3
        WHERE
            e3.reports_to = e.employee_id
    ) AS average_age
FROM
    Employees e
WHERE
    e.employee_id
IN
    (
        SELECT
            e1.reports_to
        FROM
            Employees e1
        GROUP BY
            e1.reports_to
    )
ORDER BY
    e.employee_id;
```

primary department for each employee

```sql
SELECT
    employee_id,
    department_id
FROM
    Employee
WHERE
    primary_flag = 'Y'
OR
    employee_id
NOT IN
    (
        SELECT
            employee_id
        FROM
            Employee
        WHERE
            primary_flag = 'Y'
    );
```

employees whose manager left the company

```sql
SELECT
    e1.employee_id
FROM
    Employees e1
LEFT JOIN
    Employees e2
ON
    e1.manager_id = e2.employee_id
WHERE
    e1.salary < 30000
AND
    e1.manager_id IS NOT NULL
AND
    e2.employee_id IS NULL
ORDER BY
    e1.employee_id;
```

fix names in a table

```sql
SELECT
    user_id,
    CONCAT(
        UPPER(SUBSTRING(name, 1, 1)),
        Lower(SUBSTRING(name, 2, LENGTH(name)))
    ) AS name
FROM  
    Users
ORDER BY
    user_id;
```

Group sold products by the date

```sql
SELECT
    a.sell_date
    , COUNT(DISTINCT a.product) as num_sold
    , (
        SELECT
            STRING_AGG(DISTINCT a1.product, ',' ORDER BY a1.product)
        FROM
            Activities a1
        WHERE
            a1.sell_date = a.sell_date
    )  AS products
FROM
    Activities a
GROUP BY
    a.sell_date
ORDER BY
    a.sell_date;
```

list the products ordered in a period

```sql
SELECT
    product_name,
    SUM(unit) as unit
FROM
    Orders
JOIN
    Products
ON
    Orders.product_id = Products.product_id
WHERE
    order_date >= '2020-02-01'
AND
    order_date <= '2020-02-29'
GROUP BY
    product_name
HAVING
    SUM(unit) >= 100;
```

average time of process per machine

```sql
SELECT
    a1.machine_id,
    ROUND(AVG(a2.timestamp - a1.timestamp), 3) AS processing_time
FROM
    Activity a1
JOIN
    Activity a2
ON
    a1.process_id = a2.process_id
WHERE
    a1.machine_id = a2.machine_id
AND
    a1.timestamp < a2.timestamp
AND
    a1.activity_type = 'start'
AND
    a2.activity_type = 'end'
GROUP BY
    a1.machine_id;
```

user activity for the past 30 days

```sql
SELECT
    activity_date AS day,
    COUNT(DISTINCT user_id) AS active_users
FROM
    Activity
WHERE
    activity_date >= '2019-06-28'
AND
    activity_date <= '2019-07-27'
GROUP BY
    activity_date
ORDER BY
    activity_date;
```



```sql
(
SELECT  
    sMain.student_id,
    sMain.student_name,
    Subjects.subject_name,
    COUNT(Examinations.subject_name) AS attended_exams
FROM Students sMain
CROSS JOIN Subjects
LEFT JOIN Examinations
    ON Examinations.subject_name = Subjects.subject_name
    AND Examinations.student_id = sMain.student_id
GROUP BY
    sMain.student_id,
    sMain.student_name,
    Subjects.subject_name
ORDER BY
    sMain.student_id,
    Subjects.subject_name;
)

-- Query for one student with hardcoded id and name!
SELECT
    1 AS student_id,
    'Alice' AS student_name,
    Subjects.subject_name,
    COUNT(Subjects.subject_name) AS attended_exams
FROM
    Subjects
LEFT JOIN
    Examinations
ON
    Examinations.subject_name = Subjects.subject_name
AND
    Examinations.student_id = 1
LEFT JOIN
    Students
ON
    Examinations.student_id = Students.student_id
GROUP BY
    Subjects.subject_name
ORDER BY
    Subjects.subject_name;
```

Top travellers

```sql
SELECT
    name,
    CASE
    WHEN SUM(distance) IS NOT NULL
        THEN SUM(distance)
        ELSE 0
    END AS travelled_distance
FROM
    Users
LEFT JOIN
    Rides
ON
    Users.id = Rides.user_id
GROUP BY
    Users.name, Users.id
ORDER BY
    travelled_distance DESC,
    name;
```

bank account summary

```sql
SELECT
    Users.name,
    SUM(amount) AS balance
FROM
    Users
JOIN
    Transactions
ON  
    Users.account = Transactions.account
GROUP BY
    Users.name
HAVING
    SUM(amount) > 10000;
```

daily leads and partners

```sql
SELECT
    ds1.date_id,
    ds1.make_name,
    (
        SELECT
            COUNT(DISTINCT ds2.lead_id)
        FROM
            DailySales ds2
        WHERE
            ds2.date_id = ds1.date_id
        AND
            ds2.make_name = ds1.make_name
    ) AS unique_leads,
    (
        SELECT
            COUNT(DISTINCT ds3.partner_id)
        FROM
            DailySales ds3
        WHERE
            ds3.date_id = ds1.date_id
        AND
            ds3.make_name = ds1.make_name
    ) AS unique_partners
FROM
    DailySales ds1
GROUP BY    
    ds1.date_id, ds1.make_name
ORDER BY
    ds1.date_id, ds1.make_name;
```

find total time spent by each employee

```sql
SELECT
    e1.event_day AS day,
    e1.emp_id,
    (
        SELECT
            SUM(e2.out_time - e2.in_time) AS total_time
        FROM
            Employees e2
        WHERE
            e2.emp_id = e1.emp_id
        AND
            e2.event_day = e1.event_day
    ) AS total_time
FROM
    Employees e1
GROUP BY
    e1.event_day, e1.emp_id
ORDER BY
    e1.event_day, e1.emp_id;
```

calculate special bonus

```sql
SELECT
    employee_id,
    CASE
        WHEN employee_id % 2 != 0 AND name NOT LIKE 'M%'
            THEN salary
        ELSE
            0
    END AS bonus
FROM
    Employees
ORDER BY
    employee_id
```

employees with missing information

```sql
SELECT
    CASE
        WHEN Employees.employee_id IS NULL THEN Salaries.employee_id
        WHEN Salaries.employee_id IS NULL THEN Employees.employee_id
    END AS employee_id
FROM
    Employees
FULL JOIN
    Salaries
ON
    Employees.employee_id = Salaries.employee_id
WHERE
    name IS NULL
OR
    salary IS NULL;
```

sales person

```sql
SELECT
    sp.name
FROM
    SalesPerson sp
WHERE
    sp.sales_id
NOT IN
    (
        SELECT
            Orders.sales_id
        FROM
            Orders
        JOIN
            SalesPerson
        ON
            Orders.sales_id = SalesPerson.sales_id
        JOIN
            Company
        ON
            Orders.com_id = Company.com_id
        WHERE
            Company.name = 'RED'
    );
```

swap salary

```sql
UPDATE
    Salary
SET
    sex =
        CASE
            WHEN sex = 'm' THEN 'f'
            WHEN sex = 'f' THEN 'm'
        END;
```

sales analysis III

```sql
SELECT
    Sales.product_id,
    Product.product_name
FROM
    Sales
JOIN
    Product
ON
    Product.product_id = Sales.product_id
WHERE
    Sales.product_id
NOT IN
    (
        SELECT
            product_id
        FROM
            Sales
        WHERE
            sale_date < '2019-01-01'
        OR
            sale_date > '2019-03-31'
    )
GROUP BY
    Sales.product_id,
    Product.product_name;
```

the latest login in 2020

```sql
SELECT
    l1.user_id,
    (
        SELECT
            MAX(time_stamp)
        FROM
            Logins l3
        WHERE
            l3.user_id = l1.user_id
        AND
            EXTRACT(YEAR FROM time_stamp) = 2020
    ) AS last_stamp
FROM
    Logins l1
WHERE
    2020 IN
    (
        SELECT
            EXTRACT(YEAR FROM l2.time_stamp)
        FROM
            Logins l2
        WHERE
            l2.user_id = l1.user_id
    )
GROUP BY
    l1.user_id;
```

Triangle Judgement

```sql
SELECT
    x,
    y,
    z,
    CASE
        WHEN x + y > z AND x + z > y AND y + z > x
        THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM
    Triangle;
```

Find users with valid emails

```sql
SELECT
    *
FROM
    Users
WHERE
    REGEXP_LIKE(mail, '^[a-zA-Z]{1,}[a-zA-Z0-9_.-]{0,}@leetcode\.com$');
```

Reformat department table

```sql
SELECT
    DISTINCT id,
    (
        SELECT
            dJan.revenue
        FROM
            Department dJan
        WHERE
            dJan.id = dMain.id
        AND
            dJan.month = 'Jan'
    ) AS "Jan_Revenue",
    (
        SELECT
            dFeb.revenue
        FROM
            Department dFeb
        WHERE
            dFeb.id = dMain.id
        AND
            dFeb.month= 'Feb'
    ) AS "Feb_Revenue",
    (
        SELECT
            dMar.revenue
        FROM
            Department dMar
        WHERE
            dMar.id = dMain.id
        AND
            dMar.month = 'Mar'
    ) AS "Mar_Revenue",
    (
        SELECT
            dApr.revenue
        FROM
            Department dApr
        WHERE
            dApr.id = dMain.id
        AND
            dApr.month = 'Apr'
    ) AS "Apr_Revenue",
    (
        SELECT
            dMay.revenue
        FROM
            Department dMay
        WHERE
            dMay.id = dMain.id
        AND
            dMay.month = 'May'
    ) AS "May_Revenue",
    (
        SELECT
            dJun.revenue
        FROM
            Department dJun
        WHERE
            dJun.id = dMain.id
        AND
            dJun.month = 'Jun'
    ) AS "Jun_Revenue",
    (
        SELECT
            dJul.revenue
        FROM
            Department dJul
        WHERE
            dJul.id = dMain.id
        AND
            dJul.month = 'Jul'
    ) AS "Jul_Revenue",
    (
        SELECT
            dAug.revenue
        FROM
            Department dAug
        WHERE
            dAug.id = dMain.id
        AND
            dAug.month = 'Aug'
    ) AS "Aug_Revenue",
    (
        SELECT
            dSep.revenue
        FROM
            Department dSep
        WHERE
            dSep.id = dMain.id
        AND
            dSep.month = 'Sep'
    ) AS "Sep_Revenue",
    (
        SELECT
            dOct.revenue
        FROM
            Department dOct
        WHERE
            dOct.id = dMain.id
        AND
            dOct.month = 'Oct'
    ) AS "Oct_Revenue",
    (
        SELECT
            dNov.revenue
        FROM
            Department dNov
        WHERE
            dNov.id = dMain.id
        AND
            dNov.month = 'Nov'
    ) AS "Nov_Revenue",
    (
        SELECT
            dDec.revenue
        FROM
            Department dDec
        WHERE
            dDec.id = dMain.id
        AND
            dDec.month = 'Dec'
    ) AS "Dec_Revenue"
FROM
    Department dMain;
```

rearrange products table

```sql
SELECT
    DISTINCT product_id,
    'store1' AS store,
    (
        SELECT
            p1.store1
        FROM
            Products p1
        WHERE
            p1.product_id = pMain.product_id
    ) AS price
FROM
    Products pMain
WHERE
    (
        SELECT
            p1.store1
        FROM
            Products p1
        WHERE
            p1.product_id = pMain.product_id
    ) IS NOT NULL
UNION ALL -- Unite with the store2 results
SELECT
    DISTINCT product_id,
    'store2' AS store,
    (
        SELECT
            p2.store2
        FROM
            Products p2
        WHERE
            p2.product_id = pMain.product_id
    ) AS price
FROM
    Products pMain
WHERE

    (
        SELECT
            p2.store2
        FROM
            Products p2
        WHERE
            p2.product_id = pMain.product_id
    ) IS NOT NULL
UNION ALL -- Unite with the store3 results
SELECT
    DISTINCT product_id,
    'store3' AS store,
    (
        SELECT
            p3.store3
        FROM
            Products p3
        WHERE
            p3.product_id = pMain.product_id
    ) AS price
FROM
    Products pMain
WHERE
    (
        SELECT
            p3.store3
        FROM
            Products p3
        WHERE
            p3.product_id = pMain.product_id
    ) IS NOT NULL;
```

## Regex Tasks

https://leetcode.com/problems/patients-with-a-condition/submissions/1562413784/
https://leetcode.com/problems/find-products-with-valid-serial-numbers/description/
https://leetcode.com/problems/find-valid-emails/description/

___
# Medium

managers with at least 5 direct reports

```sql
SELECT
    name
FROM
    Employee eMain
WHERE
    id
IN
    (
        SELECT
            managerId
        FROM
            Employee
        GROUP BY
            managerId
        HAVING
            COUNT(id) >= 5
    );
```


```sql
SELECT
    s.user_id,
    COALESCE(
        (
            SELECT
                ROUND(COALESCE(COALESCE(
                    SUM(CASE WHEN action = 'confirmed' THEN 1 ELSE 0 END)::NUMERIC
                /
                    NULLIF(COUNT(c.user_id) / 100::NUMERIC, 0), 0) / 100
                , 0)::NUMERIC, 2)
            FROM
                Confirmations c
            WHERE
                c.user_id = s.user_id
            GROUP BY
                c.user_id
        ), 0
    ) AS confirmation_rate
FROM
    Signups s
GROUP BY
    s.user_id;
```

monthly transactions I

```sql
SELECT
    CONCAT(
        EXTRACT('year' FROM DATE_TRUNC('month', t2.trans_date::DATE)::DATE),
        '-',
       LPAD(EXTRACT('month' FROM DATE_TRUNC('month', t2.trans_date::DATE)::DATE)::TEXT, 2, '0')
    ) AS month,
    t1.country,
    COUNT(t2.id) AS trans_count,
    SUM(
        CASE
            WHEN t2.state = 'approved' THEN 1
            ELSE 0
        END
    ) AS approved_count,
    SUM(t2.amount) AS trans_total_amount,
    SUM(
        CASE
            WHEN t2.state = 'approved' THEN t2.amount
            ELSE 0
        END
    ) AS approved_total_amount
FROM
    Transactions t1
JOIN
    Transactions t2
ON
    t1.id = t2.id
GROUP BY
    DATE_TRUNC('month', t2.trans_date::DATE)::DATE,
    t1.country;
```

immediate food delivery II

```sql
SELECT
    ROUND(
        COALESCE(
            SUM(
	            CASE 
		            WHEN "order_date" = "customer_pref_delivery_date" THEN 1 
		            ELSE 0 
				END)::NUMERIC
        /
            COALESCE(COUNT(customer_id)::NUMERIC / 100, 0)
        , 0)
    , 2) AS immediate_percentage
FROM
    (
        SELECT  
            DISTINCT customer_id,
            (
                SELECT
                    d1.order_date
                FROM
                    Delivery d1
                WHERE
                    d1.customer_id = d.customer_id
                ORDER BY
                    d1.order_date ASC
                LIMIT 1
            ) AS "order_date",
            (
                SELECT
                    d1.customer_pref_delivery_date
                FROM
                    Delivery d1
                WHERE
                    d1.customer_id = d.customer_id
                ORDER BY
                    d1.order_date ASC
                LIMIT 1
            ) AS "customer_pref_delivery_date"
        FROM
            Delivery d
    )
```

Game play analysis IV

```sql
SELECT
    ROUND(COALESCE(SUM(logged_the_next_day)::NUMERIC / COUNT(player_id)::NUMERIC, 0), 2) AS fraction
    FROM
        (
            SELECT
                aMain.player_id,
                (
                    SELECT
                        COUNT(a1.player_id)
                    FROM
                        Activity a1
                    WHERE
                        a1.player_id = aMain.player_id
                    AND
                        (a1.event_date =
                        (
                            SELECT
                                MIN(a2.event_date)
                            FROM
                                Activity a2
                            WHERE
                                a2.player_id = aMain.player_id
                        ) + INTERVAL '1 day')
                ) AS logged_the_next_day
            FROM
                Activity aMain
            GROUP BY
                aMain.player_id
        )
```

customers who bought all products

```sql
SELECT
    cMain.customer_id
FROM
    Customer cMain
WHERE
    (
        SELECT
            COUNT(DISTINCT c1.product_key)
        FROM
            Customer c1
        WHERE
            c1.customer_id = cMain.customer_id
    ) =
    (
        SELECT
            COUNT(DISTINCT product_key)
        FROM
            Product
    )
GROUP BY
    cMain.customer_id;
```

restaurant growth

```sql
SELECT
    Customer.visited_on,
    (
        SELECT
            SUM(c1.amount) AS amount
        FROM
            Customer c1
        WHERE
            c1.visited_on >= (Customer.visited_on - INTERVAL '6 days')
        AND
            c1.visited_on <= Customer.visited_on
    ) AS amount,
    (
        SELECT
            ROUND(COALESCE(SUM(c1.amount)::NUMERIC / 7, 0), 2) AS average_amount
        FROM
            Customer c1
        WHERE
            c1.visited_on >= (Customer.visited_on - INTERVAL '6 days')
        AND
            c1.visited_on <= Customer.visited_on
    ) AS average_amount
FROM
    Customer
WHERE
    visited_on - INTERVAL '6 days'
IN
    (
        SELECT
            c1.visited_on
        FROM
            Customer c1
    )
GROUP BY
    visited_on
ORDER BY
    visited_on ASC;
```

Friend Requests II

```sql
SELECT
    id,
    (
        SELECT
            COUNT(*)
        FROM
            RequestAccepted ra1
        WHERE
            ra1.accepter_id = id
    ) +
    (
        SELECT
            COUNT(*)
        FROM
            RequestAccepted ra2
        WHERE
            ra2.requester_id = id
    ) AS num
FROM
    (
        (
            SELECT
                DISTINCT requester_id AS id
            FROM
                RequestAccepted
        )
        UNION
        (
            SELECT
                DISTINCT accepter_id AS id
            FROM
                RequestAccepted
        )
    )
ORDER BY
    num DESC
LIMIT 1;
```

product price at a given date

```sql
SELECT
    DISTINCT pMain.product_id,
    COALESCE((
        SELECT
            p1.new_price
        FROM
            Products p1
        WHERE
            p1.change_date <= '2019-08-16'
        AND
            p1.product_id = pMain.product_id
        ORDER BY
            p1.change_date DESC
        LIMIT 1
    ), 10) AS price
FROM
    Products pMain;
```

Count Salary Categories

```sql
(
    SELECT
        CONCAT('Low', ' ', 'Salary') AS category,
        COUNT(*) AS accounts_count
    FROM
        Accounts
    WHERE
        income < 20000
)
UNION
(
    SELECT
        CONCAT('Average', ' ', 'Salary') AS category,
        COUNT(*) AS accounts_count
    FROM
        Accounts
    WHERE
        income >= 20000
    AND
        income <= 50000
)
UNION
(
    SELECT
        CONCAT('High', ' ', 'Salary') AS category,
        COUNT(*) AS accounts_count
    FROM
        Accounts
    WHERE
        income > 50000
);
```

product sales analysis III

```sql
SELECT
    sMain.product_id,
    sMain.year AS first_year,
    sMain.quantity,
    sMain.price
FROM
    Sales sMain
JOIN
    (
        SELECT
            sale_id,
            product_id
        FROM
            Sales sSub
        WHERE
            year =
                (
                    SELECT
                        MIN(year)
                    FROM
                        Sales
                    WHERE
                        product_id = sSub.product_id
                )
    ) subq
ON
    sMain.sale_id = subq.sale_id;
```

Second Highest Salary

```sql
SELECT
    COALESCE(
        (
            SELECT
                DISTINCT salary AS "SecondHighestSalary"
            FROM
                Employee
            WHERE
                salary NOT IN
                    (
                        SELECT
                            MAX(e1.salary)
                        FROM
                            Employee e1
                    )
            ORDER BY
                salary DESC
            LIMIT 1
        ), NULL
    ) AS "SecondHighestSalary";
```
Investments in 2016

```sql
SELECT
    ROUND(SUM(tiv_2016::NUMERIC), 2) AS tiv_2016
FROM
    Insurance iMain
WHERE
    (
        SELECT
            COUNT(*)
        FROM
            Insurance i1
        WHERE
            i1.tiv_2015 = iMain.tiv_2015
        AND
            i1.pid != iMain.pid
    ) >= 1
AND
    (
        SELECT
            COUNT(*)
        FROM
            Insurance i2
        WHERE
            i2.lat = iMain.lat
        AND
            i2.lon = iMain.lon
        AND
            i2.pid != iMain.pid
    ) = 0;
```

Consecutive Numbers

```sql
SELECT
    lMain.num AS "ConsecutiveNums"
FROM
    Logs lMain
WHERE
    lMain.num =
    (
        SELECT
            l1.num
        FROM
            Logs l1
        WHERE
            l1.id = (lMain.id + 1)
    )
AND
    lMain.num =
    (
        SELECT
            l1.num
        FROM
            Logs l1
        WHERE
            l1.id = (lMain.id + 2)
    )
GROUP BY
    lMain.num;
```

Patients with a condition

```sql
SELECT
    patient_id,
    patient_name,
    conditions
FROM Patients
WHERE
    conditions LIKE 'DIAB1%'
OR
    conditions LIKE '% DIAB1%';
```

Find products with Valid Serial Numbers

```sql
SELECT
    *
FROM
    products
WHERE
    description LIKE '% SN____-____ %'
OR
    description LIKE 'SN____-____ %'
OR
    description LIKE '%SN____-____'
ORDER BY
    product_id ASC;
```

rank scores

```sql
SELECT
    score,
    (
        SELECT
            subq.row_num
        FROM
            (
                SELECT
                    ROW_NUMBER() OVER (ORDER BY s1.score DESC) AS row_num,
                    s1.score
                FROM
                    Scores s1
                GROUP BY
                    s1.score
                ORDER BY
                    s1.score DESC
            ) subq
        WHERE
            subq.score = Scores.score
    ) AS rank
FROM
    Scores
ORDER BY
    score DESC;
```

department highest salary

```sql
SELECT
    Department.name AS "Department",
    Employee.name AS "Employee",
    Employee.salary AS "Salary"
FROM
    Employee
JOIN
    Department
ON
    Employee.departmentId = Department.id
WHERE
    salary =
    (
        SELECT
            MAX(e1.salary)
        FROM
            Employee e1
        JOIN    
            Department d1
        ON
            e1.departmentId = d1.id
        WHERE
            e1.departmentId = Employee.departmentId
    );
```

market analysis I

```sql
SELECT
    DISTINCT user_id AS buyer_id,
    join_date,
    (
        SELECT
            COUNT(*)
        FROM
            Users u1
        JOIN
            Orders o1
        ON
            u1.user_id = o1.buyer_id
        AND
            EXTRACT(year FROM o1.order_date) = 2019
        AND
            u1.user_id = Users.user_id
    ) AS orders_in_2019
FROM
    Users
LEFT JOIN
    Orders
ON
    Users.user_id = Orders.buyer_id;
```

Capital Gain/Loss

```sql
SELECT
    DISTINCT stock_name,
    (
        (
            SELECT
                SUM(s1.price)
            FROM
                Stocks s1
            WHERE
                s1.stock_name = Stocks.stock_name
            AND
                s1.operation = 'Sell'
        )
        -
        (
            SELECT
                SUM(s2.price)
            FROM
                Stocks s2
            WHERE
                s2.stock_name = Stocks.stock_name
            AND
                s2.operation = 'Buy'
        )
    ) AS capital_gain_loss
FROM
    Stocks;
```

odd and even transactions

```sql
SELECT
    DISTINCT tMain.transaction_date,
    (
        SELECT
            SUM(
                CASE
                    WHEN amount % 2 != 0 THEN amount
                    ELSE 0
                END
            ) AS odd_sum
        FROM
            transactions
        WHERE
            transaction_date = tMain.transaction_date
    ) AS odd_sum,
    (
        SELECT
            SUM(
                CASE
                    WHEN amount % 2 = 0 THEN amount
                    ELSE 0
                END
            ) AS even_sum
        FROM
            transactions
        WHERE
            transaction_date = tmain.transaction_date
    ) AS even_sum
FROM
    transactions tMain
ORDER BY
    tmain.transaction_date ASC;
```

find students who improved

```sql
SELECT
    student_id,
    subject,
    (
        SELECT
            score
        FROM
            Scores s1
        WHERE
            s1.student_id = sMain.student_id
        AND
            s1.subject = sMain.subject
        AND
            s1.exam_date =
            (
                SELECT
                    MIN(s2.exam_date)
                FROM
                    Scores s2
                WHERE
                    s2.student_id = sMain.student_id
                AND
                    s2.subject = sMain.subject
            )
    ) AS first_score,
    (
        SELECT
            score
        FROM
            Scores s1
        WHERE
            s1.student_id = sMain.student_id
        AND
            s1.subject = sMain.subject
        AND
            s1.exam_date =
            (
                SELECT
                    MAX(s2.exam_date)
                FROM
                    Scores s2
                WHERE
                    s2.student_id = sMain.student_id
                AND
                    s2.subject = sMain.subject
            )
    ) AS latest_score
FROM    
    Scores sMain
WHERE
    (
        SELECT
            COUNT(DISTINCT exam_date) > 1
        FROM
            Scores s1
        WHERE
            s1.student_id = sMain.student_id
        AND
            s1.subject = s1.subject
    )
AND
    (
        SELECT
            score
        FROM
            Scores s1
        WHERE
            s1.student_id = sMain.student_id
        AND
            s1.subject = sMain.subject
        AND
            s1.exam_date =
            (
                SELECT
                    MAX(s2.exam_date)
                FROM
                    Scores s2
                WHERE
                    s2.student_id = sMain.student_id
                AND
                    s2.subject = sMain.subject
            )
    ) >
    (
        SELECT
            score
        FROM
            Scores s1
        WHERE
            s1.student_id = sMain.student_id
        AND
            s1.subject = sMain.subject
        AND
            s1.exam_date =
            (
                SELECT
                    MIN(s2.exam_date)
                FROM
                    Scores s2
                WHERE
                    s2.student_id = sMain.student_id
                AND
                    s2.subject = sMain.subject
            )
    )
GROUP BY
    student_id,
    subject;
```


movie rating

```sql
(
    SELECT
        Users.name AS results
    FROM
        Users
    JOIN
        MovieRating
    ON
        Users.user_id = MovieRating.user_id
    JOIN
        Movies
    ON
        MovieRating.movie_id = Movies.movie_id
    GROUP BY
        users.name
    ORDER BY
        COUNT(Movies.movie_id) DESC, users.name
    LIMIT 1
)
UNION ALL
(  
    SELECT
        Movies.title AS results
    FROM
        MovieRating
    JOIN
        Movies
    ON
        MovieRating.movie_id = Movies.movie_id
    WHERE
        created_at >= '2020-02-01'
    AND
        created_at < '2020-03-01'
    GROUP BY
        Movies.title
    ORDER BY
        AVG(rating) DESC, Movies.title
    LIMIT 1
);
```