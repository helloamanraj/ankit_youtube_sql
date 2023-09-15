Question: https://www.youtube.com/watch?v=u3W_Op3FTVA

Table 1: 
```sql
CREATE TABLE employee_details (
    employeeid INT,
    phone_number VARCHAR(10),
    isdefault BOOLEAN,
    PRIMARY KEY (employeeid, phone_number)
);


INSERT INTO employee_details (employeeid, phone_number, isdefault)
VALUES
    (1001, '9999', false),
    (1001, '1111', false),
    (1001, '2222', true),
    (1003, '3333', false);
```

Table 2: 
```sql
CREATE TABLE employee_checkin_details (
    employeeid INT,
    entry_details VARCHAR(10),
    timestamp_details TIMESTAMP,
    PRIMARY KEY (employeeid, timestamp_details)
);


INSERT INTO employee_checkin_details (employeeid, entry_details, timestamp_details)
VALUES
    (1000, 'login', '2023-06-16 01:00:15.34'),
    (1000, 'login', '2023-06-16 02:00:15.34'),
    (1000, 'login', '2023-06-16 03:00:15.34'),
    (1000, 'logout', '2023-06-16 12:00:15.34'),
    (1001, 'login', '2023-06-16 01:00:15.34'),
    (1001, 'login', '2023-06-16 02:00:15.34'),
    (1001, 'login', '2023-06-16 03:00:15.34'),
    (1001, 'logout', '2023-06-16 12:00:15.34');
```

Solution:

```sql
SELECT
    (a.employeeid),
    COUNT(a.employeeid) AS totalentry,
    COUNT(CASE WHEN a.entry_details = 'login' THEN 1 ELSE NULL END) AS login_count,
    COUNT(CASE WHEN a.entry_details = 'logout' THEN 1 ELSE NULL END) AS logout_count,
    MAX(CASE WHEN a.entry_details = 'login' THEN a.timestamp_details ELSE NULL END) AS latest_login,
    MAX(CASE WHEN a.entry_details = 'logout' THEN a.timestamp_details ELSE NULL END) AS latest_logout,
    b.phone_number
FROM
    employee_checkin_details AS a
left JOIN
    employee_details AS b ON a.employeeid = b.employeeid and b.isdefault = true
GROUP BY
    a.employeeid,b.phone_number;
```