# B+ Tree Database Engine

## Overview

This project implements a custom database engine that utilises B+ Trees for efficient indexing and data retrieval. The implementation is built in Java and designed to provide fundamental database operations while focusing on performance optimisation through the use of appropriate data structures and algorithms.

## Features

- **Table Management**: Create and manage database tables with defined schemas
- **B+ Tree Indexing**: Efficient data retrieval using B+ Tree indexing structure
- **CRUD Operations**: Complete support for Create, Read, Update, and Delete operations
- **Query Operations**: Complex querying capabilities with comparison operators and logical connectives
- **Data Persistence**: Data is stored on disk using Java serialisation
- **Metadata Management**: Schema information is stored in CSV format

## Core Components

### Data Structures

- **B+ Tree**: A self-balancing tree data structure that maintains sorted data and allows for efficient operations:
  - Searching in O(log n) time
  - Insertion and deletion with automatic rebalancing
  - Range queries for selecting data within intervals
  - Support for duplicate key values

- **Tables and Pages**: Data is organised into tables containing pages, with a configurable maximum number of rows per page

- **Tuples**: Records are stored as tuples containing multiple values according to the table schema

### Main Classes

- `DBApp`: Main application class providing the API for database operations
- `Table`: Represents a database table and manages its pages and operations
- `BPTree`: Implementation of the B+ Tree structure for indexing
- `Page`: Container for storing tuples within a table
- `SQLTerm`: Represents a query condition
- `Ref`: Reference to a record's location within a page

## Usage

### Creating a Table

```java
DBApp dbApp = new DBApp();
dbApp.init();

// Define table columns and their types
Hashtable<String, String> htblColNameType = new Hashtable<>();
htblColNameType.put("id", "java.lang.Integer");
htblColNameType.put("name", "java.lang.String");
htblColNameType.put("gpa", "java.lang.Double");

// Create the table with "id" as the clustering key
dbApp.createTable("Student", "id", htblColNameType);
```

### Creating an Index

```java
// Create an index on the "id" column
dbApp.createIndex("Student", "id", "idIndex");
```

### Inserting Data

```java
// Create a record to insert
Hashtable<String, Object> htblColNameValue = new Hashtable<>();
htblColNameValue.put("id", 1);
htblColNameValue.put("name", "John Doe");
htblColNameValue.put("gpa", 3.5);

// Insert the record
dbApp.insertIntoTable("Student", htblColNameValue);
```

### Querying Data

```java
// Create query terms
SQLTerm[] arrSQLTerms = new SQLTerm[2];

arrSQLTerms[0] = new SQLTerm();
arrSQLTerms[0]._strTableName = "Student";
arrSQLTerms[0]._strColumnName = "gpa";
arrSQLTerms[0]._strOperator = ">";
arrSQLTerms[0]._objValue = 3.0;

arrSQLTerms[1] = new SQLTerm();
arrSQLTerms[1]._strTableName = "Student";
arrSQLTerms[1]._strColumnName = "name";
arrSQLTerms[1]._strOperator = "=";
arrSQLTerms[1]._objValue = "John Doe";

// Define the logical operator between terms
String[] strarrOperators = new String[1];
strarrOperators[0] = "AND";

// Execute the query
Iterator resultSet = dbApp.selectFromTable(arrSQLTerms, strarrOperators);
```

### Updating Data

```java
// Define new values
Hashtable<String, Object> htblColNameValue = new Hashtable<>();
htblColNameValue.put("name", "Jane Doe");
htblColNameValue.put("gpa", 3.7);

// Update the record with id = 1
dbApp.updateTable("Student", "1", htblColNameValue);
```

### Deleting Data

```java
// Define the record to delete
Hashtable<String, Object> htblColNameValue = new Hashtable<>();
htblColNameValue.put("id", 1);

// Delete the record
dbApp.deleteFromTable("Student", htblColNameValue);
```

## Configuration

The maximum number of rows per page can be configured in the `resources/DBApp.config` file:

```
MaximumRowsCountinPage = 200
```

## Technical Details

- **Serialization**: Tables, pages, and B+ Trees are serialized to binary files for persistence
- **Metadata**: Schema information is stored in CSV format in `resources/metaFile.csv`
- **Runtime Validation**: Type checking and validation occurs during runtime
- **Dynamic Page Management**: Pages are created and managed dynamically based on data volume

## Dependencies

- Java JDK (version 19+)
- OpenCSV library (version 5.9)

## Building and Running

This project uses Maven for dependency management:

```bash
# Compile the project
mvn compile

# Build the project
mvn package
```
