

#########################################################################
                         ASSIGNMENT 4 Apache PIG
#########################################################################
TASK 1
******
(a) Top 5 employees (employee id and employee name) with highest rating. (In case two
employees have same rating, employee with name coming first in dictionary should get
preference)

grunt> emp_details = load '/home/acadgild/employee_details.txt' using PigStorage(',') as (id:int,name:chararray,salary:int,rating:int);

grunt> dump emp_details

(101,Pandi,25000,1)
(102,Raj,30000,2)
(103,Krishna,15000,3)
(104,Kumar,20000,4)
(105,Mani,23000,4)
(106,Kandan,24000,1)
(107,Manoj,10000,5)
(108,Ram,15000,2)
(109,Bala,17000,3)
(110,suresh,12000,5)

grunt> emp_expenses = load '/home/acadgild/employee_expences.txt' using PigStorage() as (id:int,expenses:int);

grunt> dump emp_expenses

(101,200)
(102,100)
(103,400)
(104,500)
(105,200)
(106,100)
(107,200)
(108,300)
(109,300)
(110,400)
*******************************************************************************************************************

(b) Top 3 employees (employee id and employee name) with highest salary, whose employee id
is an odd number. (In case two employees have same salary, employee with name coming first
in dictionary should get preference)

grunt> employee = ORDER employee_details BY rating asc, id asc;


(101,Pandi,25000,1)
(106,Kandan,24000,1)
(102,Raj,30000,2)
(108,Ram,15000,2)
(103,Krishna,15000,3)
(109,Bala,17000,3)
(104,Kumar,20000,4)
(105,Mani,23000,4)
(107,Manoj,10000,5)
(110,suresh,12000,5)
grunt>

grunt> limit_employee = limit employee 5;

(101,Pandi,25000,1)
(106,Kandan,24000,1)
(102,Raj,30000,2)
(108,Ram,15000,2)
(103,Krishna,15000,3)

grunt> top_employee = foreach limit_employee generate id,name;

(101,Pandi)
(106,Kandan)
(102,Raj)
(108,Ram)
(103,Krishna)
**********************************************************************************************************************

(c) Employee (employee id and employee name) with maximum expense (In case two employees have same expense, employee with name coming first in dictionary should get
preference)

grunt> order_employee = order emp_details by salary desc;

(102,Raj,30000,2)
(101,Pandi,25000,1)
(106,Kandan,24000,1)
(105,Mani,23000,4)
(104,Kumar,20000,4)
(109,Bala,17000,3)
(108,Ram,15000,2)
(103,Krishna,15000,3)
(110,suresh,12000,5)
(107,Manoj,10000,5)

grunt> filter_employee = filter order_employee by (id%2)!=0;

(101,Pandi,25000,1)
(105,Mani,23000,4)
(109,Bala,17000,3)
(103,Krishna,15000,3)
(107,Manoj,10000,5)

grunt> limit_employee = limit filter_employee 3;

(101,Pandi,25000,1)
(105,Mani,23000,4)
(109,Bala,17000,3)

grunt> top3_employee = foreach limit_employee generate id,name;

(101,Pandi)
(105,Mani)
(109,Bala)
******************************************************************************************************************

(d) List of employees (employee id and employee name) having entries in employee_expenses
file.

grunt> emp_det_expences = join emp_details by id , emp_expenses by id;

(101,Pandi,25000,1,101,200)
(102,Raj,30000,2,102,100)
(103,Krishna,15000,3,103,400)
(104,Kumar,20000,4,104,500)
(105,Mani,23000,4,105,200)
(106,Kandan,24000,1,106,100)
(107,Manoj,10000,5,107,200)
(108,Ram,15000,2,108,300)
(109,Bala,17000,3,109,300)
(110,suresh,12000,5,110,400)

grunt> order_exp = order emp_det_expences by expenses desc;

(104,Kumar,20000,4,104,500)
(110,suresh,12000,5,110,400)
(103,Krishna,15000,3,103,400)
(109,Bala,17000,3,109,300)
(108,Ram,15000,2,108,300)
(107,Manoj,10000,5,107,200)
(105,Mani,23000,4,105,200)
(101,Pandi,25000,1,101,200)
(106,Kandan,24000,1,106,100)
(102,Raj,30000,2,102,100)

grunt> limit_exp = limit order_exp 2;

(104,Kumar,20000,4,104,500)
(110,suresh,12000,5,110,400)

grunt> final_emp = foreach limit_exp generate $0,$1; 


(104,Kumar)
(110,suresh)
grunt> 
********************************************************************************************************************
(d) List of employees (employee id and employee name) having entries in employee_expenses
file
*******

grunt> join_emp = join emp_details by id,emp_expenses by id;

(101,Pandi,25000,1,101,200)
(102,Raj,30000,2,102,100)
(103,Krishna,15000,3,103,400)
(104,Kumar,20000,4,104,500)
(105,Mani,23000,4,105,200)
(106,Kandan,24000,1,106,100)
(107,Manoj,10000,5,107,200)
(108,Ram,15000,2,108,300)
(109,Bala,17000,3,109,300)
(110,suresh,12000,5,110,400)

grunt> emp_names = foreach join_emp generate $0,$1;

(101,Pandi)
(102,Raj)
(103,Krishna)
(104,Kumar)
(105,Mani)
(106,Kandan)
(107,Manoj)
(108,Ram)
(110,suresh)

grunt> names = distinct emp_names;

