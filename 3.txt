CREATE TABLE AIRCRAFT (
    Aircraft_ID INT PRIMARY KEY,
    Aircraft_name VARCHAR(100),
    Cruising_range INT
);

CREATE TABLE CERTIFIED (
    Emp_ID INT,
    Aircraft_ID INT,
    PRIMARY KEY (Emp_ID, Aircraft_ID),
    FOREIGN KEY (Aircraft_ID) REFERENCES AIRCRAFT(Aircraft_ID)
);

CREATE TABLE EMPLOYEE (
    Emp_ID INT PRIMARY KEY,
    Ename VARCHAR(100),
    Salary DECIMAL(10, 2)
);
INSERT INTO AIRCRAFT (Aircraft_ID, Aircraft_name, Cruising_range)
VALUES
(1, 'Boeing 737', 3000),
(2, 'Airbus A320', 3500),
(3, 'Boeing 747', 4000),
(4, 'Cessna 172', 1000),
(5, 'Boeing 767', 4500);
INSERT INTO EMPLOYEE (Emp_ID, Ename, Salary)
VALUES
(101, 'Alice', 60000),
(102, 'Bob', 80000),
(103, 'Charlie', 90000),
(104, 'Diana', 70000),
(105, 'Eve', 50000);
INSERT INTO CERTIFIED (Emp_ID, Aircraft_ID)
VALUES
(101, 1),
(102, 2),
(103, 3),
(104, 4),
(105, 1),
(105, 2);
SELECT Emp_ID
FROM EMPLOYEE
WHERE Salary = (SELECT MAX(Salary) FROM EMPLOYEE);
SELECT DISTINCT A.Aircraft_name
FROM AIRCRAFT A
JOIN CERTIFIED C ON A.Aircraft_ID = C.Aircraft_ID
JOIN EMPLOYEE E ON C.Emp_ID = E.Emp_ID
GROUP BY A.Aircraft_name
HAVING MIN(E.Salary) > 50000;
SELECT Emp_ID
FROM CERTIFIED
GROUP BY Emp_ID
HAVING COUNT(Aircraft_ID) = (
    SELECT MAX(num_certified)
    FROM (SELECT COUNT(Aircraft_ID) AS num_certified FROM CERTIFIED GROUP BY Emp_ID) AS counts
);
