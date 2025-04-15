# Java Database Connectivity 
- JDBC - stands for Java database Connectivity  
- it is an external, relatively low level API provided by Java 
- serves as an interface for connecting java applications to a database via SQL.
- JDBC provides methods to query and update data in a database, making it key component for database interaction in java applications.
## Steps for using the JDBC 
- In order to interact with a database, we have to follow the following steps 
1. obtain a JDBC driver
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
         
        ``` 
        2. PreparedStatement 
        - it is an interface that is used to execute **precompiled** SQL statements and return a result against a database.
        - the SQL command is parametrized and the parameters are passed to the SQL command at runtime.
        -example
        ```
        PreparedStatement preparedStatement = connection.prepareStatement("SELECT * FROM users WHERE id = ?");
        preparedStatement.setInt(1, 1);
        ResultSet resultSet = preparedStatement.executeQuery();
        ```
        3. CallableStatement
    
    3. Execute some SQL statements using 
        1. using statement
        - it is an interface that is used to execute **static** SQL statements and return a result against a database.
        - the SQL command is not parametrized (means the SQL command is hard coded/ do not require any parameters).
        - the downside of using statement interface is that it is vulnerable to SQL injection attacks because of this reason it is not recommended for use in production applications.
        - example
        ```
        Statement statement = connection.createStatement (); 
        ```
        2. using PreparedStatement 
        - it is an interface that is used to execute **precompiled** SQL statements and return a result against a database.
        - the SQL command is parametrized and 




