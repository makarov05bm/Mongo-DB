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

#### Insert a document into a collection 

even if the collection does not exist it will be created

> db.collection_name.insertOne({name: "Max", age: 19})

#### List all documents in a collection

> db.collection_name.find()

(List them in a pretty way)

> db.collection_name.find().pretty()

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

