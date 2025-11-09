### Imagine you're an HR analyst at a tech company tasked with analyzing employee salaries. Your manager is keen on understanding the pay distribution and asks you to determine the second highest salary among all employees.
### It's possible that multiple employees may share the same second highest salary. In case of duplicate, display the salary only once.

<u>employee Schema:</u>
|column_name	|type	|description
| :---: | :---: | :---: |
|employee_id	|integer	|The unique ID of the employee.
|name	|string	|The name of the employee.
|salary	|integer	|The salary of the employee.
|department_id	|integer	|The department ID of the employee.
|manager_id	|integer	|The manager ID of the employee.

### Solution 

```
SELECT salary
FROM (SELECT salary, dense_rank() over ( order by salary) as rank
FROM employee
ORDER by rank DESC limit 1 offset 1) t;
```