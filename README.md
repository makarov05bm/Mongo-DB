# [01] Introduction

## Course Outline

<img width="548" alt="Screenshot 2022-11-21 115916" src="https://user-images.githubusercontent.com/77200870/203033899-7c69f02c-ea8e-446a-9d81-0c86a35efee4.png">

## What is mongoDB?

<img width="431" alt="Screenshot 2022-11-21 105449" src="https://user-images.githubusercontent.com/77200870/203020754-abe64d92-6008-48fc-be58-8a360a5f0523.png">

<img width="529" alt="Screenshot 2022-11-21 120546" src="https://user-images.githubusercontent.com/77200870/203035129-30dddb0f-928e-485c-a3b0-a52b5bae218d.png">

- Derived from *Humongous* because it can store lots of data
- Inside documents we use JSON (BJSON) data format
- Documents allow us to create complex relashions between data and store them in one document

<img width="301" alt="Screenshot 2022-11-21 105830" src="https://user-images.githubusercontent.com/77200870/203030556-525e9e36-3708-4d19-bcb8-6421d47610f4.png">

## Key mongoDB Characteristics

- Schemaless
- No/Few Relashions

## MongoDB Ecosystem

<img width="523" alt="Screenshot 2022-11-21 110736" src="https://user-images.githubusercontent.com/77200870/203023247-f6bd26a0-aadb-4c36-bde4-87c5a1c777ae.png">

## mongoDB configuration

> Install mongoDB server community edition

> Add a /data/db folder in the root (for Windows C:)

> Add an environment variable to the `bin` folder in mongoDB installation folder in PATH

> `net stop MongoDB` to stop the running service and start it manually when needed

> run `mongod` if /data/db is in the root, else run `mongod --dbpath "C:\Program Files\MongoDB\Server\4.0\data\db"`

> run `mongo` to enter the mongo shell which is the environment where we can run commands against the database

## Starting with the shell

### Basic commands

#### List all available databases

> show dbs

#### Switch to a database

(even if does not exist it will be created on teh fly)

> use db_name

#### Delete a database

> use db_name

> db.dropDatabase()

#### Insert a document into a collection 

even if the collection does not exist it will be created

> db.collection_name.insertOne({name: "Max", age: 19})

#### List all documents in a collection

> db.collection_name.find()

(List them in a pretty way)

> db.collection_name.find().pretty()

#### Free a collection

> db.collection_name.deleteMany({})

#### Drop a collection

> db.collection_name.drop()

#### Get Statistics about the database

> use db_name

> db.stats()

## Shell vs Drivers

https://www.mongodb.com/docs/drivers/

- The Shell is language-neutral
- Drivers are ODMs (object-data-mapper) we use to interact with mongoDB from our applications written in diffrent languages (NodeJS, Python...)

## MongoDB + Clients the big picture

<img width="547" alt="Screenshot 2022-11-21 115136" src="https://user-images.githubusercontent.com/77200870/203032720-1e524e3c-b784-426d-8ad1-f2d7e7e1291b.png">

<img width="520" alt="Screenshot 2022-11-21 115322" src="https://user-images.githubusercontent.com/77200870/203032735-996357d3-d893-4af4-a4bb-50c68257b6d0.png">

-----------------------------------------------------------------------

# [02] Basics & Basic CRUD Operations

- Basics about collections & documents
- Basic data types
- Performing CRUD operations

## JSON vs BSON

<img width="540" alt="Screenshot 2022-11-21 122455" src="https://user-images.githubusercontent.com/77200870/203038752-42a1ec80-130c-4849-a548-9377f5ffcfe3.png">

### _id

- This field is automatically assigned by mongoDB, but we can override it with any value we want
- ObjectId is not a valid JSON data type, but mongoDB drivers convert it into a valid type in BSON format 

## CRUD Operations

<img width="531" alt="Screenshot 2022-11-21 211340" src="https://user-images.githubusercontent.com/77200870/203149804-a406500e-0115-4bfa-a446-e9e2eff5dc76.png">

### Updation

#### Update one document

> db.collection_name.updateOne({name: "yakoub"}, {$set: {name: "inv"}})

#### Update many documents

> db.collection_name.updateMany({age: 19}, {$set: {canDrive: true}})

#### Entirely replace the document

After this the document will contain only the `_id` and `name` fields

> db.collection_name.update({_id: ObjectId("3243fd4234r324545")}, {name: "Oussama"})

**⚠️ A better approach:**

> db.collection_name.replaceOne({_id: ObjectId("3243fd4234r324545")}, {name: "Oussama", age: 19})

### Deleteion

#### Delete one document

> db.collection_name.deleteOne({name: "Oussama"})

#### Delete many documents

> db.collection_name.deleteMany({})

### Insertion

#### Insert many documents

we pass an array of documents

> db.collection_name.insertMany([{...}, {...}, ...])

### Finding

#### Return all matching elements

> db.collection_name.find({distance: {$gt: 10000}}).pretty()

#### Return the first matching element

> db.collection_name.findOne({distance: {$gt: 10000}}).pretty()

**⚠️ find() and the Cursor**

