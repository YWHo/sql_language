SQL> **CREATE** TABLE IF NOT EXISTS books ( <br>
  BookID INT NOT NULL PRIMARY KEY AUTO_INCREMENT, <br>
  Title VARCHAR(100) NOT NULL, <br>
  SeriesID INT, <br>
  AuthorID INT);

SQL> **CREATE** TABLE IF NOT EXISTS authors <br>
(id INT NOT NULL PRIMARY KEY AUTO_INCREMENT);**

SQL> **CREATE** TABLE IF NOT EXISTS series <br>
(id INT NOT NULL PRIMARY KEY AUTO_INCREMENT);

SQL> **INSERT** INTO books (Title,SeriesID,AuthorID) <br>
  VALUES('The Fellowship of the Ring',1,1), <br>
  ('The Two Towers',1,1), ('The Return of the King',1,1),  <br>
  ('The Sum of All Men',2,2), ('Brotherhood of the Wolf',2,2), <br> 
  ('Wizardborn',2,2), ('The Hobbbit',0,1);**

SQL> **SHOW** TABLES;
```
+----------------+
| Tables_in_test |
+----------------+
| authors        |
| books          |
| series         |
+----------------+
```

SQL> **DESCRIBE** books;
```
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| BookID   | int(11)      | NO   | PRI | NULL    | auto_increment |
| Title    | varchar(100) | NO   |     | NULL    |                |
| SeriesID | int(11)      | YES  |     | NULL    |                |
| AuthorID | int(11)      | YES  |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
```