# Getting Data

## Basic Elements

MariaDB [bookstore]> **SELECT** * **FROM** books;
```
+----------------+-------------------+-----------+--------------+----------+-------------+
| isbn           | title             | author_id | publisher_id | year_pub | description |
+----------------+-------------------+-----------+--------------+----------+-------------+
| 0553213695     | The Metamorphosis |         1 |         NULL | 1995     | NULL        |
| 0805210407     | The Trial         |         1 |         NULL | 1995     | NULL        |
| 0805210644     | Amerika           |         1 |         NULL | 1995     | NULL        |
| 0805211063     | The Castle        |         1 |         NULL | 1998     | NULL        |
| 9781412185714  | Hamlet            |         2 |         NULL | 2003     | NULL        |
| 9781505259568  | Romeo and Juliet  |         2 |         NULL | 2014     | NULL        |
| 99789875506558 | Otello            |         2 |         NULL | 2006     | NULL        |
+----------------+-------------------+-----------+--------------+----------+-------------+
```

MariaDB [bookstore]> **SELECT** * **FROM** authors;
```
+-----------+-------------+------------+----------------+
| author_id | name_last   | name_first | country        |
+-----------+-------------+------------+----------------+
|         1 | Kafka       | Franz      | Czech Republic |
|         2 | Shakespeare | William    | United Kingdom |
+-----------+-------------+------------+----------------+
```

MariaDB [bookstore]> **SELECT** isbn, title, author_id<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books;
```
+----------------+-------------------+-----------+
| isbn           | title             | author_id |
+----------------+-------------------+-----------+
| 0553213695     | The Metamorphosis |         1 |
| 0805210407     | The Trial         |         1 |
| 0805210644     | Amerika           |         1 |
| 0805211063     | The Castle        |         1 |
| 9781412185714  | Hamlet            |         2 |
| 9781505259568  | Romeo and Juliet  |         2 |
| 99789875506558 | Otello            |         2 |
+----------------+-------------------+-----------+
```

MariaDB [bookstore]> **SELECT** isbn, title, author_id<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **LIMIT** 2;
```
+------------+-------------------+-----------+
| isbn       | title             | author_id |
+------------+-------------------+-----------+
| 0553213695 | The Metamorphosis |         1 |
| 0805210407 | The Trial         |         1 |
+------------+-------------------+-----------+
```

MariaDB [bookstore]> **SELECT** isbn, title, author_id<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **LIMIT** 2, 2;
```
+------------+------------+-----------+
| isbn       | title      | author_id |
+------------+------------+-----------+
| 0805210644 | Amerika    |         1 |
| 0805211063 | The Castle |         1 |
+------------+------------+-----------+
```

---
## Selectivity and Order

MariaDB [bookstore]> **SELECT** isbn, title<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** author_id = 1<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **LIMIT** 3;<br>
```
+------------+-------------------+
| isbn       | title             |
+------------+-------------------+
| 0553213695 | The Metamorphosis |
| 0805210407 | The Trial         |
| 0805210644 | Amerika           |
+------------+-------------------+
```

MariaDB [bookstore]> **SELECT** isbn, title<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** author_id = 1<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **ORDER BY** title **ASC**<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **LIMIT** 3;
```
+------------+-------------------+
| isbn       | title             |
+------------+-------------------+
| 0805210644 | Amerika           |
| 0805211063 | The Castle        |
| 0553213695 | The Metamorphosis |
+------------+-------------------+
```

MariaDB [bookstore]> **SELECT** isbn, title<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** author_id = 1<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **ORDER BY** isbn **DESC**;
```
+------------+-------------------+
| isbn       | title             |
+------------+-------------------+
| 0805211063 | The Castle        |
| 0805210644 | Amerika           |
| 0805210407 | The Trial         |
| 0553213695 | The Metamorphosis |
+------------+-------------------+
```

---
## Friendlier and More Complicated

MariaDB [bookstore]> **SELECT** isbn, title,<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **CONCAT**(name_first, ' ', name_last) **AS** author<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **JOIN** authors **USING** (author_id)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** name_last = 'Kafka'<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **ORDER BY** title **ASC**<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **LIMIT** 3;
```
+------------+-------------------+-------------+
| isbn       | title             | author      |
+------------+-------------------+-------------+
| 0805210644 | Amerika           | Franz Kafka |
| 0805211063 | The Castle        | Franz Kafka |
| 0553213695 | The Metamorphosis | Franz Kafka |
+------------+-------------------+-------------+
```

MariaDB [bookstore]> **SELECT** b.isbn, b.title,<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **CONCAT**(a.name_first, ' ', a.name_last) **AS** author<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books b<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **JOIN** authors a **ON** (b.author_id = a.author_id)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** name_last = 'Kafka'<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **ORDER BY** title **ASC**<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **LIMIT** 3;
```
+------------+-------------------+-------------+
| isbn       | title             | author      |
+------------+-------------------+-------------+
| 0805210644 | Amerika           | Franz Kafka |
| 0805211063 | The Castle        | Franz Kafka |
| 0553213695 | The Metamorphosis | Franz Kafka |
+------------+-------------------+-------------+
```

MariaDB [bookstore]> **SELECT** isbn, title,<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **CONCAT**(name_first, ' ', name_last) **AS** author<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **JOIN** authors **USING** (author_id)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** name_last **LIKE** '%pear%'<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **ORDER** by title **DESC**<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **LIMIT** 2;
```
+----------------+------------------+---------------------+
| isbn           | title            | author              |
+----------------+------------------+---------------------+
| 9781505259568  | Romeo and Juliet | William Shakespeare |
| 99789875506558 | Otello           | William Shakespeare |
+----------------+------------------+---------------------+
```