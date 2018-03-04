
## Intermediate SQL Practice

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
SELECT
```
