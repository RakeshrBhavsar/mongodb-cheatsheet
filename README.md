# mongodb-cheatsheet

### Data Types 

Type | Syntax
--- | ---
Object | {}
ObjectId | ObjectId()
String | ""
Array | []
Double | 0.0
Date | ISODate()
int32 | NumberInt(x)
int64 | NumberLong(x)
boolean | true/false

**_id - reserved and it is used to uniquely identify a document.  It can be assigned manually.**


### CRUD Operations

#### Lookup Databases and Collections


`db` -> shows active database

`show databases` -> lists all databases

`use <databaseName>` -> connects to a specific database

`show collections` -> lists all the collections in an active database. 


#### Insert Documents

1. `db.<collectionName>.insert(<Object> or <Array of objects>)`

2. `db.<collectionName>.insertOne(<Object>)`
    
      ```javascript 
      db.<collectionName>.insertOne( {} )
      ```


3. `db.<collectionName>.insertMany(<Array of objects>)`
    
      ```javascript 
      db.<collectionName>.insertMany( [ {}, {}, {}] )
      ```
