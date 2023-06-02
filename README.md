mysql> CREATE TABLE Movies (
    ->     Title VARCHAR(100),
    ->     Runtime INT,
    ->     Genre VARCHAR(50),
    ->     IMDB_Score DECIMAL(3, 1),
    ->     Rating VARCHAR(10)
    -> );
Query OK, 0 rows affected (0.04 sec)

mysql>
mysql> INSERT INTO Movies (Title, Runtime, Genre, IMDB_Score, Rating)
    -> VALUES
    ->     ('Howard the Duck', 110, 'Sci-Fi', 4.6, 'PG'),
    ->     ('Lavalantula', 83, 'Horror', 4.7, 'TV-14'),
    ->     ('Starship Troopers', 129, 'Sci-Fi', 7.2, 'PG-13'),
    ->     ('Waltz With Bashir', 90, 'Documentary', 8.0, 'R'),
    ->     ('Spaceballs', 96, 'Comedy', 7.1, 'PG'),
    ->     ('Monsters Inc.', 92, 'Animation', 8.1, 'G');
Query OK, 6 rows affected (0.02 sec)
Records: 6  Duplicates: 0  Warnings: 0

mysql> INSERT INTO Movies (Title, Runtime, Genre, IMDB_Score, Rating)
    -> VALUES
    -> ('Interstellar', 140, 'REAL', 10.0, 'MUST WATCH'),
    -> ('Tokyo Drift', 135, 'Reality TV', 'Eh')
    -> ('The Dark Knight', 152, 'Action', 9.0, 'Real');
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '('The Dark Knight', 152, 'Action', 9.0, 'Real')' at line 5
mysql> INSERT INTO Movies (Title, Runtime, Genre, IMDB_Score, Rating)
    -> VALUES
    ->     ('Interstellar', 140, 'REAL', 10.0, 'MUST WATCH'),
    ->     ('Tokyo Drift', 135, 'Reality TV', NULL, 'Eh'),
    ->     ('The Dark Knight', 152, 'Action', 9.0, 'Real');
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> SELECT *
    -> FROM Movies
    -> WHERE Genre = 'Sci-Fi';
+-------------------+---------+--------+------------+--------+
| Title             | Runtime | Genre  | IMDB_Score | Rating |
+-------------------+---------+--------+------------+--------+
| Howard the Duck   |     110 | Sci-Fi |        4.6 | PG     |
| Starship Troopers |     129 | Sci-Fi |        7.2 | PG-13  |
+-------------------+---------+--------+------------+--------+
2 rows in set (0.02 sec)

mysql> SELECT *
    -> FROM Movies
    -> WHERE IMDB_Score > 6.5;
+-------------------+---------+-------------+------------+------------+
| Title             | Runtime | Genre       | IMDB_Score | Rating     |
+-------------------+---------+-------------+------------+------------+
| Starship Troopers |     129 | Sci-Fi      |        7.2 | PG-13      |
| Waltz With Bashir |      90 | Documentary |        8.0 | R          |
| Spaceballs        |      96 | Comedy      |        7.1 | PG         |
| Monsters Inc.     |      92 | Animation   |        8.1 | G          |
| Interstellar      |     140 | REAL        |       10.0 | MUST WATCH |
| The Dark Knight   |     152 | Action      |        9.0 | Real       |
+-------------------+---------+-------------+------------+------------+
6 rows in set (0.01 sec)

mysql> SELECT *
    -> FROM Movies
    -> WHERE Rating IN ('G', 'PG') AND Runtime < 100;
+---------------+---------+-----------+------------+--------+
| Title         | Runtime | Genre     | IMDB_Score | Rating |
+---------------+---------+-----------+------------+--------+
| Spaceballs    |      96 | Comedy    |        7.1 | PG     |
| Monsters Inc. |      92 | Animation |        8.1 | G      |
+---------------+---------+-----------+------------+--------+
2 rows in set (0.03 sec)

mysql> SELECT Genre, AVG (Runtime) AS AverageRuntime
    -> FROM Movies
    -> WHERE IMBD_Score < 7.5
    -> GROUP BY Genre;
ERROR 1054 (42S22): Unknown column 'IMBD_Score' in 'where clause'
mysql> SELECT Genre, AVG(Runtime) AS AverageRuntime
    -> FROM Movies
    -> WHERE IMDB_Score < 7.5
    -> GROUP BY Genre;
+--------+----------------+
| Genre  | AverageRuntime |
+--------+----------------+
| Sci-Fi |       119.5000 |
| Horror |        83.0000 |
| Comedy |        96.0000 |
+--------+----------------+
3 rows in set (0.01 sec)

mysql> UPDATE Movies
    -> SET Rating = 'R'
    -> WHERE Title = 'Starship Troopers';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> SELECT SELECT ID, Rating
    -> FROM Movies
    -> WHERE Genre IN ('Horror', 'Documentary');
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'SELECT ID, Rating
FROM Movies
WHERE Genre IN ('Horror', 'Documentary')' at line 1
mysql> ALTER TABLE Movies
    -> ADD ID INT NULL;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> SELECT ID, Rating
    -> FROM Movies
    -> WHERE Genre IN ('Horror', 'Documentary');
+------+--------+
| ID   | Rating |
+------+--------+
| NULL | TV-14  |
| NULL | R      |
+------+--------+
2 rows in set (0.00 sec)

mysql> SELECT Rating, AVG(IMDB_Score) AS AverageIMDBScore, MAX(IMDB_Score) AS MaximumIMDBScore, MIN(IMDB_Score) AS MinimumIMDBScore
    -> FROM Movies
    -> GROUP BY Rating;
+------------+------------------+------------------+------------------+
| Rating     | AverageIMDBScore | MaximumIMDBScore | MinimumIMDBScore |
+------------+------------------+------------------+------------------+
| PG         |          5.85000 |              7.1 |              4.6 |
| TV-14      |          4.70000 |              4.7 |              4.7 |
| R          |          7.60000 |              8.0 |              7.2 |
| G          |          8.10000 |              8.1 |              8.1 |
| MUST WATCH |         10.00000 |             10.0 |             10.0 |
| Eh         |             NULL |             NULL |             NULL |
| Real       |          9.00000 |              9.0 |              9.0 |
+------------+------------------+------------------+------------------+
7 rows in set (0.00 sec)

mysql> SELECT Rating, AVG(IMDB_Score) AS AverageIMDBScore, MAX(IMDB_Score) AS MaximumIMDBScore, MIN(IMDB_Score) AS MinimumIMDBScore
    -> FROM Movies
    -> GROUP BY Rating
    -> HAVING COUNT(*) > 1;
+--------+------------------+------------------+------------------+
| Rating | AverageIMDBScore | MaximumIMDBScore | MinimumIMDBScore |
+--------+------------------+------------------+------------------+
| PG     |          5.85000 |              7.1 |              4.6 |
| R      |          7.60000 |              8.0 |              7.2 |
+--------+------------------+------------------+------------------+
2 rows in set (0.00 sec)

mysql>
