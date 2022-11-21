# Mongo-DB

## What is mongoDB?

<img width="431" alt="Screenshot 2022-11-21 105449" src="https://user-images.githubusercontent.com/77200870/203020754-abe64d92-6008-48fc-be58-8a360a5f0523.png">

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

