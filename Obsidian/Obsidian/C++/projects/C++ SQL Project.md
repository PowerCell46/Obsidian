

## **Windows logo key + Shift + S**



# 2 private methods - read table name, read different types of data and return string
# Truncate reset serial cols

Value types

```cpp
std::unordered_map<int, std::string> pgTypeMap = { 
{ 23, "INTEGER" }, // INT4 (integer) 
{ 20, "BIGINT" }, // INT8 (bigint) 
{ 21, "SMALLINT" }, // INT2 (smallint) 
{ 1700, "NUMERIC" }, // DECIMAL, NUMERIC 
{ 700, "REAL" }, // FLOAT4 
{ 701, "DOUBLE" }, // FLOAT8 
{ 1082, "DATE" }, // DATE 
{ 1114, "TIMESTAMP" }, // TIMESTAMP WITHOUT TIME ZONE 
{ 1184, "TIMESTAMPTZ" }, // TIMESTAMP WITH TIME ZONE 
{ 1043, "VARCHAR" }, // VARCHAR 
{ 25, "TEXT" } // TEXT 
};
```


Check for SQL injection!
// Validate table name to prevent SQL injection if (tableName.find_first_not_of("abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_") != std::string::npos) { std::cerr << "Invalid table name.\n"; return; }

if value = n/a => display message & end the method with return 1;


add filter 

```cpp
std::string userInput = readColumnValue(PQftype(queryResult, i), currentColumnName);  
char *escapedValue = PQescapeLiteral(connection, userInput.c_str(), userInput.length());  
  
if (!escapedValue) {  
    std::cout << "INSERT failed: Error escaping input: " << PQerrorMessage(connection) << "\n";  
    delete[] tableNames;  
    PQclear(queryResult);  
    return 1;  
}  
  
std::string currentInsertValue(escapedValue);  
PQfreemem(escapedValue);  
  
insertValues.push_back(currentInsertValue);




```


method that can compile sql queries directly, but you have to authenticate with email and password, and if your data is in a table named auth you have the access to do so, otherwise display an error

add WHERE clause
AND, OR, IN


skip id can be made with # if directive 
do you want to include id in the insert or not?
