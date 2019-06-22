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
      
### Operators

1. Comparison Operators
    $eq |  $neq | $gt |  $gte | $lt |  $lte | $in | $nin 
    --- | --- | --- | --- | --- | --- | --- | --- 


    Syntax: `{ <fieldName> : { <operator> : <value>}}`

    Examples: 
    ```
    { "favoriteFruit" : { "$ne" : "apple"} }
    { "age" : { "$gt" : 23 } }
    { "color" : {"$in" : ["green", "blue"] } }

    ```

2. AND Operator ($and)

    Logically combines multiple conditions. Resulting documents must match **ALL** conditions. 

    Syntax: ``` { $and : [ { <condition1> } , { <condition2> }, ..] } ```

    Examples: 
    ```javascript
    { $and : [ { "gender" : "male" } , { "age" : "25" } ] }
    ```

3. OR Operator ($or)

    Logically combines multiple conditions. Resulting documents must match **ANY** any of the condition. 

    Syntax: ``` { $or : [ { <condition1> } , { <condition2> }, ..] } ```

    Examples: 
    ```javascript
    { $or : [ { "gender" : "male" } , { "age" : "25" } ] }
    { $or : [ { "gender" : "male" } , { "age" : {"$in" : [20,25] } } ] }
    ```

4. Element Operators ($exists , $type)

    Examples: 
    ```javascript
    { "tags" : { $exists: true } }
    { "posts" : { $type : "int"} }
    ```
    
    
### Fields Filtering (Projections)

   Example: 
   ```javascript
    db.persons.find( {query}, { name : 1, age : 1, gender : 1} )
   ```
       
### Update Documents

Syntax: 

1. `db.<collectionName>.update( <query>,<update>,<options> )`

2. `db.<collectionName>.updateOne( <query>,<update>,<options> )`

3. `db.<collectionName>.updateMany( <query>,<update>,<options> )`

Update can be done using **$set** and **$unset** operator. 

a) $set
   Examples: 

   update the first document where **item** equals **"paper"**:
   
   ```javascript
        db.inventory.updateOne(
                //query
               { item: "paper" },
                //update
               {
                 $set: { "size.uom": "cm", status: "P" },
                 $currentDate: { lastModified: true }
               },
               // update options (optional)
               {}
            )
   ```


   update all documents where **qty** is **less than 50**.

   ```javascript
        db.inventory.updateMany(
               { "qty": { $lt: 50 } },
               {
                 $set: { "size.uom": "in", status: "P" },
                 $currentDate: { lastModified: true }
               }
            )
   ```

b) $unset -> removes size.uom and status from inventory for items having quantity less than 50
   Examples: 
   Remove 
   
   ```javascript
       db.inventory.update(
              { "qty": { $lt: 50 } },
               {
                 $unset: { "size.uom": 1, status: 1 }
               }
            )
   ```

c) Combine $set and $unser operators
   Examples: 
   Remove 
   
   ```javascript
       db.inventory.update(
              { "qty": { $lt: 50 } },
               {
                 $set: { "size.uom": "in"
                          , status: "P" },
                 $unset: { "name": 1 }
               }
            )
   ```   
   
#### Replace One Document

Replaces a single document within the collection based on the query.

Syntax: `db.<collectionName>.replaceOne( <query>,<replacementObject>,<options> )`

Example:

   ```javascript
        try {
           db.restaurant.replaceOne(
              //query  
              { "name" : "Central Perk Cafe" },
              //replacement object
              { "name" : "Central Pork" }
           );
        } catch (e){
           print(e);
        }
   ```
