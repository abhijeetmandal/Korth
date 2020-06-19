# Korth
Database problems and solutions

3.11 Write 

a. All students who have taken at least one Comp. Sci. course; (distinct)

SELECT student.ID
FROM student
where 1 <= (SELECT count(DISTINCT course_id)
           FROM takes NATURAL JOIN course
           WHERE course.dept_name='Comp. Sci.'
           and student.ID=takes.ID);
           
b.            