(101,Pandi)
(102,Raj)
(103,Krishna)
(104,Kumar)
(105,Mani)
(106,Kandan)
(107,Manoj)
(108,Ram)
(109,Bala)
(110,suresh)
**********************************************************************************************************************
(e) List of employees (employee id and employee name) having no entry in employee_expenses
file.

grunt> emp_det_expe = join emp_details by id left outer,emp_expenses by id;

(101,Pandi,25000,1,101,200)
(102,Raj,30000,2,102,100)
(103,Krishna,15000,3,103,400)
(104,Kumar,20000,4,104,500)
(105,Mani,23000,4,105,200)
(106,Kandan,24000,1,106,100)
(107,Manoj,10000,5,107,200)
(108,Ram,15000,2,108,300)
(109,Bala,17000,3,109,300)
(110,suresh,12000,5,110,400)

grunt> filter_exp = filter emp_det_expe by $4 is null and $5 is null;

grunt> gen_emp_det_expe = foreach emp_det_expe generate $0,$1;

grunt> limit_emp = limit gen_emp_det_expe 5;

(101,Pandi)
(102,Raj)
(103,Krishna)
(104,Kumar)
(105,Mani)
*************************************************************************************************************************
///*Create a database named 'custom'.
Create a table named temperature_data inside custom having below fields:
1. date (mm-dd-yyyy) format
2. zip code
3. temperature
The table will be loaded from comma-delimited file.///*


mysql> create database custom;

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| custom             |
| information_schema |
| metastore          |
| mysql              |


mysql> use custom;
Database changed
mysql>

mysql> create table temperature_data (date int,zipcode int,temperature int); 
Query OK, 0 rows affected (0.78 sec)

mysql>



#############################################Task 2###################################################

hive> Create table temperature_data(temp_date string,zipcode int,temperature int) row format delimited fields terminated by ',' lines terminated by '\n';

hive> load data local inpath '/home/acadgild/temperature_data' into table temperature_data;

hive> select * from temperature_data;
OK
temperature_data.temp_date	temperature_data.zipcode	temperature_data.temperature
10-01-1990	123112	10
14-02-1991	283901	11
10-03-1990	381920	15
10-01-1991	302918	22
12-02-1990	384902	9
10-01-1991	123112	11
14-02-1990	283901	12
10-03-1991	381920	16
10-01-1990	302918	23
12-02-1991	384902	10
10-01-1993	123112	11
14-02-1994	283901	12
10-03-1993	381920	16
10-01-1994	302918	23
12-02-1991	384902	10
10-01-1991	123112	11
14-02-1990	283901	12
10-03-1991	381920	16
10-01-1990	302918	23
12-02-1991	384902	10
Time taken: 0.813 seconds, Fetched: 20 row(s)
hive>

1)Fetch date and temperature from temperature_data where zipcode is greater than 300000 and less than 399999.

hive> 
    > select * from temperature_data where zipcode>300000 and zipcode<399999;
OK
temperature_data.temp_date	temperature_data.zipcode	temperature_data.temperature
10-03-1990	381920	15
10-01-1991	302918	22
12-02-1990	384902	9
10-03-1991	381920	16
10-01-1990	302918	23
12-02-1991	384902	10
10-03-1993	381920	16
10-01-1994	302918	23
12-02-1991	384902	10
10-03-1991	381920	16
10-01-1990	302918	23
12-02-1991	384902	10
Time taken: 1.996 seconds, Fetched: 12 row(s)
hive> 

2)Calculate Maximum temperature corresponding to every year from temperature_data table

hive> select year,MAX(temperature) from (select substring(temp_date,7) as year,temperature from temperature_data)t2 group by year;

Total MapReduce CPU Time Spent: 11 seconds 220 msec
OK
year	_c1
1990	23
1991	22
1993	16
1994	23
Time taken: 102.958 seconds, Fetched: 4 row(s)
hive>


3)Calculate maximum temperature from tempertaure_data table corresponding to those years which have at least 2 entries from the table 

hive> select count(year) as countOfYearGreaterThan2,MAX(temperature) from (select temp_date as year,temperature from temperature_data)t2 group by year having CountOfYearGreaterThan2>2;

Total MapReduce CPU Time Spent: 12 seconds 190 msec
OK
countofyeargreaterthan2	_c1
3	23
3	22
3	10
Time taken: 76.168 seconds, Fetched: 3 row(s)
hive> 


4)Creating Tables from the SUBQUERY
***********************************
hive> create table helo row format delimited fields terminated by ',' as select temp_date,MAX(temperature) from (select (temp_date,7) as temp_date from temperature_data group by temp_date)t3;

hive> select * from helo;

1990	23
1991	22
1993	16
1994	23
Time taken: 78.349 seconds, Fetched: 4 row(s)
hive> 
 


Creating view on subquery result tables
*****************************************

hive> create view temperature_view_view as select * from (select temp_date,MAX(temperature) from (select substring(temp_date,7) as temp_date,temperature from temperature_data)t2 group by temp_date)t3;

hive> select * from temperature_view_view;

Total MapReduce CPU Time Spent: 12 seconds 90 msec
OK
temperature_view_view.temp_date	temperature_view_view._c1
1990	23
1991	22
1993	16
1994	23
Time taken: 78.349 seconds, Fetched: 4 row(s)
hive> 

Export contents from temperature_data to file in local file system,such that each file is "|" delimited.
*********************************************************************************************************
hive> insert overwrite local directory '/home/acadgild/exported_data_hive/hive' select * from temperature_view_view;


