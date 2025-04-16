# Java Database Connectivity 
- JDBC - stands for Java database Connectivity  
- it is a part of the Java Standard Edition (Java SE) included in JDK, relatively low level API provided by Java.
- it is located in the java.sql and javax.sql package.
- serves as an interface for connecting java applications to a database via SQL.
- JDBC provides methods to query and update data in a database, making it key component for database interaction in java applications.
## Steps for using the JDBC 
- In order to interact with a database, we have to follow the following steps 
1. obtain a JDBC driver
    - it is a software component (JAR file/dependency) that enables java applications to interact with a database.
    - it acts as a bridge between the java application and the specific database vendor.
    - it is vendor specific and must be compatible with the database being used.
    - it is a jar file that contains the classes and methods needed to connect to the database.
    - JDBC provides a set of standard interfaces to connect a Java application to a database. Since each database is vendor-specific, the implementation of these interfaces is provided by the vendor through a JDBC driver, not by the DBMS itself.
2. Establish a connection using <br>
    i. Database URL<br>
    ii. Username <br>
    iii. Password 
    ```
    Connection conn = DriverManager.getConnection("databaseUrl", "username", "password");
    ```
3. Create a statement using <br>
    i. Statement <br>
    ii. PreparedStatement <br>
    iii. CallableStatement
4. Execute some SQL statements using <br>
    i. executeQuery () for SELECT statements
    ii. executeUpdate () for INSERT, UPDATE, DELETE statements
    iii. execute () for any other SQL statements
5. Retrieve the result of executing the statement
6. Close the connection.

## Open a connection
- there are two ways to create connection to a database using java
1. using Driver manager
    - Driver manager is a class that manages a list of database drivers.
    - it is used to establish a connection to a database by providing the database URL, username, and password.
    - open a new database connection every time when a query is made.
    - this makes it less efficient for applications that require multiple queries to be executed.
    - example 
    ```java
    Connection conn = DriverManager.getConnection("databaseUrl", "username", "password");
    ```
