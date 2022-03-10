# SQL常用命令


Mosh/SQL
<!--more-->

```sql
-- REGEXP 正则表达式
REGEXP '[gim]p'
-- 组合 gp, ip, mp
REGEXP '^start'
-- 匹配以start开始的
REGEXP 'end$'
-- 匹配以end结束的
REGEXP 'a|b|c'
-- 匹配包含有a,或b,或c的

SELECT *
FROM sql_test
WHERE name REGEX 'sam$'
ORDER BY date
LIMIT 10
-- 以sam结尾的名字,前10个人(默认升序排列,降序使用DESC关键字)

LIMIT 6, 3
-- 跳过前6个记录,并选择3个记录返回(7,8,9条记录)

-- JOIN 操作
SELECT order_id, O.custom_id, first_name, last_name 
FROM order O
JOIN custom C 
	ON O.custom_id = C.custom_id
	
-- order O 表示O为order的缩写

--自身表JOIN查询
SELECT 
	e.employee_id,
	e.first_name,
	m.first_name AS manager
FROM employee e
JOIN employee m
	ON e.reports_to = m.employee_id
	
--合并三个表
SELECT *
FROM order o
JOIN customs c
	ON o.custom_id = c.custom_id
JOIN order_status os
	ON o.status = os.order_status_id
		
--OUTER JOIN
-- LEFT JOIN / RIGHT JOIN
SELECT 
	c.custom_id,
	c.first_name,
	o.order_id
FROM custom c
LEFT JOIN orders o
	ON c.custon_id = o.custon_id
ORDER BY c.custon_id
	
-- 其中 c.custom_id = o.custom_id
-- 可以用 USING(custom_id) 代替
SELECT *
FROM order_items oi
JOIN order_item_notes oin
	ON oi.order_id = oin.order_id AND
		oi.product_id = oin.product_id
			
-- 使用USING关键字
SELECT *
FROM order_items oi
JOIN order_item_notes oin
	USING(order_id, product_id)
	
-- CROSS JOIN笛卡尔积
SELECT 
	c.first_name AS custom
	p.name AS product
FROM custom c, orders o
ORDER BY c.first_name
	
-- CROSS JOIN 另一种写法
	SELECT 
	c.first_name AS custom
	p.name AS product
FROM custom c
CROSS JOIN products p
ORDER BY c.first_name
	
-- UNION 关键字,组合不同的结果
-- 结合的结果中,列的数量应该相同对应,否则会报错
SELECT first_name
FROM customers
UNION
SELECT name
FROM shippers
	
-- INSERT操作
INSERT INTO custom
VALUES(DEFAULT, 'John', 'Smith')
--双引号和引号都可以
	
-- 内置函数
LAST_INSERT_ID()
-- 最后插入的数据的id
	
INSERT INTO orders(custom_id, order_date, status)
VALUES(1, '2019-01-02', 1);
INSERT INTO order_items
VALUES(LAST_INSERT_ID(), 1, 1, 1.5)
	
-- 将查找出来的数据,复制到另一个表中
CREATE TABLE invoice_archive AS
SELECT 
	i.invoice_id,
	i.number,
	c.name AS client,
	i.invoice_total,
	i.payment_total,
	i.invoice_date,
	i.payment_date
	i.due_date
FROM invoice i
JOIN client c
	USING (client_id)
WHERE payment_date IS NOT NULL
	
--更新数据
UPDATE invoice
SET payment_total = 10, payment_date = '2019-03-01'
WHERE invoice_id = 1
	
--从查找出来的数据中,进行更新
UPDATE invoice
SET 
	payment_total = invoice_total * 0.5
	payment_date = due_date
WHERE client_id IN 
				(SELECT client_id
                FROM clients
                WHERE name = 'MyWorks')
	
--聚合函数
SELECT 
	MAX(invoice_total) AS hightest,
	MIN(invoice_total) AS lowest
    AVG(invoice_total) AS average
    SUM(invoice_total) AS total
    COUNT(invoice_total) AS count_num
FROM invoice

-- GROUP BY
SELECT
	state,
	city,
	SUM(invoice_total) AS total_sale
FROM invoice i
JOIN clients USING (client_id)

SELECT 
	date,
	SUM(amount) AS total
FROM payment
GROUP BY date
ORDER BY date

--聚合函数筛选
SELECT 
	client_id
	SUM(invoice_total) AS total_sale
FROM invoice
GROUP BY client_id
HAVING total_sales > 500
-- 在GROUP BY之前使用where, 在GROUP BY之后使用HAVING
-- 在HAVING中筛选的列,需要是前面select中提出的

--分组求和函数With Rollup
SELECT
	payment_method,
	SUM(amount) AS total
FROM payments p
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
GROUP BY payment_method WITH ROLLUP

SELECT *
FROM clients
WHERE client_id NOT IN (
	SELECT DISTINCT client_id
    FROM invoices
)

-- ALL关键字查询
SELECT *
FROM invoices
WHERE invoice_total > ALL (
	SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
)

--SOME关键字
SELECT *
FROM invoice_total > SOME(
	SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
)

--嵌套子查询
SELECT *
FROM clients C
WHERE EXISTS(
	SELECT client_id
    FROM invoices
    WHERE client_id = c.client_id
)

-- 
SELECT 
	invoice_id,
	invoice_total,
	(SELECT AVG(invoice_total)
    	FROM invoice) AS invoice_average,
    invoice_total-invoice_average
FROM invoices

SELECT 
	client_id,
	(SELECT SUM(invoice_total)
    FROM invoices
    WHERE client_id = c.client_id) AS total_sales
    (SELECT AVG(invoice_total) FROM invoices) AS avg
    (SELECT total_sales - avg) AS difference
FROM client c
```


