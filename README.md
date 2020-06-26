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

```SQL

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

DELETE FROM takes
WHERE takes.ID in (SELECT student.ID
FROM student
WHERE student.name='Chavez');

DELETE FROM
course
WHERE course_id='CS-001';

DELETE FROM takes
WHERE (takes.course_id,takes.sec_id,takes.semester,takes.year) =
(SELECT section.course_id,section.sec_id,section.semester,section.year
FROM course NATURAL JOIN section
WHERE lower(title) like '%database%');


CREATE TABLE person
		(driver_id	varchar(10),
         name		varchar(20) not null,
         address	varchar(50),
         PRIMARY KEY (driver_id)
         );
	 
	 
CREATE TABLE car
(license	varchar(10),
 model		varchar(10),
 year 		numeric(4,0) check (year>1701 and year<2100),
 PRIMARY KEY (license)
);


CREATE TABLE accident
(report_number	varchar(10),
 date date,
 location varchar(20),
 PRIMARY KEY (report_number)
);

CREATE TABLE owns
(driver_id	varchar(10),
 license	varchar(10),
 PRIMARY KEY (driver_id,license),
 FOREIGN KEY (driver_id) REFERENCES person(driver_id)
 						ON DELETE CASCADE,
 FOREIGN KEY (license) REFERENCES car(license)
 						ON DELETE CASCADE
)

CREATE TABLE participated
(report_number	varchar(10),
 license	varchar(10),
 driver_id	varchar(10),
 damage_amount numeric(12,2),
 PRIMARY KEY (report_number,license),
 FOREIGN KEY (report_number) REFERENCES accident(report_number)
 						ON DELETE CASCADE,
 FOREIGN KEY (license) REFERENCES car(license)
 						ON DELETE CASCADE,
 FOREIGN KEY (driver_id) REFERENCES person(driver_id)
 						ON DELETE SET NULL
)

```

4.9

```SQL

CREATE TABLE manager
	(employee_name varchar(20) not null,
     manager_name varchar(20) not null,
     PRIMARY KEY (employee_name),
     FOREIGN KEY (manager_name) REFERENCES manager(employee_name) 
     			ON DELETE CASCADE
    );

CREATE TABLE employee
	(employee_name varchar(20),
    street varchar(20),
    city varchar(20),
    PRIMARY KEY (employee_name)
    );

CREATE TABLE manages
	(employee_name varchar(20),
    manager_name varchar(20),
    PRIMARY KEY (employee_name),
    FOREIGN KEY (employee_name) REFERENCES employee(employee_name) ON DELETE CASCADE,
    FOREIGN KEY (manager_name) REFERENCES employee(employee_name) ON DELETE SET NULL)
 ;   
 
INSERT INTO employee VALUES ('Emp1', 'Street1', 'City1');
INSERT INTO employee VALUES ('Emp2', 'Street2', 'City2');
INSERT INTO employee VALUES ('Emp3', 'Street3', 'City3');
INSERT INTO employee VALUES ('Emp4', 'Street4', 'City4');


SELECT *
FROM employee;

SELECT *
FROM manages;

SELECT * 
FROM manages NATURAL RIGHT OUTER JOIN employee
where manages.manager_name is NULL;

SELECT * 
FROM employee
where employee.employee_name not in (SELECT manages.employee_name
                                    FROM manages
                                    WHERE manages.manager_name is NOT NULL and manages.employee_name=employee.employee_name)
                                    ;
                                    
                                    
SELECT * 
FROM employee
where NOT EXISTS (SELECT *
                  FROM manages
                  WHERE manages.manager_name is NOT NULL AND manages.employee_name=employee.employee_name)
                 ;                                    
		 

```
