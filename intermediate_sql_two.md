## Intermediate SQL Practice Two

* Run `psql`
* You may need to run `CREATE DATABASE intermediate_sql;`
* To exit this shell, you can run `CTRL` `d`

#### Create tables

Let's create some tables in the database.
```
CREATE TABLE students(id SERIAL, name TEXT);
CREATE TABLE classes(id SERIAL, name TEXT, teacher_id INT);
CREATE TABLE teachers(id SERIAL, name TEXT, room_number INT);
CREATE TABLE enrollments(id SERIAL, student_id INT, class_id INT, grade INT);
```

#### Add data

Let's insert some students.

```
INSERT INTO students (name)
VALUES ('Penelope'),
       ('Peter'),
       ('Pepe'),
       ('Parth'),
       ('Priscilla'),
       ('Pablo'),
       ('Puja'),
       ('Patricia'),
       ('Piper'),
       ('Paula'),
       ('Pamela'),
       ('Paige'),
       ('Peggy'),
       ('Pedro'),
       ('Phoebe'),
       ('Pajak'),
       ('Parker'),
       ('Priyal'),
       ('Paxton'),
       ('Patrick');
```

Let's add some teachers.

```
INSERT INTO teachers (name, room_number)
VALUES ('Phillips', 456),
       ('Vandergrift', 120),
       ('Mauch', 101),
       ('Patel', 320),
       ('Marquez', 560),
       ('Boykin', 200),
       ('Phlop', 333),
       ('Pendergrass', 222),
       ('Palomo', 323),
       ('Altshuler', 543),
       ('Aleman', 187),
       ('Ashley', 432),
       ('Bonacci', 399),
       ('Brazukas', 287),
       ('Brockington', 299),
       ('Brizuela', 376),
       ('Burkhart', 199),
       ('Choi', 463),
       ('Shah', 354),
       ('Dimaggio', 251);
```

Let's add some classes.

```
INSERT INTO classes (name, teacher_id)
VALUES ('Cooking Pasta', 1),
       ('Yoga', 1),
       ('How to Guitar', 2),
       ('Gym', 3),
       ('Football', 4),
       ('Calculus', 5),
       ('Fruit', 6),
       ('Social Studies', 7),
       ('English', 8),
       ('Programming', 9),
       ('Singing', 10),
       ('Fashion', 11);
```

Lastly, let's add some enrollments!

```
INSERT INTO enrollments (student_id, class_id, grade)
VALUES (1, 1, 60),
       (2, 2, 70),
       (2, 4, 100),
       (3, 2, 74),
       (4, 3, 82),
       (5, 3, 45),
       (5, 4, 50),
       (7, 11, 62),
       (7, 10, 76),
       (7, 9, 81),
       (7, 8, 91),
       (8, 8, 84),
       (9, 8, 88),
       (9, 7, 83),
       (10, 7, 93),
       (10, 5, 95),
       (11, 5, 95),
       (11, 11, 80),
       (11, 6, 95),
       (11, 1, 94),
       (11, 2, 60),
       (12, 6, 55),
       (13, 7, 97),
       (14, 10, 86),
       (15, 9, 77),
       (15, 4, 93),
       (15, 1, 73),
       (16, 2, 79),
       (16, 6, 73),
       (17, 7, 86),
       (17, 8, 91),
       (17, 9, 93),
       (18, 10, 94),
       (19, 4, 84),
       (20, 1, 85),
       (20, 11, 89),
       (20, 3, 98);
```

### Practice!!

* List all the students and their classes
```sql
SELECT students.name, classes.name FROM enrollments INNER JOIN students ON enrollments.student_id = students.id INNER JOIN classes ON enrollments.class_id = classes.id;
```
* List all the students and their classes and rename the columns to "student" and "class"
```sql
SELECT students.name as student, classes.name as classes FROM enrollments INNER JOIN students ON enrollments.student_id = students.id INNER JOIN classes ON enrollments.class_id = classes.id;
```
* List all the students and their average grade
```sql
SELECT students.name, ROUND(AVG(grade),2) as average_grade FROM students INNER JOIN enrollments ON students.id = enrollments.student_id GROUP BY students.name ORDER BY average_grade;
```
* List all the students and a count of how many classes they are currently enrolled in
```sql
SELECT students.name, COUNT(classes.name) as number_of_classes
FROM enrollments
INNER JOIN students ON students.id = enrollments.student_id
INNER JOIN classes ON classes.id = enrollments.class_id
GROUP BY students.name
ORDER BY number_of_classes;
```
* List all the students and their class count IF they are in more than 2 classes
```sql
SELECT students.name, COUNT(classes.name) as number_of_classes
FROM enrollments
INNER JOIN students ON students.id = enrollments.student_id
INNER JOIN classes ON classes.id = enrollments.class_id
GROUP BY students.name
HAVING COUNT(classes.name) > 2;
```
* List all the teachers for each student
```sql
SELECT teachers.name as teacher, students.name as student
FROM enrollments
INNER JOIN students ON students.id = enrollments.student_id
INNER JOIN classes ON classes.id = enrollments.class_id
INNER JOIN teachers ON teachers.id = classes.id
GROUP BY students.name, teachers.name;
```
* List all the teachers for each student grouped by each student
```sql
SELECT teachers.name as teacher, students.name as student
FROM enrollments
INNER JOIN students ON students.id = enrollments.student_id
INNER JOIN classes ON classes.id = enrollments.class_id
INNER JOIN teachers ON teachers.id = classes.id
GROUP BY students.name, teachers.name
ORDER BY students.name;

```
* Find the average grade for a each class
```sql
SELECT classes.name as class, ROUND(avg(grade),2) as average_grade
FROM enrollments
INNER JOIN classes ON classes.id = enrollments.class_id
GROUP BY classes.name
ORDER BY average_grade;
```
* List students' name and their grade IF their grade is lower than the average.
```sql
SELECT students.name, classes.name, enrollments.grade
FROM students
JOIN enrollments ON students.id = enrollments.student_id
JOIN classes ON classes.id = enrollments.class_id
WHERE enrollments.grade < (SELECT ROUND(AVG(grade),2 FROM students JOIN enrollments ON students.id = enrollments.student_id);
```
