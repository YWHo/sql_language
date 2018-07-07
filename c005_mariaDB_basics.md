# Basics

## Connecting to MariaDB

% mysql -u root -p -h localhost

(Note: Just pressed enter when asked for password)

---
## Creating a Structure

MariaDB [(none)]> **CREATE DATABASE** bookstore;

MariaDB [(none)]> **USE** bookstore;

MariaDB [bookstore]> **CREATE TABLE** books (<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> isbn CHAR(20) PRIMARY KEY,<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> title VARCHAR(50),<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> author_id INT,<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> publisher_id INT,<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> year_pub CHAR(4),<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> description TEXT );

MariaDB [bookstore]> **DESCRIBE** books;
```
+--------------+-------------+------+-----+---------+-------+
| Field        | Type        | Null | Key | Default | Extra |
+--------------+-------------+------+-----+---------+-------+
| isbn         | char(20)    | NO   | PRI | NULL    |       |
| title        | varchar(50) | YES  |     | NULL    |       |
| author_id    | int(11)     | YES  |     | NULL    |       |
| publisher_id | int(11)     | YES  |     | NULL    |       |
| year_pub     | char(4)     | YES  |     | NULL    |       |
| description  | text        | YES  |     | NULL    |       |
+--------------+-------------+------+-----+---------+-------+
```

MariaDB [bookstore]> **CREATE TABLE** authors<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> (author_id INT AUTO_INCREMENT PRIMARY KEY,<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> name_last VARCHAR(50),<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> name_first VARCHAR(50),<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> country VARCHAR(50) );

---
## Entering Data

MariaDB [bookstore]> **INSERT INTO** authors<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> (name_last, name_first, country)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **VALUES**('Kafka', 'Franz', 'Czech Republic');

MariaDB [bookstore]> **INSERT INTO** authors<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> (name_last, name_first, country)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **VALUES** ('Shakespeare', 'William', 'United Kingdom');

MariaDB [bookstore]> **INSERT INTO** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> (title, author_id, isbn, year_pub)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **VALUES**('The Castle', '1', '0805211063', '1998');<br>

MariaDB [bookstore]> **INSERT INTO** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> (title, author_id, isbn, year_pub)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **VALUES**('The Trial', '1', '0805210407', '1995'),<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> ('The Metamorphosis', '1', '0553213695', '1995'),<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> ('America', '1', '0805210644', '1995');

MariaDB [bookstore]> **INSERT INTO** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> (title, author_id, isbn, year_pub)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **VALUES** ('Romeo and Juliet', 2, 9781505259568, 2014);

---
## Retrieving Data

MariaDB [bookstore]> **SELECT** title **FROM** books;
```
+-------------------+
| title             |
+-------------------+
| The Metamorphosis |
| The Trial         |
| America           |
| The Castle        |
| Romeo and Juliet  |
+-------------------+
```

MariaDB [bookstore]> **SELECT** title<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **LIMIT** 3;
 ```   
+-------------------+
| title             |
+-------------------+
| The Metamorphosis |
| The Trial         |
| America           |
+-------------------+
```

MariaDB [bookstore]> **SELECT** title, name_last<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **JOIN** authors **USING** (author_id);
```
+-------------------+-------------+
| title             | name_last   |
+-------------------+-------------+
| The Metamorphosis | Kafka       |
| The Trial         | Kafka       |
| America           | Kafka       |
| The Castle        | Kafka       |
| Romeo and Juliet  | Shakespeare |
+-------------------+-------------+
```

MariaDB [bookstore]> **SELECT** title **AS** 'Kafka Books'<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **JOIN** authors **USING** (author_id)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** name_last = 'Kafka';<br>
```
+-------------------+
| Kafka Books       |
+-------------------+
| The Metamorphosis |
| The Trial         |
| America           |
| The Castle        |
+-------------------+
```

---
## Changing Data

MariaDB [bookstore]> **UPDATE** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **SET** title = 'Amerika'<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** isbn = '0805210644';

MariaDB [bookstore]> **SELECT** title AS 'Kafka Books', isbn<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **ROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **JOIN** authors **USING** (author_id)<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** name_last = 'Kafka';
```
+-------------------+------------+
| Kafka Books       | isbn       |
+-------------------+------------+
| The Metamorphosis | 0553213695 |
| The Trial         | 0805210407 |
| Amerika           | 0805210644 |
| The Castle        | 0805211063 |
+-------------------+------------+
```

---
## Delete Data

MariaDB [bookstore]> **DELETE FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **WHERE** author_id = '2';

MariaDB [bookstore]> **SELECT** title, name_last<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **FROM** books<br>
&nbsp;&nbsp;&nbsp;&nbsp;-> **JOIN** authors **USING** (author_id);
```
+-------------------+-----------+
| title             | name_last |
+-------------------+-----------+
| The Metamorphosis | Kafka     |
| The Trial         | Kafka     |
| Amerika           | Kafka     |
| The Castle        | Kafka     |
+-------------------+-----------+
```