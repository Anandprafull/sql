drop table customers;
drop table orders;
drop table shippings;

CREATE TABLE EMPLOYEE (
    Fname VARCHAR(100),
    Lname VARCHAR(100),
    SSN INT PRIMARY KEY,
    Addrs VARCHAR(200),
    Sex CHAR(1),
    Salary DECIMAL(10, 2),
    SuperSSN INT,
    Dno INT
);

CREATE TABLE DEPARTMENT (
    Dname VARCHAR(100),
    Dnumber INT PRIMARY KEY,
    MgrSSN INT,
    MgrStartDate DATE,
    FOREIGN KEY (MgrSSN) REFERENCES EMPLOYEE(SSN)
);

CREATE TABLE DEPENDENT (
    Dname VARCHAR(100),
    ESSN INT,
    PRIMARY KEY (Dname, ESSN),
    FOREIGN KEY (ESSN) REFERENCES EMPLOYEE(SSN)
);

INSERT INTO EMPLOYEE (Fname, Lname, SSN, Addrs, Sex, Salary, SuperSSN, Dno)
VALUES
('Alice', 'Smith', 101, 'Bangalore', 'F', 60000, NULL, 1),
('Bob', 'Brown', 102, 'Mumbai', 'M', 75000, 101, 2),
('Charlie', 'Green', 103, 'Delhi', 'M', 80000, 102, 3),
('Diana', 'White', 104, 'Chennai', 'F', 50000, 101, 1),
('Eve', 'Black', 105, 'Kolkata', 'F', 90000, 102, 2);

INSERT INTO DEPARTMENT (Dname, Dnumber, MgrSSN, MgrStartDate)
VALUES
('HR', 1, 101, '2020-01-01'),
('Finance', 2, 102, '2021-01-01'),
('IT', 3, 103, '2022-01-01'),
('Sales', 4, 105, '2023-01-01'),
('Tech Support', 5, 104, '2024-01-01');

INSERT INTO DEPENDENT (Dname, ESSN)
VALUES
('Health Insurance', 101),
('Health Insurance', 102),
('Health Insurance', 103),
('Education Support', 104),
('Retirement Fund', 105);

SELECT Dname, AVG(Salary) AS avg_salary
FROM DEPARTMENT
JOIN EMPLOYEE ON DEPARTMENT.Dnumber = EMPLOYEE.Dno
GROUP BY Dname;

SELECT DISTINCT E.Fname, E.Lname
FROM EMPLOYEE E
JOIN DEPARTMENT D ON E.SSN = D.MgrSSN
JOIN DEPENDENT DEP ON E.SSN = DEP.ESSN;

SELECT *
FROM DEPARTMENT
WHERE Dname LIKE '%tech%';
