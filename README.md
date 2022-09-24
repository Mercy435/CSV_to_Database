# CSV_to_Database

### mysql database

## steps
1. Install requirements

-pip install mysql-connector-python
-pip install pandas
-MySQL Workbench with mysql

2. Prepare a csv file if none exists 

3. Import csv as a dataframe using pandas
  - first import pandas
  import pandas as pd
  
 pd.read_csv("path to csv file")
 
4. connect to mySQL using Python and create database
  - Import the required modules
    import mysql.connector as msql
    from mysql.connector import Error
    
 usinga. try and except block, connet to mysql
try:
    conn = msql.connect(host='localhost', user='root',  
                        password='root@123')#give ur username, password
    if conn.is_connected():
        cursor = conn.cursor()
        cursor.execute("CREATE DATABASE employee")
        print("Database is created")
except Error as e:
    print("Error while connecting to MySQL", e)
    
   OR
   pip install mysql-connector-python

    install mysql workbench and create database with a name (e.g employee)
    
  5. create a table in mySQL and import csv to the table
      - import required modules
        import mysql.connector as msql
        from mysql.connector import Error
          try:
              conn = mysql.connect(host='localhost', database='employee', user='root', password='root@123')
              if conn.is_connected():
                  cursor = conn.cursor()
                  cursor.execute("select database();")
                  record = cursor.fetchone()
                  print("You're connected to database: ", record)
                  cursor.execute('DROP TABLE IF EXISTS employee_data;')
                  print('Creating table....')
          # in the below line please pass the create table statement which you want #to create
                  cursor.execute("CREATE TABLE employee_data(first_name varchar(255),last_name varchar(255),company_name varchar(255),address varchar(255),city varchar(255),county varchar(255),state varchar(255),zip int,phone1 varchar(255),phone2 varchar(255),email varchar(255),web varchar(255))")
                  print("Table is created....")
                  #loop through the data frame
                  for i,row in empdata.iterrows():
                      #here %S means string values 
                      sql = "INSERT INTO employee.employee_data VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)"
                      cursor.execute(sql, tuple(row))
                      print("Record inserted")
                      # the connection is not auto committed by default, so we must commit to save our changes
                      conn.commit()
          except Error as e:
                      print("Error while connecting to MySQL", e)
                      
    6. query table to see saved data
    
        # Execute query
          sql = "SELECT * FROM employee.employee_data"
          cursor.execute(sql)
          # Fetch all the records
          result = cursor.fetchall()
          for i in result:
              print(i)



### sql server

##### import necesary modules
  import pandas as pd
  import pyodbc

# Read csv file
data = pd.read_csv ("path to csv file')   
df = pd.DataFrame(data)

# Connect to SQL Server
conn = pyodbc.connect('Driver={SQL Server};'
                      'Server=RON\SQLEXPRESS;'
                      'Database=test_database;'
                      'Trusted_Connection=yes;')
cursor = conn.cursor()

# Create Table
cursor.execute('''
		CREATE TABLE products (
			product_id int primary key,
			product_name nvarchar(50),
			price int
			)
               ''')

# Insert DataFrame to Table
for row in df.itertuples():
    cursor.execute('''
                INSERT INTO products (product_id, product_name, price)
                VALUES (?,?,?)
                ''',
                row.product_id, 
                row.product_name,
                row.price
                )
conn.commit()

Query table
select * from products



    
