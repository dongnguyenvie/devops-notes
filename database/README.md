```
SELECT m.movie_id, m.movie_title AS title, m.genre_id
FROM `movies` AS m

# WHERE m.movie_id LIKE "%n%"

# LIMIT 2

# WHERE m.year in (1986, 1997)

WHERE m.genre_id in (SELECT genre_id FROM genres AS g WHERE g.genre_title = "Fantasy") GROUP BY movie_id DESC
------
INSERT
INTO `movies`(`movie_title`, `director`, `year`, `genre_id`) 
VALUES 
('3000 days of love', 'nolan', 2000, (SELECT genres.genre_id FROM genres WHERE genres.genre_title = 'Comedy' LIMIT 1))
-----
UPDATE 
`movies` 
SET 
`director`='NOLAN',
`movie_title`= (SELECT genres.genre_id FROM genres WHERE genres.genre_title='Action' LIMIT 1)
WHERE movie_title = 'Contact'
-----
(// the same put and patch)
(this one is similar update command but it's a bit different)
REPLACE
INTO `people` (`email`, `first_name`, `last_name`, `common_name`)
VALUES ("join@snow.org", "Join", "Snow", "King join the firts")
-----
>> BAD: because it query all of record and filter it by where
SELECT m.movie_id, m.director, g.genre_title
FROM movies as m, genres as g
WHERE m.genre_id = g.genre_id
>> GOOD: When running any query, your objective is to make it efficient. To make a query run faster, you want to reduce the amount of data it has to process as early as possible. JOINs happen before filtering with WHERE. You could potentially reduce the amount of data for your WHERE filter by 80 or 90% by doing a JOIN first and throwing away records that don't need to be filtered. The JOIN is used when SELECTing the data. It means you only get the records that you want at the end.
If you skip the join and only use a WHERE filter, then your initial SELECT has to get ALL the data in both tables before the filter takes place.
SELECT m.movie_id, m.director, g.genre_title
FROM movies as m INNER JOIN genres as g
ON m.genre_id = g.genre_id
----- count record according to director
SELECT COUNT(movies.movie_id) as total, movies.director as dir
FROM movies
GROUP BY dir
----- UNION 
>> UNION: mixins both select query
SELECT movies.movie_id as id, movie_title as title
FROM movies
UNION
SELECT shows.initial_year as id, shows.show_title as title
FROM shows
----- BETWEEN
>>>
SELECT movie_title, year
FROM movies
WHERE year >= 1982 and year <= 1995
>>>
SELECT movie_title, year
FROM movies
WHERE year BETWEEN 1982 and 1995
----- IN
>>>
SELECT movie_title, year
FROM movies
WHERE movie_id = 1 or movie_id = 2
>>>
SELECT movie_title, year
FROM movies
WHERE movie_id in (1, 2)
---
CREATE FULLTEXT INDEX id_txt ON `shows` (show_title);
-----
SELECT b.name as b_name, c.name as c_name, c.id, b.best_friend
FROM b LEFT JOIN c
ON c.id = b.best_friend;

SELECT b.name as b_name, c.name as c_name, c.id, b.best_friend
FROM b RIGHT JOIN c
ON c.id = b.best_friend;

SELECT b.name as b_name, c.name as c_name, c.id, b.best_friend
FROM b INNER JOIN c
ON c.id = b.best_friend
---- 
SELECT p.productName, pl.productLine, c.customerName 
FROM productlines AS pl INNER JOIN products AS p 
ON pl.productLine = p.productLine 
INNER JOIN orderdetails AS od 
USING (productCode) 
INNER JOIN orders AS o 
USING (orderNumber) 
INNER JOIN customers AS c 
USING (customerNumber) 
INNER JOIN employees AS e 
ON c.salesRepEmployeeNumber = e.employeeNumber 
INNER JOIN offices AS off 
USING (officeCode) 
WHERE pl.productLine = 'Planes' AND off.city = 'Sydney';
---- many to many
>> actors | movies | actors_movies
select m.name
from movies m
inner join actors_movies am on m.id = am.movie_id
inner join actors a on am.actor_id = a.id
where a.name = 'Christopher Walken'
```

### Aggregate Functions
```
SELECT COUNT(movies.movie_title) as num
FROM movies
---
SELECT SUM(movies.year) as num
FROM movies
--- average
SELECT AVG(movies.year) as num
FROM movies
---
MIN, MAX, ...
https://dev.mysql.com/doc/refman/5.7/en/aggregate-functions.html
```

### String Functions
```
SELECT UPPER(movies.movie_title), LOWER(movies.director), CONCAT(movies.movie_title, "---", movies.director) AS full_info
FROM movies

https://dev.mysql.com/doc/refman/5.7/en/string-functions.html
```

