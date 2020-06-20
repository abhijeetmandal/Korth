# Korth
Database problems and solutions

3.11 Write 

a. All students who have taken at least one Comp. Sci. course; (distinct)

```SQL
SELECT student.ID
FROM student
where 1 <= (SELECT count(DISTINCT course_id)
           FROM takes NATURAL JOIN course
           WHERE course.dept_name='Comp. Sci.'
           and student.ID=takes.ID);
```           
           
b.            


d.

```SQL
SELECT * FROM instructor;

SELECT MAX(salary),dept_name, instructor.name 
FROM instructor
GROUP BY dept_name;

WITH max_sal_dept as (SELECT MAX(salary) as max_salary,dept_name
     FROM instructor
     GROUP BY dept_name)
SELECT instructor.salary,instructor.dept_name,instructor.name
FROM instructor,max_sal_dept
WHERE instructor.dept_name=max_sal_dept.dept_name
AND instructor.salary=max_sal_dept.max_salary
;

WITH max_sal_dept as (SELECT MAX(salary) as max_salary,dept_name
     FROM instructor
     GROUP BY dept_name)
SELECT *
FROM department,max_sal_dept
WHERE department.dept_name=max_sal_dept.dept_name
;

WITH max_sal_dept as (SELECT MAX(salary) as max_salary,dept_name
     FROM instructor
     GROUP BY dept_name),max_sal_per_dept as (SELECT department.dept_name,max_sal_dept.max_salary,instructor.name
												FROM department,max_sal_dept,instructor
												WHERE department.dept_name=max_sal_dept.dept_name
												and instructor.dept_name=department.dept_name
												and instructor.salary=max_sal_dept.max_salary)
select dept_name,max_salary
FROM
max_sal_per_dept
where max_salary= (select min(max_salary) FROM max_sal_per_dept)
;
```

3.12

SELECT student.ID, section.course_id, section.sec_id, section.semester, section.year
FROM student,section
WHERE student.dept_name='Comp. Sci.'
AND section.course_id='CS-001'
;

INSERT INTO takes (ID,course_id,sec_id,semester,year)
SELECT student.ID, section.course_id, section.sec_id, section.semester, section.year
FROM student,section
WHERE student.dept_name='Comp. Sci.'
AND section.course_id='CS-001'
;