2. using Data source 
    - this is a more efficient way to create a connection to a database.
    - instead of using DriverManager.getConnection() method, we use a DataSource object to create a connection.
    - the data source object is configured with the database URL, username, and password.
    - DataSource is an interface that provides a way to create a connection to a database.
    - it is used to create a connection pool, which is a set of connections that can be reused.
    - a connection pool is a catch of database connections that can be reused, which makes it more efficient for applications that require multiple queries to be executed.
    - many JDBC connection pool i
    - example 
    ```java
    DataSource dataSource = new SomeDataSourceImplementation();
    Connection conn = dataSource.getConnection();

    /* the connection object is used to create a statement and have a method such as 
        1. createStatement (), 
        2. prepareStatement () and 
        3. callableStatement ()
    */
    ```
    - In java DataSource is an interface and the actual implementation is determined by the specific database being used. For example, if we use h2 database, we use JdbcDataSource class and if we use postgresql, we use PGSimpleDataSource class.

    - example 
    ```
    public class ConnectionUtil {

        private static String DB_URL = "jdbc:h2:mem:testdb"; 
        private static String USERNAME = "sa";
        private static String PASSWORD = "pass";

        private static JdbcDataSource ds = new JdbcDataSource();

        static {
            ds.setUrl(DB_URL);
            ds.setUser(USERNAME);
            ds.setPassword(PASSWORD);
        }

        public static Connection getConnection() throws SQLException {
            return ds.getConnection();
        }
        
    }
    // the database url includes the jdbc:database type(h2)://host:port/databaseName 
    ```
    ## How it works?
    - when the application starts, a pool of connections is created instead of creating a new connection every time a query is made.
    - when a connection is needed, an existing connection from the pool is used.
    - when finished, the connection is returned to the pool instead of being closed.
    - if no  available connection exist a new connection is created (up to the limit).
    - we can set the maximum number of connections in the pool. this is done by setting the maximum pool size. 
    ```
    ds.setMaxPoolSize(10);
    ```
    
    ### Advantages
    - this improves performance (faster execution), resource usage (fewer active connection), scalability (support more users).
    - opening and closing a database is slower because each connection 
    1. Establish a network connection.
    2. Authenticates username/ password
    3. Allocates resource in database server.
    
    - if the application has many concurrent users, creating a new connection for each request wastes time and resources.

    3. Create a statement
    - Once a connection is established, we can create a statement object using the connection object.
    - Statement is an interface that is used to execute SQL statements and return a result against a database.
    - there are three types of statement interfaces in JDBC
        1. Statement 
        - it is an interface that is used to execute **static** SQL statements and return a result against a database.
        - the SQL command is not parametrized (means the SQL command is hard coded/ do not require any parameters).
        - the downside of using statement interface is that it is vulnerable to SQL injection attacks because of this reason it is not recommended for use in production applications.
        - example
        ```
        Statement statement = connection.createStatement ();
        // we can't use new keyword to create a statement object because it is an interface. Instead we use the createStatement() method of the connection object.
         
        ``` 
        2. PreparedStatement 
        - it is an interface that is used to execute **precompiled** SQL statements and return a result against a database.
        - the SQL command is parametrized and we use the SetInt(), SetString (), SetDouble(), SetDate() methods to set the parameters.
        - the SQL command is precompiled and stored in a PreparedStatement object.
        - the parameters are passed to the SQL command at runtime.
        - advantages 
            - SQL injection protection - inputs are not interpreted as a part of the SQL command.
            - Performance - the database engine can reuse the query plan by just changing the parameters on top of the precompiled SQL command.
            - Reusability - the same SQL command can be reused with different parameters.
            - Readability - no messy string concatenation. (String sql = "insert into student values ("+ sid + ", '" + sname + "', " + marks + ")";) 
        example
        ```
        String query = "select * from users where username = ? and password = ?";
        PreparedStatement ps = connection.prepareStatement(query);
        ps.setString(1, "username");
        ps.setString(2, "password");
        ResultSet rs = ps.executeQuery();

        /*
        - ? are placeholders (also called bind parameters)
        -ps.setString binds the actual value at runtime - not during SQL compilation.
        - The SQL query is compiled once, but parameters are injected later and safely.
        - this makes it more efficient and secure than using statement interface.

        */  
        ```
        ## Note 
        - **In JDBC, with Statement, the full SQL query (with values) is added during the execution step using executeQuery() or executeUpdate(). With PreparedStatement, the SQL query (with placeholders) is added during the creation step via prepareStatement(), and parameter values are set in a separate binding step using setXXX() methods before execution.**

        3. using CallableStatement
        - it is an interface that is used to execute **stored procedures** and return a result against a database.
        - the SQL command is not hard coded and can be changed at runtime.
        - example
        ```
        CallableStatement callableStatement = connection.prepareCall("{call getUserById(?)}");
        callableStatement.setInt(1, 1);
        ResultSet resultSet = callableStatement.executeQuery();
        ```
    
    4. Execute some SQL statements using 
       - once a statement object is created using the connection object, we can execute some SQL statements using the statement object.
       - there are three methods to execute SQL statements using the statement object.
        1. executeQuery () for SELECT statements
        - this method returns a result set object that contains the data returned by the SQL statement.
        - the result set object is an interface that provides methods to iterate through the data returned by the SQL statement.
        2. executeUpdate () for INSERT, UPDATE, DELETE statements
        - this method returns an integer value that represents the number of rows affected by the SQL statement.
        3. execute () for any other SQL statements
        -  this method returns a boolean value that represents the success or failure of the SQL statement.
        - this method is used for executing any other SQL statements that do not fall under the above two categories. for example, DDL statements such as CREATE, ALTER, DROP, etc.

        - example
        ```
        Statement statement = connection.createStatement();
        // execute a SELECT statement
        ResultSet resultSet = statement.executeQuery("SELECT * FROM users");
        // execute an INSERT statement
        int rowsAffected = statement.executeUpdate("INSERT INTO users (username, password) VALUES ('user1', 'pass1')");
        // execute a DELETE statement
        int rowsDeleted = statement.executeUpdate("DELETE FROM users WHERE username = 'user1'");
        // execute a DDL statement
        boolean result = statement.execute("CREATE TABLE users (id INT PRIMARY KEY, username VARCHAR(50), password VARCHAR(50))");
        ```
        - the execute method can be used to execute any SQL statement, but it is not recommended for use in production applications because it is less efficient than using the executeQuery() and executeUpdate() methods.

    5. Retrieve the result of executing the statement
    - once the SQL statement is executed, we can retrieve the result of executing the statement using the result set object.
    - the result set object provides methods to iterate through the data returned by the SQL statement.
    - the result set object is an interface that provides methods to iterate through the data returned by the SQL statement.
    - the result set object is created by executing a SELECT statement using the executeQuery() method.
    - the result set object is used to retrieve the data returned by the SQL statement.
    - some of the methods provided by the result set object are:
        1. next() - moves the cursor to the next row in the result set. Provides a boolean value indicating if there is a next row.
        - the initial position of the cursor is before the first row.
        - the next() method moves the cursor to the next row in the result set and returns a boolean value indicating if there is a next row.

        2. getString() - retrieves the value of a column in the current row as a string.
        3. getInt() - retrieves the value of a column in the current row as an integer.
        4. getDouble() - retrieves the value of a column in the current row as a double.
        5. getDate() - retrieves the value of a column in the current row as a date.
        6. close() - closes the result set object and releases any resources associated with it.
        - example
        ```
        Statement statement = connection.createStatement();
        ResultSet resultSet = statement.executeQuery("SELECT * FROM users");
        while (rs.next()) {
            int id = rs.getInt("id");
            String username = rs.getString("username");
            String password = rs.getString("password");
            System.out.println("ID: " + id + ", Username: " + username + ", Password: " + password);
        }
        ```
        - the above code retrieves the data returned by the SQL statement and prints it to the console.

        ```
        // These is how we handle the result set if the table/the number of columns are not known 
        // 3. Query the table
            ResultSet rs = stmt.executeQuery("SELECT * FROM employee");
            ResultSetMetaData metaData = rs.getMetaData();    // ResultSetMetaData is an interface that provides information about the types and properties of the columns in a ResultSet object.

            int columnCount = metaData.getColumnCount();  // Get the number of columns in the table for iteration purpose.

            // 4. Print column names
            System.out.println("Table Columns:");
            for (int i = 1; i <= columnCount; i++) {
                String columnName = metaData.getColumnName(i);
                String columnType = metaData.getColumnTypeName(i);
                System.out.println("- " + columnName + " (" + columnType + ")");
            }

            System.out.println("\nData:");
            // 5. Iterate through rows
            while (rs.next()) {
                for (int i = 1; i <= columnCount; i++) {
                    Object value = rs.getObject(i); // Dynamic type
                    System.out.print(value + "\t");
                }
                System.out.println();
            }
        ```

    6. Close the connection
    - once the SQL statement is executed and the result is retrieved, we can close the connection using the close() method of the connection object.
    - this method closes the connection to the database and releases any resources associated with it.
    - it is important to close the connection to the database to avoid memory leaks and other issues.
    - example
    ```
    connection.close();
    ```
    - if we use a connection pool, we do not need to close the connection. instead, we return the connection to the pool using the close() method of the connection object.
    - example
    ```
    connection.close(); // returns the connection to the pool
    ```
    - the connection pool will manage the connections and close them when they are no longer needed.


       

