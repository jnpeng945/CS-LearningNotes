[TOC]

:star:  MySQL 中，如果要转义用作列名的保留字，可以如下表示：

```sql
`Rank`		-- 关键字之前和之后用撇号
```



#### [175. 组合两个表](https://leetcode-cn.com/problems/combine-two-tables/)

多表查询问题，使用联结。多表的联结分为：

- 左联结（LEFT JOIN）：联结结果保留左表的全部数据；
- 右联结（RIGHT JOIN）：联结结果保留右表的全部数据；
- 内联结（INNER JOIN）：取两表的公共数据；

分析本题：Address 表中的 PersonId 为 Person 的外关键字，我们选择以 Person 表进行左联结，保留左边表中的全部数据。联结条件就是：PersonId 。

```sql
SELECT FirstName, LastName, City, State
FROM Person LEFT JOIN Address
ON Person.PersonId = Address.PersonId;
```



#### [176. 第二高的薪水](https://leetcode-cn.com/problems/second-highest-salary/)

薪水可能有一样的值，于是用 `DISTINCT` 薪水进行薪水去重。

解法1：使用子查询和 LIMIT 子句。将不同薪资按照降序排列，然后用 LIMIT 子句获得第二高的薪资。

```sql
SELECT 
    (SELECT DISTINCT Salary FROM Employee
    ORDER BY Salary DESC
    LIMIT 1 OFFSET 1	-- 表示查询结果跳过1条数据，读取前1条数据
    ) AS SecondHighestSalary;
```

解法2：使用 IFNULL 和 LIMIT 子句

```sql
-- IFNULL(a, b) 如果a不是NULL返回a,否则返回b
SELECT IFNULL (
    (SELECT DISTINCT Salary
    FROM Employee
    ORDER BY Salary DESC
    LIMIT 1 OFFSET 1), NULL) 
AS SecondHighestSalary;
```

#### [177. 第N高的薪水](https://leetcode-cn.com/problems/nth-highest-salary/)

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    SET N := N-1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT IFNULL
      (
        (SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT N, 1),
         NULL
      )
  );
END
```

#### [181. 超过经理收入的员工](https://leetcode-cn.com/problems/employees-earning-more-than-their-managers/)

解法1：使用 WHERE 子句

```sql
SELECT a.Name AS 'Employee'
FROM Employee AS a, Employee AS b
WHERE a.ManagerId = b.Id
AND a.Salary > b.Salary;
```

解法2：使用 JOIN 子句联结

```sql
SELECT a.Name AS 'Employee'
FROM Employee AS a JOIN Employee AS b
ON a.ManagerId = b.Id
AND a.Salary > b.Salary; 
```

#### [182. 查找重复的电子邮箱](https://leetcode-cn.com/problems/duplicate-emails/)

解法1：使用 GROUP BY 和临时表

```sql
SELECT Email FROM 
(
    SELECT Email, COUNT(Email) AS num
    FROM Person
    GROUP BY Email
) AS statistic
WHERE num > 1;
```

解法2：使用 GROUP BY 和 HAVING 条件

```sql
SELECT Email FROM Person
GROUP BY Email
HAVING COUNT(Email) > 1;
```

#### [183. 从不订购的客户](https://leetcode-cn.com/problems/customers-who-never-order/)

解法：使用子查询和 `NOT IN` 子句

```sql
SELECT Customers.Name AS 'Customers'
FROM Customers
WHERE Customers.Id NOT IN
(
    SELECT CustomerId FROM Orders
);
```

#### [196. 删除重复的电子邮箱](https://leetcode-cn.com/problems/delete-duplicate-emails/)

解法：使用 `DELETE` 和 `WHERE` 子句

```sql
DELETE p1
FROM Person p1, Person p2
WHERE p1.Email = p2.Email AND p1.Id > p2.Id;
```

#### [197. 上升的温度](https://leetcode-cn.com/problems/rising-temperature/)

解法：使用 `DATEDIFF()` 子句，MySQL 使用 `DATEDIFF` 来比较两个日期类型的值。

```sql
-- 写法1：使用 WHERE AND 子句和 DATEDIFF 函数
SELECT w2.id AS 'id'
FROM Weather AS w1, Weather AS w2
WHERE DATEDIFF(w2.recordDate , w1.recordDate ) = 1
AND w2.Temperature > w1.Temperature;

-- 写法2：使用 JOIN 和 DATEDIFF() 函数
SELECT Weather.id AS 'id'
FROM Weather JOIN Weather AS w
ON DATEDIFF(Weather.recordDate , w.recordDate ) = 1
AND Weather.Temperature > w.Temperature;
```

#### [178. 分数排名](https://leetcode-cn.com/problems/rank-scores/)

解法1：结果包含两个部分：1）降序排列的分数；2）每个分数对应的排名。

```sql
SELECT s1.Score AS 'Score', (
    SELECT COUNT(DISTINCT s2.Score)
    FROM Scores AS s2
    WHERE s2.Score >= s1.Score
) AS 'Rank'
FROM Scores AS s1
ORDER BY s1.Score DESC;
```

解法2：窗口函数 DENSE_RANK()

```sql
SELECT Score, DENSE_RANK() OVER(ORDER BY Score DESC) as 'Rank'
FROM Scores;
```



:bulb: 窗口函数 rank, dense_rank, row_number 有什么区别?

<div align="center"> <img src="Figs/SQL%E7%BB%83%E4%B9%A0_1.png" width="700"/> </div><br>

#### [180. 连续出现的数字](https://leetcode-cn.com/problems/consecutive-numbers/)

解法1：使用 `DISTINCT` 和 `WHERE` 语句

连续出现意味着相同数字的 Id 是连着的，而且题中要求至少连续出现 3 次。任意选择l1.Num 或者 l2.Num 或者 l3.Num 作为返回结果，需要去使用 `DISTINCT`。

```sql
SELECT DISTINCT l1.Num AS 'ConsecutiveNums'
FROM Logs AS l1, Logs AS l2, Logs AS l3
WHERE l1.Id = l2.Id - 1
AND l2.Id = l3.Id - 1
AND l1.Num = l2.Num
AND l2.Num = l3.Num;
```

#### [184. 部门工资最高的员工](https://leetcode-cn.com/problems/department-highest-salary/)

解法1：使用 `JOIN` 和 `IN` 语句

由于 **Employee** 表包含 *Salary* 和 *DepartmentId* 字段，通过此表我们可以查询部门内最高工资。

```sql
SELECT DepartmentId, MAX(Salary)
FROM Employee
GROUP BY DepartmentId;
```

随后将 **Employee** 表和 **Department** 联结，并在临时表中使用 `IN` 语句查询部门名字和工资关系。

```sql
SELECT Department.Name AS 'Department',
        Employee.Name AS 'Employee',
        Salary
FROM Employee JOIN Department ON Employee.DepartmentId = Department.Id
WHERE (Employee.DepartmentId, Salary) IN
(
    SELECT DepartmentId, MAX(Salary)
    FROM Employee
    GROUP BY DepartmentId
);
```

