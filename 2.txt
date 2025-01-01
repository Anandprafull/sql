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

CREATE TABLE PROJECT (
    Pno INT PRIMARY KEY,
    Pname VARCHAR(100),
    Dnum INT,
    FOREIGN KEY (Dnum) REFERENCES DEPARTMENT(Dnumber)
);

CREATE TABLE WORKS_ON (
    ESSN INT,
    Pno INT,
    Hours DECIMAL(5, 2),
    PRIMARY KEY (ESSN, Pno),
    FOREIGN KEY (ESSN) REFERENCES EMPLOYEE(SSN),
    FOREIGN KEY (Pno) REFERENCES PROJECT(Pno)
);

INSERT INTO EMPLOYEE (Fname, Lname, SSN, Addrs, Sex, Salary, SuperSSN, Dno)
VALUES
('Alice', 'Smith', 101, 'Bangalore', 'F', 60000, NULL, 1),
('Bob', 'Johnson', 102, 'Mumbai', 'M', 75000, 101, 2),
('Charlie', 'Brown', 103, 'Delhi', 'M', 90000, 102, 3),
('Diana', 'White', 104, 'Chennai', 'F', 50000, 101, 1),
('Eve', 'Black', 105, 'Kolkata', 'F', 95000, 103, 3);

INSERT INTO DEPARTMENT (Dname, Dnumber, MgrSSN, MgrStartDate)
VALUES
('HR', 1, 101, '2020-01-01'),
('Finance', 2, 102, '2021-01-01'),
('IT', 3, 103, '2022-01-01'),
('Sales', 4, 104, '2023-01-01'),
('Operations', 5, 105, '2024-01-01');

INSERT INTO PROJECT (Pno, Pname, Dnum)
VALUES
(1, 'Recruitment', 1),
(2, 'Payroll System', 2),
(3, 'Website Development', 3),
(4, 'Customer Management', 4),
(5, 'Logistics', 5);

INSERT INTO WORKS_ON (ESSN, Pno, Hours)
VALUES
(101, 1, 20),
(102, 2, 15),
(103, 3, 25),
(104, 4, 30),
(105, 5, 40);

SELECT Fname, Lname
FROM EMPLOYEE
WHERE Salary > (
    SELECT MAX(Salary)
    FROM EMPLOYEE
    WHERE Dno = 5
);

SELECT ESSN
FROM WORKS_ON
WHERE Pno IN (1, 2, 3);

SELECT Pno, SUM(Hours) AS Total_Hours
FROM WORKS_ON
GROUP BY Pno;