### Numeric Functions
```
SELECT ROUND(RAND() * 10) as num
FROM movies
https://dev.mysql.com/doc/refman/5.7/en/mathematical-functions.html#
```

### Numeric DataTypes
```
https://dev.mysql.com/doc/refman/5.7/en/numeric-types.html
```

###  DataTypes for Time and Dates
```
https://dev.mysql.com/doc/refman/5.7/en/date-and-time-types.html
```

### String and Binary Datatypes
```
https://dev.mysql.com/doc/refman/5.7/en/string-types.html
```

### 
```
DROP TABLE IF EXISTS `movies`

```

### database command
```
SHOW DATABASES
SHOW TABLES FROM movies
SHOW COLUMNS FROM movies.shows
DESCRIBE movies
EXPLAIN SELECT * FROM movies.shows WHERE show_title LIKE "%a%"
SELECT [system_var] >> SELECT @@basedir
----
SET @nolan = "nolan_name";
SELECT @nolan as name
----
SET @ids = (SELECT movies.movie_title FROM movies LIMIT 1);
SELECT @ids;
----
USE, ...
SHOW https://dev.mysql.com/doc/refman/5.7/en/show.html
DESCRIBE https://dev.mysql.com/doc/refman/5.7/en/describe.html
EXPLAIN https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain-output-columns
User defined variable https://dev.mysql.com/doc/refman/5.7/en/user-variables.html
```

### CREATE INDEX
```
https://dev.mysql.com/doc/refman/5.7/en/create-index.html
```

```
-> Với 1 ứng dụng có tần suất đọc cao như trang tin tức,blog... thì bạn nên dùng MyISAM. -> Với ứng dụng có tần suất insert và update cao như: Diễn đàn, mạng xã hội.. thì bạn nên dùng InnoDB -> Bạn nên dùng MEMORY Storage Engine cho các table chứa dữ liệu tạm và thông tin phiên làm việc của người dùng (Session) -> Việc chuyển đổi 1 table từ storage engine này sang storage engine khác sẽ diễn ra tương đối lâu nếu dữ liệu trên table lớn. Do đó cần kiên nhẫn.

```

``` FOREIGN KEY
ALTER TABLE characters DROP FOREIGN KEY `fk_character_race`; 
DROP TABLE IF EXISTS races;
DROP TABLE IF EXISTS characters;

CREATE TABLE races (
   race_id TINYINT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
   race_name VARCHAR(30) NOT NULL
)ENGINE=INNODB;

CREATE TABLE characters(
    character_id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY Key,
    character_name VARCHAR(50) NOT NULL,
    race_id TINYINT UNSIGNED NOT NULL, 
    INDEX `idx_race`(race_id),
    CONSTRAINT `fk_character_race`
    FOREIGN KEY (race_id)
    REFERENCES races(race_id) ON UPDATE CASCADE ON DELETE RESTRICT
)ENGINE=INNODB;
```

### datetime-queries.sql
```
-- '2019-05-25 13:00:10.123456'  -- DATETIME
-- '2018-05-26 11:00:10.133333'
-- '2019-04-01'  -- DATE
-- '2019-02-03'  
-- '09:20:00.123456' -- TIME
-- '15:20:00.123456'
-- 1559335502  -- TIMESTAMP

 SELECT UNIX_TIMESTAMP('2019-05-25 13:00:10.123456'), UNIX_TIMESTAMP(), NOW(), SYSDATE(6);

 SELECT ADDDATE('2019-04-01', INTERVAL 3 DAY);  -- ADDTIME()
 SELECT SUBDATE('2019-04-01', INTERVAL 3 DAY);  -- SUBTIME()
 SELECT DATEDIFF('2019-04-01', '2019-02-03'), DATEDIFF('2019-02-03', '2019-04-01'); -- TIMEDIFF()

 SELECT TIMESTAMPDIFF(MONTH, '2019-04-01', '2019-02-01'), TIMESTAMPDIFF(MINUTE, '2019-05-25 13:00:10.123456', '2018-05-26 11:00:10.133333');

 SELECT MAKETIME(14, 22, 1);

 SELECT LAST_DAY('2019-02-03');

 SELECT GET_FORMAT(DATETIME, 'USA'), GET_FORMAT(DATETIME, 'EUR'), GET_FORMAT(DATETIME, 'ISO');
      
 SELECT DATE_FORMAT('2019-05-25 13:00:10.123456', '%Y-%m-%d %H:%i:%s');

 https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html
 https://dev.mysql.com/doc/refman/5.7/en/create-view.html
```

https://dev.mysql.com/doc/refman/5.7/en/create-function.html
