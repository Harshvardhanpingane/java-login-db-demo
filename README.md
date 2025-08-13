# Java Login App with MySQL

This is a simple Java web application with JSP and Servlet for user login using MySQL database.

## Database Setup

```sql
CREATE DATABASE studentdb;

USE studentdb;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(100) NOT NULL,
    password VARCHAR(100) NOT NULL
);

INSERT INTO users (username, password) VALUES ('admin', 'admin123');
```

## Tech Stack

- Java Servlet
- JSP
- MySQL
- JDBC
- Apache Tomcat (for deployment)

## Run Instructions

1. Import this project into Eclipse/IntelliJ as Dynamic Web Project.
2. Setup Tomcat Server.
3. Make sure MySQL is running and DB is created.
4. Update DB credentials in `LoginServlet.java`.
5. Run the project and access `login.jsp`.
