# Oracle Built-in Function/Operator

## Overview




The Motive of the testing/googling is to identify that Oracle Build-In function (apart from orafce) and operator which are using by Spacewalk are Present/Not Present in PG

The minimum supported version of Postgres is PG 8.1+.
### About Oracle Built-In Function



Oracle built-function:    Oracle functions http://ist.marshall.edu/ist466/functions.html

Oracle Build in Operator: Other sql/plsql  http://www.techonthenet.com/oracle/index.php       
### Oracle Function using in Spacewalk available in Postgres




| *Function Name* |
| --- |


| ASCII |
| --- |
| AVG |
| MAX |
| COUNT |
| CHR |
| CAST |
| TO_NUMBER |
| GREATEST |
| MOD |
| SUM |
| UPPER |
| LPAD |
### Oracle Operator using in Spacewalk available in Postgres




| *OPERATOR NAME* |
| --- |


| UNION |
| --- |
| UNION ALL |
| EXISTS |
### Oracle Function using in Spacewalk *Not* available in Postgres




| *Function Name* |
| --- |


| ROWID |
| --- |
| ROWNUM |
### Oracle Operator using in Spacewalk *Not* available in Postgres





| *OPERATOR NAME* |
| --- |


| MULITSET |
| --- |
| MINUS |
### Oracle Data Dictionary table using in Spacewalk *Not* available in Postgres




| *DATA DICTIONARY TABLE* |
| --- |


| USER_CONSTRAINTS |
| --- |
| ALL_CONS_COLUMNS |
| ALL_CONSTRAINTS |
### Oracle pl/sql data type using in Spacewalk *Not* available in Postgres




| *Pl/sql data type* |
| --- |


| BINARY_INTEGER |
| --- |
### Observation



*Apart from all above theses,Didn't find any other Function Which are using/not using in Spacewalk.

*Oracle and Postgres Built-in function/operator which are using in Spacewalk are Compatible.

*Spacewalk is using these function's But *NOT* in a great Extend. 