> find() in reality returns a cursor to the data, not an array, and the shell by default only displays us with the first 20 documents, but if we want more documents, we run the `toArray()` method on the cursor or instead and better, we use the `forEach((data) => {prindjson(data)}) method`

## Projection

<img width="461" alt="Screenshot 2022-11-21 215122" src="https://user-images.githubusercontent.com/77200870/203155826-4cdf4c12-fa20-42d6-b38a-3fb5a4cb24bc.png">

> db.collection_name.find({}, {field1: 1, field2: 1})

⚠️ This will return the _id of each document as well, so we have to explicitly exclude the _id

> db.collection_name.find({}, {age: 19, _id: 0})

## Embedded Documents

<img width="526" alt="Screenshot 2022-11-21 215851" src="https://user-images.githubusercontent.com/77200870/203157035-66c3f22b-af56-458a-ba3e-c885ed97b867.png">

<img width="538" alt="Screenshot 2022-11-21 215918" src="https://user-images.githubusercontent.com/77200870/203157105-a8c18f23-5b2d-4126-8ac2-9a178ee6a312.png"> 

> db.allflights.updateMany({}, {$set: {captain: {name: "max", age: 25, hobbies: ["sports", "coding", "hacking"]}}

```js
[
  {
    _id: ObjectId("637bdf3779ed705cc635375a"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true,
    captain: {
      name: 'max',
      age: 25,
      hobbies: [ 'sports', 'coding', 'hacking' ]
    }
  },
  {
    _id: ObjectId("637bdf3779ed705cc635375b"),
    departureAirport: 'LHR',
    arrivalAirport: 'TXL',
    aircraft: 'Airbus A320',
    distance: 950,
    intercontinental: false,
    captain: {
      name: 'max',
      age: 25,
      hobbies: [ 'sports', 'coding', 'hacking' ]
    }
  }
]
```

### Accessing embedded documents

> db.allflights.find({"captain.name": "max"})

## Module Summary

<img width="530" alt="Screenshot 2022-11-21 221655" src="https://user-images.githubusercontent.com/77200870/203159843-540c3a0a-9dad-4e41-9c05-63592d524b4b.png">

-------------------------------------------------------------------

# [03] Schemas & Relations How to Structure Documents

## What's In This Module

- Understading Document Schemas & Data Types
- Modelling Relashions
- Schema Validation

<img width="495" alt="Screenshot 2022-11-23 212122" src="https://user-images.githubusercontent.com/77200870/203639355-4203d68a-1313-4ecc-8a8a-41550ae12e8c.png">

we can have these two documents in the same collection!

```js
  {
    _id: ObjectId("637e80685a49faf9d87c5c59"),
    name: 'book',
    price: 12.99
  },
  {
    _id: ObjectId("637e808d5a49faf9d87c5c5a"),
    title: 't-shirt',
    seller: { name: 'oussama', age: 20 }
  }
```

> While it's not necessary to have a schema, it would be better to have one so that we work more effeciently with the data in the backend

## Available Approaches do structure collections

<img width="532" alt="Screenshot 2022-11-23 212554" src="https://user-images.githubusercontent.com/77200870/203640077-d685bde6-28b8-4c54-b261-268082733f5c.png">

> The cases in the middle and on the right are the ones we see more often

**⚠️ If we want to take a SQLish approach, we add the fields that are not common, in all the documents but if a document does not use it, we set it to `null`**

## Data Types

**⚠️ Max size of each document is 16MB**

- Text: "Yacoub"
- Boolean: true, false
- Number
  + Integer(int32)
  + NumberInt()
  + NumberLong(int64)
  + NumberDecimal (high precision floating point number)
- ObjectId
- ISODate
  + Timestamp
- Embedded document
- Arrays

<img width="521" alt="Screenshot 2022-11-23 214216" src="https://user-images.githubusercontent.com/77200870/203642568-123d1d64-25fa-4bd4-a100-95b2635586da.png">

**Eample:**

```js
db.company.insertOne({name: "Fresh Apples Inc", isStartup: true, employeesCount: 33, funding: 12345678901234567890, details: {ceo: "Mark"}, tags: [{title: "super"}, {title: "great"}], foundingDate: new Date(), insertedAt: new Timestamp()})
```

## Data Schemas & Data Modelling

<img width="544" alt="Screenshot 2022-11-23 220612" src="https://user-images.githubusercontent.com/77200870/203646064-20cc1f9b-18eb-4805-b3a5-0800e46bf57e.png">

## Relashions

### Two tastes of relashions

<img width="530" alt="Screenshot 2022-11-24 184035" src="https://user-images.githubusercontent.com/77200870/203841907-85158926-4c3b-454d-80f2-75ce3ea5104f.png">

<img width="530" alt="Screenshot 2022-11-23 220852" src="https://user-images.githubusercontent.com/77200870/203646428-7af7fb32-7af8-4d90-a4d4-4b003055fb58.png">

> If the data we want to refrence does not affect the document when changes, then we embed it, else we should only refrence it with the **id**

#### Example #1: One To One Relashion

**⚠️ Best Approach: Embedded refrenced document**

**⚠️ An Application Driven Approach:** For example, we have `users` and `cars` which is refrenced through `car's id` in the users documents, if our application needs to display all cars and all users seperatly, then it will be a lot more efficient to split the two collections apart

<img width="503" alt="Screenshot 2022-11-23 221144" src="https://user-images.githubusercontent.com/77200870/203646882-8e53f3cf-bc9f-402d-badd-3fc6111b83d2.png">

#### Example #2: One To Many Relashion

<img width="488" alt="Screenshot 2022-11-24 180954" src="https://user-images.githubusercontent.com/77200870/203837620-596d5a1d-4c57-478c-937f-b115b3d570ee.png">

**⚠️ Best Approach: Embedded refrenced document;** because we never fetch the answers alone without the thread, the thread always comes along with the answers.

#### Example #3: One To Many Relashion

<img width="482" alt="Screenshot 2022-11-24 181204" src="https://user-images.githubusercontent.com/77200870/203837934-2e1b1be9-1470-44bc-8a59-9813c59c4f1f.png">

**⚠️ Best Approach: Refrence documents;** because if we just want to fetch the cities for example we will not need to fetch all the citizens as well which can be a lot of data over the wire. So we just refrence the city for each citizen.

```js
db.cities.insertOne({name: "Baverlley", coor: {lat: 23, lng: 55}})
```

```js
db.citizens.insertMany([{name: "oussama", age: 20, cityId: ObjectId("637fa78b5a49faf9d87c5c61")}, {name: "yakoub", age: 25, cityId: ObjectId("637fa78b5a49faf9d87c5c61")}])
```

#### Example #4: Many To One Relashion

> Typically we model many-to-many relashions by refrences

> But, we should first ask, how often do we change our data, and when we change it do we have to change it everywhere.

<img width="483" alt="Screenshot 2022-11-24 182149" src="https://user-images.githubusercontent.com/77200870/203839356-36a6bd60-682e-475f-a716-8b761b46d732.png">

**⚠️ Best Approach: Either works;** because we don't need to have the latest version of the products in the `orders` array inside the `customer` document.

<img width="487" alt="Screenshot 2022-11-24 183027" src="https://user-images.githubusercontent.com/77200870/203840638-e07eef9d-fa7a-4df5-bbe9-c85e4e188125.png">

**⚠️ Best Approach: Refrence documents;** because we need the latest version of data.

## Merging Related Documents Split Up Using Refrences

### lookup()

<img width="446" alt="Screenshot 2022-11-24 184628" src="https://user-images.githubusercontent.com/77200870/203842748-03263640-1fd2-4e2e-b573-c0661b0e40ec.png">

**Authors**

```js
{
    _id: ObjectId("637fab1d5a49faf9d87c5c65"),
    name: 'robert',
    age: 30,
    address: { street: 'main', state: 'TX' }
  },
  {
    _id: ObjectId("637fab1d5a49faf9d87c5c66"),
    name: 'fellip',
    age: 40,
    address: { street: 'main-str', state: 'NY' }
  }
```

**Books**

```js
{
    _id: ObjectId("637faab45a49faf9d87c5c64"),
    title: 'cipher',
    authors: [
      ObjectId("637fab1d5a49faf9d87c5c65"),
      ObjectId("637fab1d5a49faf9d87c5c66")
    ]
}
```

**Merging**

```js
db.books.aggregate([
    {$lookup: {
                from: "authors", 
                localField: "authors", 
                foreignField: "_id", 
                as: "creators"
               }
    }])
```

**Result**

```js
{
    _id: ObjectId("637faab45a49faf9d87c5c64"),
    title: 'cipher',
    authors: [
      ObjectId("637fab1d5a49faf9d87c5c65"),
      ObjectId("637fab1d5a49faf9d87c5c66")
    ],
    creators: [
      {
        _id: ObjectId("637fab1d5a49faf9d87c5c66"),
        name: 'fellip',
        age: 40,
        address: { street: 'main-str', state: 'NY' }
      },
      {
        _id: ObjectId("637fab1d5a49faf9d87c5c65"),
        name: 'robert',
        age: 30,
        address: { street: 'main', state: 'TX' }
      }
    ]
}
```

## Schema Validation

<img width="363" alt="Screenshot 2022-11-24 210404" src="https://user-images.githubusercontent.com/77200870/203857455-9438c942-6fed-4166-b087-f5269e68937e.png">

<img width="527" alt="Screenshot 2022-11-24 210542" src="https://user-images.githubusercontent.com/77200870/203857593-713781a7-267f-4615-b92f-8f35cb84c7d2.png">

> db.createCollection(collection_name, {validation rules})

### Adding Schema Validation When Creating the Collection

```js
db.createCollection("posts", {
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["title", "text", "writer", "comments"],
            properties: {
                title: {
                    bsonType: "string",
                    description: "muest be a string and is required"
                },
                text: {
                    bsonType: "string",
                    description: "must be a string and is required"
                },
                writer: {
                    bsonType: "objectId",
                    description: "must be an onjectId and is required"
                },
                comments: {
                    bsonType: "array",
                    description: "must be an array and is required",
                    items: {
                        bsonType: "object",
                        required: ["text", "author"],
                        properties: {
                            text: {
                                bsonType: "string",
                                description: "must be a string and is required"
                            },
                            author: {
                                bsonType: "objectId",
                                description: "muest be an objectId and is required"
                            }
                        }
                    }
                }
            }
        }
    }
})
```

### Adding Schema Validation After Creating the Collection

> db.runCommand({collMod: "collection_name", validator: {validation schma}, validationAction: "error"||"warn"})

**⚠️ When the `validationType` is set to `warn` then even if the document is not valid it gets inserted and a warning gets added to the file**

```js
db.runCommand({
    collMod: "posts",
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["title", "text", "writer", "comments"],
            properties: {
                title: {
                    bsonType: "string",
                    description: "muest be a string and is required"
                },
                text: {
                    bsonType: "string",
                    description: "must be a string and is required"
                },
                writer: {
                    bsonType: "objectId",
                    description: "must be an onjectId and is required"
                },
                comments: {
                    bsonType: "array",
                    description: "must be an array and is required",
                    items: {
                        bsonType: "object",
                        required: ["text", "author"],
                        properties: {
                            text: {
                                bsonType: "string",
                                description: "must be a string and is required"
                            },
                            author: {
                                bsonType: "objectId",
                                description: "muest be an objectId and is required"
                            }
                        }
                    }
                }
            }
        }
    },
    validationAction: "warn"
})
```

## Round Up

<img width="495" alt="Screenshot 2022-11-24 214809" src="https://user-images.githubusercontent.com/77200870/203861159-11167efd-bc59-4d38-ad3d-9c8ca62562f8.png">

<img width="530" alt="Screenshot 2022-11-24 215017" src="https://user-images.githubusercontent.com/77200870/203861172-50b2b1e7-291d-4074-b3bd-1058909be771.png">

----------------------------------------------------------------------------

# [04] The Shell & The Server

### What's inside this module?

- Start mongoDB server as a process or as a service
- Configuring database & log path (and mode)
- Fixing issues

## Server Configuration

> mongod --help

Change the path where the database files are saved

> mongod --dbpath path

Set the logs file

> mongod --logpath path/logs.log

Run the `mongod` server as a background process (works only on mac and linux)

> mongod --fork --logpath path

⚠️ For Windows, when we install the `mongod` server, the service is already running, to stop it we go to the task manager and end the service `MongoDB` or we run on the cmd as admin the command `net stop MongoDB` or we start `mongo` and run the command `db.shutdownServer()`, to start it again, we run the cmd as admin and run `net start MongoDB`

<br/>

> To add a configuration file for our settings, we go to bin and create a file `conf.cfg`

```js
systemLog:
    destination: file
    logAppend: true
    path: "C:\Program Files\MongoDB\Server\6.0\log\mongod.log"
storage:
    dbPath: "C:\Program Files\MongoDB\Server\6.0\data"
```

To use it

> mongod -f conf_path

## Shell Configuration

To get all the commands we can run on the database object

> db.help()

To get all the commands we can run on a collection

> db.test.help()

--------------------------------------------------------------------

# [05] Diving Deeper Into Document Creation

<img width="243" alt="Screenshot 2022-11-25 213053" src="https://user-images.githubusercontent.com/77200870/204052730-27b7be7f-ece6-4ca1-bafd-92024c372124.png">

## Creating Documents

<img width="526" alt="Screenshot 2022-11-25 213147" src="https://user-images.githubusercontent.com/77200870/204052785-e20a2a69-b620-4869-96ca-56a162cf0dad.png">

⚠️ insert() works with one document or an array of documents but it's prone to errors, and makes it hard to understand what's going in the code

<img width="523" alt="Screenshot 2022-11-25 213301" src="https://user-images.githubusercontent.com/77200870/204052885-48429369-b4e0-4de6-9909-5e43a2927d3c.png">

## Working With Ordered Inserts

When inserting multiple documents into the collection with `insertMany`, we can set an option which determines whether we continue inserting after a document fails to get inserted or not

> ordered: true        DON'T CONTINUE
> ordered: false       CONTINUE

```js
db.hobbies.insertMany([{ _id: "yoga", name: "Yoga" }, { _id: "cooking", name: "cooking" }, { _id: "food", name: "Food" }], {ordered: false})
```

## WriteConcern Option

<img width="533" alt="Screenshot 2022-11-25 215554" src="https://user-images.githubusercontent.com/77200870/204054542-4c2d0a5a-6560-4650-97ed-3f7c60a38017.png">

⚠️ `writeConcern` works for all insert, delete and update methods

**Syntax:** 

```js
{ w: <value>, j: <boolean>, wtimeout: <number> }
```

**Example:**
```js
db.persons.insertOne({name: "susan", age: 25}, {writeConcern: {w: 1, j: true, wtimeout: 200}})
```

## Atomicity

<img width="491" alt="Screenshot 2022-11-25 221232" src="https://user-images.githubusercontent.com/77200870/204055760-b58f10c7-6e5d-46b0-bfa5-798caebfc335.png">

## Importing Data

- jsonArray: we are importing an array of documents, not just one document
- drop: if a collection with this name already exists, then delete it and replace it with this new one

```js
mongoimport path_to_json_file -d movieDB -c movies --jsonArray --drop
```

## Read Operations

- Methods, Filters & Operators
- Query Selectors
- Projection Operations

### Query Selectors

#### [01] Comparision Operators

- $eq : equal
- $ne : not equal
- $lt : lower than
- $lte: lower than or equal
- $gt : greater than
- $gte : greater than or equal
- $in : find the matching documents where the field is in the specified array `db.movies.find({runtime: {$in: [30, 42]}})`
- $nin : find the matching documents where the field is not in the specified array

#### [02] Querying Embedded Fields

```js
db.movies.find({"rating.average": {$gt: 9}})
```

> we can go deep in the embedding level as much as we want

#### [03] Querying Arrays

```js
db.movies.find({genres: "Drama"})
```

> This means that "Drama" is included in the `genres` array

⚠️ For exactly matching an array, we add the []

```js
db.movies.find({genres: ["Drama"]})
```

#### [04] Logical Operators

- $or : `db.movies.find({$or: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]})`
- $nor : neither of the conditions is met
- $and : `db.movies.find({$and: [{"rating.average": {$gt: 9}}, {genres: "Drama"}]})`

> The default behavior for find(...) is that it takes a bunch or filters and AND them, so why we have an `$and` operator

> We have it because if we want to do something like this `db.movies.find({genres: "Drama", genres: "Thrilling"})` we will get only the arrays that have 'Thrilling', the solution here is to use `$and` >> `db.movies.find({$and: [{genres: "Drama"}, {genres: "Thrilling"}]})`

- $not : look for the opposite or a query, `db.movies.find({runtime: {$not: {$eq: 60}}})`

#### [05] Element Operators

- $exists : check if a field exists >> `db.movies.find({age: {$exists: true}})`

	> Sometimes there are documents where the field exists but it's set to `null`, to bypass this >> `db.movies.find({age: {$exists: true, $ne: null}})`

- $type : the type of the field value >> `db.users.find({phone: {$type: "number"}})`

	> We can also pass an array of types to check any of them

	`db.users.find({phone: {$type: ["number", "string"]}})`

#### [06] Evaluation Operators

- $regex : look for a pattern in a text >> `db.movies.find({summary: {$regex: /musical/}})`
- $expr : compare two fields inside one document
- 
<img width="607" alt="Screenshot 2022-12-01 182912" src="https://user-images.githubusercontent.com/77200870/205120444-e4069eb0-b0e9-4a3c-8649-5c0174c08912.png">

```js
db.sales.find({$expr: {$gt: ["$volume", "$target"]}})
```

```js
db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: {$subtract: ["$volume", 10]}, else: "$volume"}}, "$target"]}})
```

```js

let discountedPrice = {
   $cond: {
      if: { $gte: ["$qty", 100] },
      then: { $multiply: ["$price", NumberDecimal("0.50")] },
      else: { $multiply: ["$price", NumberDecimal("0.75")] }
   }
};
// Query the supplies collection using the aggregation expression
db.supplies.find( { $expr: { $lt:[ discountedPrice,  NumberDecimal("5") ] } });
```

**⚠️ Note: Even though $cond calculates an effective discounted price, that price is not reflected in the returned documents. Instead, the returned documents represent the matching documents in their original state.**

#### [07] Querying Arrays

We use `nested path syntax`

```js
db.users.find({"hobbies.title": "sports"})
```

#### [08] Array Query Selectors

- **$size:** matches any array with the number of elements specified by the argument (must be an exact number, we cannot pass a range of numbers or use `$gt`, `$lt`...)

```js
db.users.find({hobbies: {$size: 3}})
```

- **$all**

<img width="614" alt="Screenshot 2022-12-01 190503" src="https://user-images.githubusercontent.com/77200870/205127588-2c775ecd-74b1-4ac2-94e6-ac665711124a.png">

The following operation uses the `$all` operator to query the inventory collection for documents where the value of the tags field is an array whose elements include `appliance`, `school`, and `book`
 
```js
db.inventory.find( { tags: { $all: [ "appliance", "school", "book" ] } } )
```

- **$elemMatch**

<img width="620" alt="Screenshot 2022-12-01 224533" src="https://user-images.githubusercontent.com/77200870/205165599-cafec8c2-d37d-4347-a375-3cfc0d88f3d1.png">

<img width="626" alt="Screenshot 2022-12-01 225543" src="https://user-images.githubusercontent.com/77200870/205167533-de8dd876-8b0a-4b6d-9ee6-8c7e4370b4f4.png">

<img width="608" alt="Screenshot 2022-12-01 225558" src="https://user-images.githubusercontent.com/77200870/205167557-ed926922-a593-490c-9042-88030b24eeee.png">

<img width="617" alt="Screenshot 2022-12-01 225804" src="https://user-images.githubusercontent.com/77200870/205167745-265234be-9bb1-4934-a69f-82bee4aaf606.png">

<img width="602" alt="Screenshot 2022-12-01 225829" src="https://user-images.githubusercontent.com/77200870/205167753-188c8ed0-68b7-43c8-8ab4-d2d7b1baa6b9.png">

#### [07] Cursors

<img width="538" alt="Screenshot 2022-12-02 212339" src="https://user-images.githubusercontent.com/77200870/205379642-bc4031df-f4b8-4974-b532-9624dc54462e.png">

> The query `db.users.find()` returs a cursor

- If we want to fetch all the documents at one time: `db.users.find().forEach(user => printjson(user))` or `db.users.find().toArray()`
- If we want to fetch the next document: `db.users.find().next()` 
- Skip the next document: `db.users.find().skip()`
- Check if there is a document in the next position: `db.users.find().hasNext()`

**Sorting cursor's documents**

<img width="581" alt="Screenshot 2022-12-02 214516" src="https://user-images.githubusercontent.com/77200870/205383116-e97eeccb-ed33-420c-8ad2-287877268430.png">

**⚠️ You can sort on a maximum of 32 keys**

- 1 : ascending
- -1 : descending

```js
db.movies.find().sort({"rating.average": -1})
```

**⚠️ but what if we have multiple documents with a duplicate rating.average field value? This will cause the query to return diffrent order across multiple executions of the same sort**

> Then, we must specify another sorting criteria

```js
db.movies.find().sort({"rating.average: 1, runtime: -1"})
```

**Skipping and limiting cursor's documents**

<img width="467" alt="Screenshot 2022-12-04 214536" src="https://user-images.githubusercontent.com/77200870/205514613-6a9c43e8-c526-40d8-9078-782017aa33d4.png">

<img width="485" alt="Screenshot 2022-12-04 214635" src="https://user-images.githubusercontent.com/77200870/205514661-a792fdae-0c76-4224-9075-97aa37297f2e.png">

```js
function printStudents(pageNumber, nPerPage) {
  print( "Page: " + pageNumber );
  db.students.find()
             .sort( { _id: 1 } )
             .skip( pageNumber > 0 ? ( ( pageNumber - 1 ) * nPerPage ) : 0 )
             .limit( nPerPage )
             .forEach( student => {
               print( student.name );
             } );
}
```

#### [08] Projection

```js
db.movies.find({}, {name: 1, genres: 1, runtime: 1, rating: 1, _id: 0})
```

**Project embeded documents**

```js
db.movies.find({}, {name: 1, genres: 1, runtime: 1, "schedule.time": 1, _id: 0})
```

**Projection with arrays**

```js
db.movies.find({"rating.average": {$gt: 9}}, {genres: {$elemMatch: {$eq: "Horror"}}})
```

- $slice: The`$slice` projection operator specifies the number of elements in an array to return in the query result.

> $slice: `<number>`

> $slice: `[ <number to skip>, <number to return> ]`

```js
db.movies.find({"rating.average": {$gt: 9}}, {genres: {$slice: [1, 2]}, name: 1})
db.movies.find({"rating.average": {$gt: 9}}, {genres: {$slice: 2}, name: 1})
```

## Update Operations

https://www.mongodb.com/docs/manual/reference/operator/update/positional-filtered/#mongodb-update-up.---identifier--

- `$` positional operator for update operations
- `$[]` all positional operator for update operations
- `$[<identifier>]` filtered positional operator for update operations


**[+] upsert: An option for update operations; e.g. db.collection.updateOne(), db.collection.findAndModify(). If set to true, the update operation will either update the document(s) matched by the specified query or if no documents match, insert a new document. The new document will have the fields indicated in the operation**

### [01] Update matched array element

```js
db.users.updateMany({hobbies: {$elemMatch: {title: "sports", frequency: {$gte: 3}}}}, {$set: {"hobbies.$.highFrequency": true}})
```

### [02] Update multiple matching array elements

```js
 db.users.updateMany({age: {$gt: 19}}, {$inc: {"hobbies.$[].frequency": -1}})
```

### [03] Update exact matched array element

```js
db.users.updateMany({"hobbies.frequency": {$lte: 1}}, {$set: {"hobbies.$[element].done": true}}, {arrayFilters: [{"element.frequency": {$lt: 1}}]})
```

### [04] Update All Array Elements that Match Multiple Conditions

> Same arrayFilter document

```js
db.students3.updateMany({}, {$set: {"grades.$[element].std": 10}}, {arrayFilters: [{"element.grade": {$gte: 80}, "element.std": {$gte: 5}}]})
```

### [05] Adding elements to arrays

- $push: appends a specified value to an array.

> `{ $push: { <field1>: <value1>, ... } }`

<img width="625" alt="Screenshot 2022-12-05 215954" src="https://user-images.githubusercontent.com/77200870/205741733-5bff0767-2bdc-4d03-be78-be873b0813fd.png">

<img width="581" alt="Screenshot 2022-12-05 220133" src="https://user-images.githubusercontent.com/77200870/205741939-18637c99-e883-4112-8eb6-8ecad2893fc9.png">

#### Append a Value to an Array

```js
db.students.updateOne(
   { _id: 1 },
   { $push: { scores: 89 } }
)
```

#### Append a Value to an array but make it unique

```js
db.users.updateOne({name: 'Maria'}, {$addToSet: {hobbies: {title: "hiking", frequency: 2}}})
```

#### Append a Value to Arrays in Multiple Documents

```js
db.students.updateMany(
   { },
   { $push: { scores: 95 } }
)
```

#### Append Multiple Values to an Array

```js
db.students.updateOne(
   { name: "joe" },
   { $push: { scores: { $each: [ 90, 92, 85 ] } } }
)
```

#### Use `$push` Operator with Multiple Modifiers

The following `$push` operation uses:

- the `$each` modifier to add multiple documents to the quizzes array

- the `$sort` modifier to sort all the elements of the modified quizzes array by the score field in descending order

- the `$slice` modifier to keep only the first three sorted elements of the quizzes array

```js
db.students.updateOne(
   { _id: 5 },
   {
     $push: {
       quizzes: {
          $each: [ { wk: 5, score: 8 }, { wk: 6, score: 7 }, { wk: 7, score: 6 } ],
          $sort: { score: -1 },
          $slice: 3
       }
     }
   }
```

### [06] Deleting elements from arrays

- $pull: removes from an existing array all instances of a value or values that match a specified condition.

<img width="576" alt="Screenshot 2022-12-06 221608" src="https://user-images.githubusercontent.com/77200870/206024748-3a92d671-55db-459f-be1b-6838aa1eca6c.png">

```js
db.stores.insertMany( [
   {
      _id: 1,
      fruits: [ "apples", "pears", "oranges", "grapes", "bananas" ],
      vegetables: [ "carrots", "celery", "squash", "carrots" ]
   },
   {
      _id: 2,
      fruits: [ "plums", "kiwis", "oranges", "bananas", "apples" ],
      vegetables: [ "broccoli", "zucchini", "carrots", "onions" ]
   }
] )
```

#### Remove All Items That Equal a Specified Value

```js
db.stores.updateMany(
    { },
    { $pull: { fruits: { $in: [ "apples", "oranges" ] }, vegetables: "carrots" } }
)
```

#### Remove last or first element of an array

```js
db.stores.updateOne({_id: 2}, {$pop: {fruits: 1}})
```

--------------------------------------------------------------------------------

# [07] Working With Indexes

## What are indexes?

> Indexes drastically speed up queries

<img width="543" alt="Screenshot 2022-12-07 103211" src="https://user-images.githubusercontent.com/77200870/206142449-258715fe-1844-4113-80f1-7707d284f4b3.png">

**⚠️ Don't use too many indexes**

<img width="537" alt="Screenshot 2022-12-07 103442" src="https://user-images.githubusercontent.com/77200870/206142502-2b4f9fc6-f029-45ce-bd1a-03b5845285e8.png">

### explain()

<img width="584" alt="Screenshot 2022-12-07 104630" src="https://user-images.githubusercontent.com/77200870/206145081-ac38e8ac-ce5a-4b70-8cfd-d9255f15a966.png">

```js
db.contacts.explain('executionStats').find({"dob.age": {$gt: 60}})
```

### Create Indexes

- 1: sort the list of index list in a descending order
- -1: sort the list of index list in a ascending order

```js
db.contacts.createIndex({'dob.age': 1})
```

<br/>

#### What does createIndex() do in detail?

Whilst we can't really see the index, you can think of the index as a simple list of values + pointers to the original document. <br/>

Something like this (for the "age" field):<br/>

(29, "address in memory/ collection a1")<br/>

(30, "address in memory/ collection a2")<br/>

(33, "address in memory/ collection a3")<br/>

The documents in the collection would be at the "addresses" a1, a2 and a3. The order does not have to match the order in the index (and most likely, it indeed won't).<br/>

The important thing is that the index items are ordered (ascending or descending - depending on how you created the index). createIndex({age: 1}) creates an index with ascending sorting, createIndex({age: -1}) creates one with descending sorting.
<br/>
MongoDB is now able to quickly find a fitting document when you filter for its age as it has a sorted list. Sorted lists are way quicker to search because you can skip entire ranges (and don't have to look at every single document).
<br/>
Additionally, sorting (via sort(...)) will also be sped up because you already have a sorted list. Of course this is only true when sorting for the age.
<br/>

### Delete Indexes

```js
db.contacts.dropIndex({"dob.age": 1})
```

**⚠️ If we have queries that retrieve all the documents or almost all of them then indexes will slow down the query instead of speeding it up**

### Compound Indexes

> speed up queries that use multiple fields to retrive documents

```js
db.contacts.createIndex({"dob.age": 1, gender: 1})
```

> This will sort the `gender` field within the `dob.age` field

> So we can use the index to find documents by the left field (age) ot both (age and gender), but we can't find documents by the `gender` only

**⚠️ If we run a find query using only `gender`, mongoDB will do a `COLLSCAN` and not a `IXSCAN`**

### Using Indexes for sorting

> When we have millions of documents and we want to sort them, the sort can not happen because mongoDB by default have 32mb memory, and when we sort, we first fetch all the documents, put them into memory and then sort, which is impossible for millions of documents.

> **Solution:** using an index with the field we want to sort with.

```js
db.contacts.explain().find({"dob.age": 35}).sort({gender: 1})
```

### The default index

> The default index uses: `_id`

To get all indexes on a collection: `db.contacts.getIndexes()`

### Configuring Indexes

#### Unique Indexes

```js
db.contacts.createIndex({email: 1}, {unique: true})
```

#### Partial Filters

Partial indexes only index the documents in a collection that meet a specified filter expression. By indexing a subset of the documents in a collection, partial indexes have lower storage requirements and reduced performance costs for index creation and maintenance.

<br/>

**Create a Partial Index**

To create a partial index, use the `db.collection.createIndex()` method with the `partialFilterExpression` option. The `partialFilterExpression` option accepts a document that specifies the filter condition using:

- equality expressions (i.e. field: value or using the `$eq` operator),
- `$exists`: true expression,
- `$gt`, `$gte`, `$lt`, `$lte` expressions,
- `$type` expressions,
- `$and` operator at the top-level only

```js
db.restaurants.createIndex(
   { cuisine: 1, name: 1 },
   { partialFilterExpression: { rating: { $gt: 5 } } }
)
```

**Behavior**

<img width="489" alt="Screenshot 2022-12-10 141957" src="https://user-images.githubusercontent.com/77200870/206857289-cd38ee20-79fc-4651-b52c-6272f9985e3e.png">

<img width="490" alt="Screenshot 2022-12-10 142013" src="https://user-images.githubusercontent.com/77200870/206857291-f9aba7e2-b129-4cb1-b45c-7207373a12c3.png">

#### Special case for unique and partialIndexExpression

When we add an index and it's unique, if we add new documents where that field is "null" then we will get an error because "non-existing" fields are unique themselves.

> **Solution:** using `partialIndexExpression`

```js
db.users.createIndex({email: 1}, {unique: true, partialIndexExpression: {email: {$exists: true}}})
```

> This means that we use this index only when the `email` field is present

### Time-To-Live (TTL) indexes

> This document provides an introduction to MongoDB's "time to live" or `TTL` collection feature. `TTL` collections make it possible to store data in MongoDB and have the mongod automatically remove data after a specified number of seconds or at a specific clock time.

> Data expiration is useful for some classes of information, including machine generated event data, logs, and session information that only need to persist for a limited period of time.

```js
db.sessions.createIndex({createdAt: 1}, {expireAfterSeconds: 10})
```

<img width="487" alt="Screenshot 2022-12-10 145400" src="https://user-images.githubusercontent.com/77200870/206858781-f73652a2-4cea-4ae2-9485-8042824c3c69.png">

**⚠️ You can modify the expireAfterSeconds of an existing TTL index using the collMod command.**

**⚠️ If the field contains an array of BSON date-typed objects, data expires if at least one of BSON date-typed object is older than the number of seconds specified in `expireAfterSeconds`.**

**⚠️ Do not set expireAfterSeconds to NaN in your TTL index configuration.**

#### Identify misconfigured indexes.

```js
function getNaNIndexes() {
  const nan_index = [];

  const dbs = db.adminCommand({ listDatabases: 1 }).databases;

  dbs.forEach((d) => {
    const listCollCursor = db
      .getSiblingDB(d.name)
      .runCommand({ listCollections: 1 }).cursor;

    const collDetails = {
      db: listCollCursor.ns.split(".$cmd")[0],
      colls: listCollCursor.firstBatch.map((c) => c.name),
    };

    collDetails.colls.forEach((c) =>
      db
        .getSiblingDB(collDetails.db)
        .getCollection(c)
        .getIndexes()
        .forEach((entry) => {
          if (Object.is(entry.expireAfterSeconds, NaN)) {
            nan_index.push({ ns: `${collDetails.db}.${c}`, index: entry });
          }
        })
    );
  });

  return nan_index;
};

getNaNIndexes();
```

### Query Diagnosis & Query Planning

<img width="545" alt="Screenshot 2022-12-10 151310" src="https://user-images.githubusercontent.com/77200870/206859717-c025cc9d-6d7b-4d68-8fbf-ac7d351e4a34.png">

<img width="526" alt="Screenshot 2022-12-10 151437" src="https://user-images.githubusercontent.com/77200870/206859721-a0b33286-2862-47e1-ad27-fa417bf868d3.png">

#### Coverd Query

A covered query is a query that can be satisfied entirely using an index and does not have to examine any documents. An index covers a query when all of the following apply:

- all the fields in the query are part of an index.

- all the fields returned in the results are in the same index.

- no fields in the query are equal to `null` (i.e. `{"field" : null}` or `{"field" : {$eq : null}}`).

**Example:**

```js
db.customers.createIndex({"name": 1})
```

```js
db.customers.explain('executionStats').find({name: 'max'}, {_id: 0, name: 1})
```

**⚠️ For the specified index to cover the query, the projection document must explicitly specify `_id: 0` to exclude the _id field from the result since the index does not include the _id field.**

**Covered Query in Embedded Documents:**

For example, consider a collection `userdata` with documents of the following form:

```js
{ _id: 1, user: { login: "tester" } }
```

The collection has the following index:

```js
{ "user.login": 1 }
```

The `{ "user.login": 1 }` index will cover the query below:

```js
db.userdata.find( { "user.login": "tester" }, { "user.login": 1, _id: 0 } )
```
