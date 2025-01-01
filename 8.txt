-- Drop existing tables if they exist
DROP TABLE IF EXISTS BATTING;
DROP TABLE IF EXISTS BOWLING;
DROP TABLE IF EXISTS MATCH;
DROP TABLE IF EXISTS PLAYER;

-- Create PLAYER table
CREATE TABLE PLAYER (
    Pid INT PRIMARY KEY,
    Lname VARCHAR(100),
    Fname VARCHAR(100),
    Country VARCHAR(100),
    Yborn INT,
    Bplace VARCHAR(100)
);

-- Create MATCH table
CREATE TABLE MATCH (
    MatchId INT PRIMARY KEY,
    Team1 VARCHAR(100),
    Team2 VARCHAR(100),
    Ground VARCHAR(100),
    Date DATE,
    Winner VARCHAR(100)
);

-- Create BATTING table
CREATE TABLE BATTING (
    MatchId INT,
    Pid INT,
    Nruns INT,
    Fours INT,
    Sixes INT,
    PRIMARY KEY (MatchId, Pid),
    FOREIGN KEY (MatchId) REFERENCES MATCH(MatchId),
    FOREIGN KEY (Pid) REFERENCES PLAYER(Pid)
);

-- Create BOWLING table
CREATE TABLE BOWLING (
    MatchId INT,
    Pid INT,
    Novers INT,
    Maidens INT,
    Nruns INT,
    Nwickets INT,
    PRIMARY KEY (MatchId, Pid),
    FOREIGN KEY (MatchId) REFERENCES MATCH(MatchId),
    FOREIGN KEY (Pid) REFERENCES PLAYER(Pid)
);

-- Insert data into PLAYER table
INSERT INTO PLAYER (Pid, Lname, Fname, Country, Yborn, Bplace)
VALUES
(1, 'Dhoni', 'MS', 'India', 1981, 'Ranchi'),
(2, 'Smith', 'Steve', 'Australia', 1989, 'Sydney'),
(3, 'Kohli', 'Virat', 'India', 1988, 'Delhi'),
(4, 'Warner', 'David', 'Australia', 1986, 'Sydney'),
(5, 'Stokes', 'Ben', 'England', 1991, 'Christchurch');

-- Insert data into MATCH table
INSERT INTO MATCH (MatchId, Team1, Team2, Ground, Date, Winner)
VALUES
(101, 'Australia', 'India', 'Melbourne', '2024-12-01', 'India'),
(102, 'India', 'England', 'Lords', '2024-12-02', 'India'),
(103, 'Australia', 'England', 'Sydney', '2024-12-03', 'Australia'),
(104, 'India', 'Australia', 'Delhi', '2024-12-04', 'Australia'),
(105, 'England', 'Australia', 'London', '2024-12-05', 'England');

-- Insert data into BATTING table
INSERT INTO BATTING (MatchId, Pid, Nruns, Fours, Sixes)
VALUES
(101, 1, 75, 6, 2),
(101, 3, 45, 4, 1),
(102, 3, 85, 7, 3),
(103, 2, 100, 12, 5),
(104, 4, 65, 8, 2);

-- Insert data into BOWLING table
INSERT INTO BOWLING (MatchId, Pid, Novers, Maidens, Nruns, Nwickets)
VALUES
(101, 5, 10, 2, 50, 3),
(102, 4, 8, 1, 60, 2),
(103, 5, 9, 1, 55, 4),
(104, 2, 10, 3, 45, 5),
(105, 1, 7, 0, 65, 1);

-- Query 1: Get distinct grounds where Australia played
SELECT DISTINCT Ground
FROM MATCH
WHERE Team1 = 'Australia'
ORDER BY Ground;

-- Query 2: Get match details where MS Dhoni played
SELECT DISTINCT M.*
FROM MATCH M
JOIN BATTING B ON M.MatchId = B.MatchId
JOIN PLAYER P ON B.Pid = P.Pid
WHERE P.Fname = 'MS' AND P.Lname = 'Dhoni';

-- Query 3: Get first and last names of players who batted in match 103
SELECT P.Fname, P.Lname
FROM PLAYER P
JOIN BATTING B ON P.Pid = B.Pid
WHERE B.MatchId = 103;
