# PostgreSQL Container with Data Import and Queries
(inspired from https://www.docker.com/blog/how-to-use-the-postgres-docker-official-image/ and  https://hub.docker.com/_/postgres)

First, we pull the latest PostgreSQL image with `docker pull postgres`
then we run the container using *ituser* in `docker run --name postgresEx -e POSTGRES_USER=ituser -e POSTGRES_PASSWORD=your_password -d postgres`
The container should appear in Docker Desktop

![image](https://github.com/user-attachments/assets/284822aa-eaef-4ca0-91a4-73b48ac1bfe9)


To access the container, run `docker exec -it postgresEx psql -U ituser`.This will log you into PostgreSQL as *ituser*.
Now we use SQL commands to execute queries. We create *company_db* database using `CREATE DATABASE company_db;` and connect with it `\c company_db;`

![image](https://github.com/user-attachments/assets/d20f6bfe-c1aa-49e1-b024-1bdb6826af5e)


After creating the tables and introducing some values to them, we can check with 
`\dt` -- Check tables

`SELECT * FROM departments;` -- Check departments

`SELECT * FROM employees;` -- Check employees

`SELECT * FROM salaries;` -- Check salaries


###  Exercises with queries
**Total number of employees:** `SELECT COUNT(*) FROM employees;`  -> Result: 53

**Names of employees in a specific department** : 
```
SELECT e.first_name, e.last_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
WHERE d.department_name = 'IT';
```
![image](https://github.com/user-attachments/assets/78e6f31b-1c10-4c2a-b1e0-fe3a185073ed)


**Calculate the highest and lowest salaries per department**
```
SELECT d.department_name, 
       MAX(s.salary) AS highest_salary, 
       MIN(s.salary) AS lowest_salary
FROM employees e
JOIN salaries s ON e.employee_id = s.employee_id
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY highest_salary DESC;

```

![image](https://github.com/user-attachments/assets/8afdd5f4-70f7-40d0-a22c-edb9b413dd29)




Dump the dataset into a file: `docker exec -t postgresEx pg_dump -U ituser -d company_db > company_db_dump.sql`



## Bash task
```
#!/bin/bash

# Set variables
CONTAINER_NAME="postgresEx"
DB_NAME="company_db"
DB_USER="ituser"
DB_ADMIN="admin_cee"
DUMP_FILE="company_db_dump.sql"
LOG_FILE="query_results.log"

echo "Creating database..."
docker exec -t $CONTAINER_NAME psql -U postgres -c "CREATE DATABASE $DB_NAME;"

echo "Creating admin user ($DB_ADMIN)..."
docker exec -t $CONTAINER_NAME psql -U postgres -d $DB_NAME -c "CREATE USER $DB_ADMIN WITH SUPERUSER PASSWORD 'securepassword';"

echo "Importing dataset from $DUMP_FILE..."
docker exec -i $CONTAINER_NAME psql -U $DB_USER -d $DB_NAME < $DUMP_FILE

echo "Executing queries and saving output to $LOG_FILE..."

docker exec -t $CONTAINER_NAME psql -U $DB_USER -d $DB_NAME -c "
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
ORDER BY d.department_name, e.last_name;" > $LOG_FILE

docker exec -t $CONTAINER_NAME psql -U $DB_USER -d $DB_NAME -c "
SELECT d.department_name, MAX(s.salary) AS highest_salary, MIN(s.salary) AS lowest_salary
FROM employees e
JOIN salaries s ON e.employee_id = s.employee_id
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY highest_salary DESC;" >> $LOG_FILE

echo "Results saved in $LOG_FILE."

```

Running the script:
```
docker start my_postgres --if container is not running
docker ps
chmod +x setup_db.sh
./setup_db.sh
```

(made with help from ChatGPT)
