# DBMS

>A database is a collection of data stored in a format that can easily be accessed.

In order to manage our databases we use a software application called DBMS. We interact with DBMS only. Classfied into two types:

1. Relational DDMS (SQL to interact) : MySQL, Oracle, SQL Server
2. Non-relational DMBS (NoSQL to interact, don't understand SQL)

> SQL - Structured English Query Langauge.

Admin: starting/stopping server, importing/exporting data.

Schema shows databases in the current database server.

In database server, there is one sys database by default, it's used by mysql internally to do it's work.

```
DROP DATABASE IF EXISTS `sql_invoicing`;
CREATE DATABASE `sql_invoicing`; 
USE `sql_invoicing`;

SET NAMES utf8 ;
SET character_set_client = utf8mb4 ;

CREATE TABLE `payment_methods` (
  `payment_method_id` tinyint(4) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  PRIMARY KEY (`payment_method_id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `payment_methods` VALUES (1,'Credit Card');
INSERT INTO `payment_methods` VALUES (2,'Cash');
INSERT INTO `payment_methods` VALUES (3,'PayPal');
INSERT INTO `payment_methods` VALUES (4,'Wire Transfer');

CREATE TABLE `clients` (
  `client_id` int(11) NOT NULL,
  `name` varchar(50) NOT NULL,
  `address` varchar(50) NOT NULL,
  `city` varchar(50) NOT NULL,
  `state` char(2) NOT NULL,
  `phone` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`client_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `clients` VALUES (1,'Vinte','3 Nevada Parkway','Syracuse','NY','315-252-7305');
INSERT INTO `clients` VALUES (2,'Myworks','34267 Glendale Parkway','Huntington','WV','304-659-1170');
INSERT INTO `clients` VALUES (3,'Yadel','096 Pawling Parkway','San Francisco','CA','415-144-6037');
INSERT INTO `clients` VALUES (4,'Kwideo','81674 Westerfield Circle','Waco','TX','254-750-0784');
INSERT INTO `clients` VALUES (5,'Topiclounge','0863 Farmco Road','Portland','OR','971-888-9129');

CREATE TABLE `invoices` (
  `invoice_id` int(11) NOT NULL,
  `number` varchar(50) NOT NULL,
  `client_id` int(11) NOT NULL,
  `invoice_total` decimal(9,2) NOT NULL,
  `payment_total` decimal(9,2) NOT NULL DEFAULT '0.00',
  `invoice_date` date NOT NULL,
  `due_date` date NOT NULL,
  `payment_date` date DEFAULT NULL,
  PRIMARY KEY (`invoice_id`),
  KEY `FK_client_id` (`client_id`),
  CONSTRAINT `FK_client_id` FOREIGN KEY (`client_id`) REFERENCES `clients` (`client_id`) ON DELETE RESTRICT ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `invoices` VALUES (1,'91-953-3396',2,101.79,0.00,'2019-03-09','2019-03-29',NULL);
INSERT INTO `invoices` VALUES (2,'03-898-6735',5,175.32,8.18,'2019-06-11','2019-07-01','2019-02-12');
INSERT INTO `invoices` VALUES (3,'20-228-0335',5,147.99,0.00,'2019-07-31','2019-08-20',NULL);
INSERT INTO `invoices` VALUES (4,'56-934-0748',3,152.21,0.00,'2019-03-08','2019-03-28',NULL);
INSERT INTO `invoices` VALUES (5,'87-052-3121',5,169.36,0.00,'2019-07-18','2019-08-07',NULL);
INSERT INTO `invoices` VALUES (6,'75-587-6626',1,157.78,74.55,'2019-01-29','2019-02-18','2019-01-03');
INSERT INTO `invoices` VALUES (7,'68-093-9863',3,133.87,0.00,'2019-09-04','2019-09-24',NULL);
INSERT INTO `invoices` VALUES (8,'78-145-1093',1,189.12,0.00,'2019-05-20','2019-06-09',NULL);
INSERT INTO `invoices` VALUES (9,'77-593-0081',5,172.17,0.00,'2019-07-09','2019-07-29',NULL);
INSERT INTO `invoices` VALUES (10,'48-266-1517',1,159.50,0.00,'2019-06-30','2019-07-20',NULL);
INSERT INTO `invoices` VALUES (11,'20-848-0181',3,126.15,0.03,'2019-01-07','2019-01-27','2019-01-11');
INSERT INTO `invoices` VALUES (13,'41-666-1035',5,135.01,87.44,'2019-06-25','2019-07-15','2019-01-26');
INSERT INTO `invoices` VALUES (15,'55-105-9605',3,167.29,80.31,'2019-11-25','2019-12-15','2019-01-15');
INSERT INTO `invoices` VALUES (16,'10-451-8824',1,162.02,0.00,'2019-03-30','2019-04-19',NULL);
INSERT INTO `invoices` VALUES (17,'33-615-4694',3,126.38,68.10,'2019-07-30','2019-08-19','2019-01-15');
INSERT INTO `invoices` VALUES (18,'52-269-9803',5,180.17,42.77,'2019-05-23','2019-06-12','2019-01-08');
INSERT INTO `invoices` VALUES (19,'83-559-4105',1,134.47,0.00,'2019-11-23','2019-12-13',NULL);

CREATE TABLE `payments` (
  `payment_id` int(11) NOT NULL AUTO_INCREMENT,
  `client_id` int(11) NOT NULL,
  `invoice_id` int(11) NOT NULL,
  `date` date NOT NULL,
  `amount` decimal(9,2) NOT NULL,
  `payment_method` tinyint(4) NOT NULL,
  PRIMARY KEY (`payment_id`),
  KEY `fk_client_id_idx` (`client_id`),
  KEY `fk_invoice_id_idx` (`invoice_id`),
  KEY `fk_payment_payment_method_idx` (`payment_method`),
  CONSTRAINT `fk_payment_client` FOREIGN KEY (`client_id`) REFERENCES `clients` (`client_id`) ON UPDATE CASCADE,
  CONSTRAINT `fk_payment_invoice` FOREIGN KEY (`invoice_id`) REFERENCES `invoices` (`invoice_id`) ON UPDATE CASCADE,
  CONSTRAINT `fk_payment_payment_method` FOREIGN KEY (`payment_method`) REFERENCES `payment_methods` (`payment_method_id`)
) ENGINE=InnoDB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `payments` VALUES (1,5,2,'2019-02-12',8.18,1);
INSERT INTO `payments` VALUES (2,1,6,'2019-01-03',74.55,1);
INSERT INTO `payments` VALUES (3,3,11,'2019-01-11',0.03,1);
INSERT INTO `payments` VALUES (4,5,13,'2019-01-26',87.44,1);
INSERT INTO `payments` VALUES (5,3,15,'2019-01-15',80.31,1);
INSERT INTO `payments` VALUES (6,3,17,'2019-01-15',68.10,1);
INSERT INTO `payments` VALUES (7,5,18,'2019-01-08',32.77,1);
INSERT INTO `payments` VALUES (8,5,18,'2019-01-08',10.00,2);


DROP DATABASE IF EXISTS `sql_store`;
CREATE DATABASE `sql_store`;
USE `sql_store`;

CREATE TABLE `products` (
  `product_id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `quantity_in_stock` int(11) NOT NULL,
  `unit_price` decimal(4,2) NOT NULL,
  PRIMARY KEY (`product_id`)
) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `products` VALUES (1,'Foam Dinner Plate',70,1.21);
INSERT INTO `products` VALUES (2,'Pork - Bacon,back Peameal',49,4.65);
INSERT INTO `products` VALUES (3,'Lettuce - Romaine, Heart',38,3.35);
INSERT INTO `products` VALUES (4,'Brocolinni - Gaylan, Chinese',90,4.53);
INSERT INTO `products` VALUES (5,'Sauce - Ranch Dressing',94,1.63);
INSERT INTO `products` VALUES (6,'Petit Baguette',14,2.39);
INSERT INTO `products` VALUES (7,'Sweet Pea Sprouts',98,3.29);
INSERT INTO `products` VALUES (8,'Island Oasis - Raspberry',26,0.74);
INSERT INTO `products` VALUES (9,'Longan',67,2.26);
INSERT INTO `products` VALUES (10,'Broom - Push',6,1.09);


CREATE TABLE `shippers` (
  `shipper_id` smallint(6) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  PRIMARY KEY (`shipper_id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `shippers` VALUES (1,'Hettinger LLC');
INSERT INTO `shippers` VALUES (2,'Schinner-Predovic');
INSERT INTO `shippers` VALUES (3,'Satterfield LLC');
INSERT INTO `shippers` VALUES (4,'Mraz, Renner and Nolan');
INSERT INTO `shippers` VALUES (5,'Waters, Mayert and Prohaska');


CREATE TABLE `customers` (
  `customer_id` int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(50) NOT NULL,
  `last_name` varchar(50) NOT NULL,
  `birth_date` date DEFAULT NULL,
  `phone` varchar(50) DEFAULT NULL,
  `address` varchar(50) NOT NULL,
  `city` varchar(50) NOT NULL,
  `state` char(2) NOT NULL,
  `points` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`customer_id`)
) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `customers` VALUES (1,'Babara','MacCaffrey','1986-03-28','781-932-9754','0 Sage Terrace','Waltham','MA',2273);
INSERT INTO `customers` VALUES (2,'Ines','Brushfield','1986-04-13','804-427-9456','14187 Commercial Trail','Hampton','VA',947);
INSERT INTO `customers` VALUES (3,'Freddi','Boagey','1985-02-07','719-724-7869','251 Springs Junction','Colorado Springs','CO',2967);
INSERT INTO `customers` VALUES (4,'Ambur','Roseburgh','1974-04-14','407-231-8017','30 Arapahoe Terrace','Orlando','FL',457);
INSERT INTO `customers` VALUES (5,'Clemmie','Betchley','1973-11-07',NULL,'5 Spohn Circle','Arlington','TX',3675);
INSERT INTO `customers` VALUES (6,'Elka','Twiddell','1991-09-04','312-480-8498','7 Manley Drive','Chicago','IL',3073);
INSERT INTO `customers` VALUES (7,'Ilene','Dowson','1964-08-30','615-641-4759','50 Lillian Crossing','Nashville','TN',1672);
INSERT INTO `customers` VALUES (8,'Thacher','Naseby','1993-07-17','941-527-3977','538 Mosinee Center','Sarasota','FL',205);
INSERT INTO `customers` VALUES (9,'Romola','Rumgay','1992-05-23','559-181-3744','3520 Ohio Trail','Visalia','CA',1486);
INSERT INTO `customers` VALUES (10,'Levy','Mynett','1969-10-13','404-246-3370','68 Lawn Avenue','Atlanta','GA',796);


CREATE TABLE `order_statuses` (
  `order_status_id` tinyint(4) NOT NULL,
  `name` varchar(50) NOT NULL,
  PRIMARY KEY (`order_status_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `order_statuses` VALUES (1,'Processed');
INSERT INTO `order_statuses` VALUES (2,'Shipped');
INSERT INTO `order_statuses` VALUES (3,'Delivered');


CREATE TABLE `orders` (
  `order_id` int(11) NOT NULL AUTO_INCREMENT,
  `customer_id` int(11) NOT NULL,
  `order_date` date NOT NULL,
  `status` tinyint(4) NOT NULL DEFAULT '1',
  `comments` varchar(2000) DEFAULT NULL,
  `shipped_date` date DEFAULT NULL,
  `shipper_id` smallint(6) DEFAULT NULL,
  PRIMARY KEY (`order_id`),
  KEY `fk_orders_customers_idx` (`customer_id`),
  KEY `fk_orders_shippers_idx` (`shipper_id`),
  KEY `fk_orders_order_statuses_idx` (`status`),
  CONSTRAINT `fk_orders_customers` FOREIGN KEY (`customer_id`) REFERENCES `customers` (`customer_id`) ON UPDATE CASCADE,
  CONSTRAINT `fk_orders_order_statuses` FOREIGN KEY (`status`) REFERENCES `order_statuses` (`order_status_id`) ON UPDATE CASCADE,
  CONSTRAINT `fk_orders_shippers` FOREIGN KEY (`shipper_id`) REFERENCES `shippers` (`shipper_id`) ON UPDATE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `orders` VALUES (1,6,'2019-01-30',1,NULL,NULL,NULL);
INSERT INTO `orders` VALUES (2,7,'2018-08-02',2,NULL,'2018-08-03',4);
INSERT INTO `orders` VALUES (3,8,'2017-12-01',1,NULL,NULL,NULL);
INSERT INTO `orders` VALUES (4,2,'2017-01-22',1,NULL,NULL,NULL);
INSERT INTO `orders` VALUES (5,5,'2017-08-25',2,'','2017-08-26',3);
INSERT INTO `orders` VALUES (6,10,'2018-11-18',1,'Aliquam erat volutpat. In congue.',NULL,NULL);
INSERT INTO `orders` VALUES (7,2,'2018-09-22',2,NULL,'2018-09-23',4);
INSERT INTO `orders` VALUES (8,5,'2018-06-08',1,'Mauris enim leo, rhoncus sed, vestibulum sit amet, cursus id, turpis.',NULL,NULL);
INSERT INTO `orders` VALUES (9,10,'2017-07-05',2,'Nulla mollis molestie lorem. Quisque ut erat.','2017-07-06',1);
INSERT INTO `orders` VALUES (10,6,'2018-04-22',2,NULL,'2018-04-23',2);


CREATE TABLE `order_items` (
  `order_id` int(11) NOT NULL AUTO_INCREMENT,
  `product_id` int(11) NOT NULL,
  `quantity` int(11) NOT NULL,
  `unit_price` decimal(4,2) NOT NULL,
  PRIMARY KEY (`order_id`,`product_id`),
  KEY `fk_order_items_products_idx` (`product_id`),
  CONSTRAINT `fk_order_items_orders` FOREIGN KEY (`order_id`) REFERENCES `orders` (`order_id`) ON UPDATE CASCADE,
  CONSTRAINT `fk_order_items_products` FOREIGN KEY (`product_id`) REFERENCES `products` (`product_id`) ON UPDATE CASCADE
) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `order_items` VALUES (1,4,4,3.74);
INSERT INTO `order_items` VALUES (2,1,2,9.10);
INSERT INTO `order_items` VALUES (2,4,4,1.66);
INSERT INTO `order_items` VALUES (2,6,2,2.94);
INSERT INTO `order_items` VALUES (3,3,10,9.12);
INSERT INTO `order_items` VALUES (4,3,7,6.99);
INSERT INTO `order_items` VALUES (4,10,7,6.40);
INSERT INTO `order_items` VALUES (5,2,3,9.89);
INSERT INTO `order_items` VALUES (6,1,4,8.65);
INSERT INTO `order_items` VALUES (6,2,4,3.28);
INSERT INTO `order_items` VALUES (6,3,4,7.46);
INSERT INTO `order_items` VALUES (6,5,1,3.45);
INSERT INTO `order_items` VALUES (7,3,7,9.17);
INSERT INTO `order_items` VALUES (8,5,2,6.94);
INSERT INTO `order_items` VALUES (8,8,2,8.59);
INSERT INTO `order_items` VALUES (9,6,5,7.28);
INSERT INTO `order_items` VALUES (10,1,10,6.01);
INSERT INTO `order_items` VALUES (10,9,9,4.28);

CREATE TABLE `sql_store`.`order_item_notes` (
  `note_id` INT NOT NULL,
  `order_Id` INT NOT NULL,
  `product_id` INT NOT NULL,
  `note` VARCHAR(255) NOT NULL,
  PRIMARY KEY (`note_id`));

INSERT INTO `order_item_notes` (`note_id`, `order_Id`, `product_id`, `note`) VALUES ('1', '1', '2', 'first note');
INSERT INTO `order_item_notes` (`note_id`, `order_Id`, `product_id`, `note`) VALUES ('2', '1', '2', 'second note');


DROP DATABASE IF EXISTS `sql_hr`;
CREATE DATABASE `sql_hr`;
USE `sql_hr`;


CREATE TABLE `offices` (
  `office_id` int(11) NOT NULL,
  `address` varchar(50) NOT NULL,
  `city` varchar(50) NOT NULL,
  `state` varchar(50) NOT NULL,
  PRIMARY KEY (`office_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `offices` VALUES (1,'03 Reinke Trail','Cincinnati','OH');
INSERT INTO `offices` VALUES (2,'5507 Becker Terrace','New York City','NY');
INSERT INTO `offices` VALUES (3,'54 Northland Court','Richmond','VA');
INSERT INTO `offices` VALUES (4,'08 South Crossing','Cincinnati','OH');
INSERT INTO `offices` VALUES (5,'553 Maple Drive','Minneapolis','MN');
INSERT INTO `offices` VALUES (6,'23 North Plaza','Aurora','CO');
INSERT INTO `offices` VALUES (7,'9658 Wayridge Court','Boise','ID');
INSERT INTO `offices` VALUES (8,'9 Grayhawk Trail','New York City','NY');
INSERT INTO `offices` VALUES (9,'16862 Westend Hill','Knoxville','TN');
INSERT INTO `offices` VALUES (10,'4 Bluestem Parkway','Savannah','GA');



CREATE TABLE `employees` (
  `employee_id` int(11) NOT NULL,
  `first_name` varchar(50) NOT NULL,
  `last_name` varchar(50) NOT NULL,
  `job_title` varchar(50) NOT NULL,
  `salary` int(11) NOT NULL,
  `reports_to` int(11) DEFAULT NULL,
  `office_id` int(11) NOT NULL,
  PRIMARY KEY (`employee_id`),
  KEY `fk_employees_offices_idx` (`office_id`),
  KEY `fk_employees_employees_idx` (`reports_to`),
  CONSTRAINT `fk_employees_managers` FOREIGN KEY (`reports_to`) REFERENCES `employees` (`employee_id`),
  CONSTRAINT `fk_employees_offices` FOREIGN KEY (`office_id`) REFERENCES `offices` (`office_id`) ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `employees` VALUES (37270,'Yovonnda','Magrannell','Executive Secretary',63996,NULL,10);
INSERT INTO `employees` VALUES (33391,'D\'arcy','Nortunen','Account Executive',62871,37270,1);
INSERT INTO `employees` VALUES (37851,'Sayer','Matterson','Statistician III',98926,37270,1);
INSERT INTO `employees` VALUES (40448,'Mindy','Crissil','Staff Scientist',94860,37270,1);
INSERT INTO `employees` VALUES (56274,'Keriann','Alloisi','VP Marketing',110150,37270,1);
INSERT INTO `employees` VALUES (63196,'Alaster','Scutchin','Assistant Professor',32179,37270,2);
INSERT INTO `employees` VALUES (67009,'North','de Clerc','VP Product Management',114257,37270,2);
INSERT INTO `employees` VALUES (67370,'Elladine','Rising','Social Worker',96767,37270,2);
INSERT INTO `employees` VALUES (68249,'Nisse','Voysey','Financial Advisor',52832,37270,2);
INSERT INTO `employees` VALUES (72540,'Guthrey','Iacopetti','Office Assistant I',117690,37270,3);
INSERT INTO `employees` VALUES (72913,'Kass','Hefferan','Computer Systems Analyst IV',96401,37270,3);
INSERT INTO `employees` VALUES (75900,'Virge','Goodrum','Information Systems Manager',54578,37270,3);
INSERT INTO `employees` VALUES (76196,'Mirilla','Janowski','Cost Accountant',119241,37270,3);
INSERT INTO `employees` VALUES (80529,'Lynde','Aronson','Junior Executive',77182,37270,4);
INSERT INTO `employees` VALUES (80679,'Mildrid','Sokale','Geologist II',67987,37270,4);
INSERT INTO `employees` VALUES (84791,'Hazel','Tarbert','General Manager',93760,37270,4);
INSERT INTO `employees` VALUES (95213,'Cole','Kesterton','Pharmacist',86119,37270,4);
INSERT INTO `employees` VALUES (96513,'Theresa','Binney','Food Chemist',47354,37270,5);
INSERT INTO `employees` VALUES (98374,'Estrellita','Daleman','Staff Accountant IV',70187,37270,5);
INSERT INTO `employees` VALUES (115357,'Ivy','Fearey','Structural Engineer',92710,37270,5);


DROP DATABASE IF EXISTS `sql_inventory`;
CREATE DATABASE `sql_inventory`;
USE `sql_inventory`;


CREATE TABLE `products` (
  `product_id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `quantity_in_stock` int(11) NOT NULL,
  `unit_price` decimal(4,2) NOT NULL,
  PRIMARY KEY (`product_id`)
) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
INSERT INTO `products` VALUES (1,'Foam Dinner Plate',70,1.21);
INSERT INTO `products` VALUES (2,'Pork - Bacon,back Peameal',49,4.65);
INSERT INTO `products` VALUES (3,'Lettuce - Romaine, Heart',38,3.35);
INSERT INTO `products` VALUES (4,'Brocolinni - Gaylan, Chinese',90,4.53);
INSERT INTO `products` VALUES (5,'Sauce - Ranch Dressing',94,1.63);
INSERT INTO `products` VALUES (6,'Petit Baguette',14,2.39);
INSERT INTO `products` VALUES (7,'Sweet Pea Sprouts',98,3.29);
INSERT INTO `products` VALUES (8,'Island Oasis - Raspberry',26,0.74);
INSERT INTO `products` VALUES (9,'Longan',67,2.26);
INSERT INTO `products` VALUES (10,'Broom - Push',6,1.09);
```
## In databases:

1. Tables 
2. Views: Virtual Tables. Combine data from multiple tables. Power for creating reports.
3. Stored Procedures: Proc for getting all the customers in a  given city.
4. Functions

## Redundacy Issue Example:

Orders table:

order_id, customer_id

If we directly store customer data and a customer place many orders. Later he changed his address/phone, we have to change at every position.

##  Select

```
USE sql_store;
select * from ..;
select col1, col2 from ..;
select col1, col2 from .. where col1 = .. order by col1;

select 1, 2;                            -- from, where, order by clauses are optional

Linespaces, whitespaces and tabs ignored while executing SQL statements.
Recommended: Capital letters for keywords, lower for others.
             If query size large, better put each clause on new line.

select points + 10 from ..;             -- * / % - (precedence)

()
*  /
+  -

select (price*1.1) AS new_price from ..         -- descriptive names
                                                --For spaces in col names use double quotes

SELECT DISTINCTcol1  FROM ..;

SELECT COALESCE(NULL, NULL, NULL, 'W3Schools.com', NULL, 'Example.com');
Returns first non NULL value: W3schools

```

## Where

```
< 
>
<=
>=
<> !=
=

String: sequence of characters, Put in single quotes. (String/text data)

By default data is case insenstive. But we can make it sensitive using COLLATE.

In SQL, dates are not strings but we still include dates in quotes.
'YYYY-MM-DD'

NOT (to revert the result)
AND (High prec than OR)
OR 


select * from .. where col1 = 'a' OR col1 = 'b' Or col1 = 'c';
select * from .. where col1 in ('a', 'b', 'c');
NOT IN


select * from .. where points >= 1000 AND points <= 3000;
select * from .. where points BETWEEN 1000 AND 3000 (Recommended for range);
Select * from .. where birth_date between '1990-01-01' and '2000-01-01'; 


Like 'b%     starts with b and then any number of chars
Like '%b%'   any number of chars before and after b
Like '%y'    ends with y
Like '_y'    single char before y
Like 'y--'
Like 'j____n'

select * from customers where address LIKE '%mansa%' OR
                              address LIKE '%rama%';
select * from customers where phone NOT LIKE '%9'





REGEXP : Very powerfule when it comes to searching for strings. For more complex patters.

select * from customers where name REGEXP 'field' 
Exactly Same as LIKE '%field%'

Search Patterns:

^field : name must start with field
field$ : end
^field|mac|rose : (either start with field) or (mac) or (rose)
[gim]e: search for gi, ie, me
e[gim]: search for eg, ei, em
[a-h]e 



select * from customers where phone IS NULL;
select * from customers where phone IS NOT NULL;
eg: In orders tables, some orders don't has shipped_date. To get these orders.



Records by default sorted by the primary key.
select * from customers order by state desc, first_name asc;

In MySQL, In order by clause we can use col name which is not present in the select statment, but we can't do in all other DMBS's
Expression we use in the order by clause doesn't have to be col name, it can be alias or an arithmatic expression too.

select *, quantity*unit_price AS total_price from order_items where order_id = 2 ORDER BY total_price DESC;

select first_name, last_name from customers order by 1, 2; (Avoid due to future changes);





LIMIT 300;
OFFSET very useful in situations where we want to paginate the data.
LIMIT 6,3;


-- ORDER matters
SELECT *
FROM CUSTOMERS
WHERE
ORDER BY points DESC
LIMIT 3
```

## Joins

```
1. Inner join
2. Outer join

INNER JOIN -- inner keyword is optional

select order_id, c.customer_id, first_name, last_name
from orders o
join customers c
on o.customer_id = c.customer_id;

See customer_id prefix bcz of ambiguity. 
Select * from ......... is valid too. No ambiguity

eg: unit_price column present in order_items and products both bcz unit_price may change after order.




JOINING ACROSS DATABASES: (we have to mention the name of databases as prefix)

use sql_store;
select * from order_items oi
join sql_inventory.products p on oi.product_id = p.product_id;



SELF JOIN
select e.employee_id, e.first_name, m.first_name as manager
from employees e
join employees m
on e.reports_to = m.employee_id




JOINING Multiple tables:
Orders - customer - status (processed, shipped, delivered)
payments - client - payment_method(credit card, paypal, cash, wire transfer)



COMPOUND JOIN CONDITIONS
composite primary key contain more than one column
Multiple conditions to join these tables, required when one of the table contains composite primary key.



IMPLICIT JOIN INDEX (same as inner join)
Select * from orders o, customers c where o.customer_id = c.customer_id;
If we forgot where it becomes cross join, but if we forget ON in joins, it would generate error.




customers - orders (Customer who hasn't placed any order)
LEFT JOIN - outer keyword is optional
RIGHT JOIN - same as left, we just have to change the order of tables.



OUTER JOINS BETWEEN MULTIPLE TABLES:
Avoid right, use left to easily visualize


SELF OUTER JOIN:
employees who don't have any manager like CEO



THE USING CLAUSE - when query becomes more complex (Replace ON clause)
ON o.customer_id = c.customer_id = USING(customer_id)
When we have to exact same name of column
eg: 
select * from order_items oi
join order_item_notes oin 
using (order_id, product_id);




Natural Join
Database engine figure out or guess the join. We don't have control over it. They can produce unexpected results.
select * from order o join customers c;



CROSS JOIN 
select * from customer cross join products p;
eg: to generate every possible combination. Like diff size * diff color

Implicit syntax
select * from table1, table2;




UNION:
Joins can join columns from multiple tables. In SQL we can also combine rows from mutilple tables.
eg: place orders in this year as active and from prev years as archive

select order_id, order_date, 'Active' AS status from orders where order_date >= '2021-01-01'
UNION
select order_id, order_date, 'Archived' AS status from orders where order_date < '2021-01-01'

select first_name from customers
union
select name from shippers

Name of the column is based on the first query.

```

## Insert, Update, Delete


```
CHAR - DBMS insert remaining characters as spaces.
VARCHAR - varible character. (extra 1 space for storing the size internally)

Auto Increment - mostly for primary key
We can pass default for auto increment and default values.
Always generate new value, even if we delete some last rows.

Insert into customers values (DEFAULT, 'Jashan', 1, NULL, DEFAULT,...);
Insert into customers (first_name, last_name) values (..);

Insert into shippers(name)
values ('Shipper1'),
       ('Shipper2')
       ('Shipper3')





Inserting Hierarichal data:
Order, Order items (parent, child relationship) (Child has multiple rows)

Order_id is auto increment
Insert into orders (customer_id, order_date, status) values ....
Insert into order_items values(
    (LAST_INSERT_ID(), 1, 1, 2.95),
    (LAST_INSERT_ID(), 1, 1, 2.95)
)




Creating copy of a table:
CREATE table orders_archived As  select * from orders;    -- Sub query in create table statement
By this, MYSQL ignore attributes. It ignores primary key and auto increment attributes in this.

Truncate the data and use this insert query with sub query.
INSERT into orders_archived
select * from orders where order_date < '2019-01-01';





Update invoices set payment_total = invoice_total*0.5, p_date = '2019-03-01' where invoice_id = 1;
Update invoices set payment_total = DEFAULT, p_date = NULL where invoice_id = 1;

Update invoices set .. where client_id = 3
// MySQL work in safe update mode. So, change settings first.



Subqueries in Update statement eg: update invoices of client provided their name only.
Update invoices set payment_total = invoice_total*0.5, p_date = NULL where client_id = (select client_id from clients where name = 'jashan')

If subquery can return multiple records.
Update invoices set payment_total = invoice_total*0.5, p_date = NULL where client_id IN (select client_id from clients where name = 'jashan')

First test using:
Update invoices
SET
    payment_total = invoice_total* 0.5,
    payment_date = due_date
Select * from invoices                  -- remove this when confident
Where payment_date IS NULL;





DELETE FROM invoices Where invoice_id = 1;

Delete from invoices
where invoice_id = 1 in(
    select * from clients where name = 'MyWorks'
)
```


## Aggregate Functions 

> Queries that summarize data

```
Only operate on Non NULL values.

Max()
Min()
Avg()
Sum()
Count()

In order to count no. of records irrespective of NULL values use count(*)
By default these functions takes duplicate values, if we want to remove just use distinct keyword.

select sum(distinct invoice_total * 1.1) as total from invoices where ..


GROUP BY - Every column without aggregate must included in group by, but every col in group by may or may not be included in select

select c1, c2, c3 from data group by c1, c2, c3;
select distinct c1, c2, c3 from data;

HAVING - to filter groups

select client_id, sum(invoice_total) as total_sales from invoices group by client_id having total_sales > 5000;

We can reference col which is not included in the select clause in having, but we can reference not selected columns in where.




Only part of MYSQL, not part of standard SQL
THE ROLLUP OPERATOR (Summarizes out entire result set - cols included in the aggregate functions)

select client_id, sum(invoice_total) as total_sales from invoices group by client_id WITH ROLLUP;

Last row would be the sum of the cliend_id's

Summary value for each group as well as the entire result set:

select state, city, sum(invoice_total) as total_sales
from invoices i
join clients c using (client_id)
group by state, city with rollup;

for every state there is a rollup value;

When we use rollup operator, we can't use alias in the group by clause;
```



## Complex Queries

```
Subqueries is a select statement within other SQL statement. In where clause, from clause and select clause;

select * from products where unit_price > (select unit_price from products where product_id = 3);

IN Operator - products which hasn't been ordered yet (Used in case of mutilple value)
select * from products where product_id not in (select distinct product_id from order_items);

Same query using joins- (Readability, performance)
select * from products left join order_items using(product_id) where product_id IS NULL;

```

## Funtions
```
Numeric:
Round(5.73) = 6, Round(5.7345, 2) = 5.73
Truncate(5.7345, 2) = 5.73
CEILING(2.1) = 3
FLOOR(2.1) = 2
ABS(-5.2) = 5.2
RAND() = RANDOM floating point number between 0 and 1

String:
Length('sky') = 3
upper('sky')
lower('sky')
LTRIM('   sky')
RTRIM('sky  ')
TRIM('   sky ')
left('jashan', 4) = jash
right('jashan', 4)
substring('Jashanbansal', 1, 4) = jash (1 based indexing)
substring('Jashanbansal', 1) = Jashanbansal
Locate('n', 'Kindergarten') = 3 (Searching is not case sensitive)
                                (If not exist return 0, we can also search for seq of chars)
Replace('Kindergarten', 'garten', 'garden')
concat('First', 'last') = Firstlast

Date:
Now() = date time
curdate() 
curtime() 

year(now()), month, day, hour, minute, second
DAYNAME(now()) = MONDAY, MONTHNAME
EXTRACT(YEAR FROM NOW())   // SQL Standard)

DATA_FORMAT(NOW(), '%y') = two digit year 21
                     Y   = four digit year 2021
                   '%m %Y' = 08 2021
                   %M %Y = August 2021
                   %M %d %Y = August 01 2021

TIME_FORMAT(NOW(), '%H:%i %p) = 12:58 PM

Date_Add(NOW(), Interval 1 day)
                          year
                        -1 year = to go back in time
Date_sub(now(), INTERVAL 1 year)
datediff('2021-08-01' , '2021-07-02')  returns diff in days only                    
TIME_TO_SEC('09:00') = NO. of seconds elapsed since midnight
select TIME_TO_SEC('09:00') - TIME_TO_SEC('09:02')  = -120


IFNULL(someCol, 'New value') // Replace null value
COLESCE(shipper_id, comments, 'Not Assigned')  // replace first non null value in a list


IF(Expression, first, second);
select 
    order_id, 
    order_date, 
    IF(YEAR(order_date) = YEAR(Now), 
       'ACTIVE', 
       'ARCHIVED') as category
FROM orders;


select 
    order_id, 
    order_date, 
    CASE
        when year(order_date) = year(now())) THEN 'Active'
        when year(order_date) = year(now())-1) THEN 'Last Year'
        when year(order_date) < year(now())-1) THEN 'Archived'
        else 'future'
    end as category
FROM orders;
```

## Views
>Sometimes queries becomes complex when we use joins and subqueries, then we can use views to simplify the select statements. We don't have to write those queries again and again. We can query views just like tables.

> Views are like virtal tables, they don't store data. Data stored in actual tables. Provide just the view to underlined tables, that't why they called as view.

> Best practice is to create files for creation of views, so later it can be used by others to create the same view.

```
create view sales_by_client as 
select c.client_id, c.name, sum(invoice_total) as total_sales
from client c join invoices i using(client_id) group by client_id, name

select * from sales_by_client order by total_sales desc;


drop view sales_by_client;

create or replace view ....



Updatable views: we can use views in update, insert and delete statements but only under certain circumstances. If the view don't have
-- distinct
-- aggregate functions, group by, having
-- union
then we can update data through it.

delete from invoices_with_balance when invoice_id = 1;
update invoices_with_balance set due_date = date_add(due_date, interval 2 dsay) where invoice_id = 1;

We can insert also in the view, but it will only work if view has all the required cols in the underlined tables

Most of the times, we update date using tables, but sometimes we don't have access to main table due to security reasons. That time we use views to update date in tables.


WITH CHECK OPTION 
- At last of view creation. If we try to modify a row i.e. they no longer be available in the view, we will get an error.



Other benefits:
1. Simplify queries
2. Reduce the impact of changes
-- Let say we update the name of column, just use alias in view.
-- Let say we move some col in other tables, we join other table with prev table and bring back the column
3. Restrict access to the data (restrict rows using where and cols using select) 




















