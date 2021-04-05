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