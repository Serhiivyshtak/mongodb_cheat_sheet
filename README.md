<h1 id="top" style="font-weight: bold; color: green;
text-decoration: underline;">Mongo DB Cheat Sheet<h1>

<h2 style="font-style: italic; color: lightgreen">Contents</h2>

- <a href="#1">Show databases</a>
- <a href="#2">Switch databases</a>
- <a href="#3">Show collections</a>
- <a href="#4">Insert data</a>
- <a href="#5">Simple data reading</a>
- <a href="#6">Length of a collection</a>
- <a href="#7">Comparasion, existence and logical operators</a>
- <a href="#8">Data sorting and limiting</a>
- <a href="#9">Querying nested arrays and documents</a>

<h2 id="1" style="font-style: italic; color: lightgreen">Show databases</h2>

```js
dbs // prints all databases
db // prints current one
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>

<h2 id="2" style="font-style: italic; color: lightgreen">Switch databases</h2>

```js
use <database name>
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>

<h2 href="3" style="font-style: italic; color: lightgreen">Show collections</h2>

```js
show collections
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>

<h2 id="4" style="font-style: italic; color: lightgreen">Insert data</h2>


```js
db.collection.insertOne({first_name: "Serhii", second_name: "Vyshtak"}) // inserts a single document

db.collection.insertMany([{first_name: "Serhii", second_name: "Vyshtak"}, {first_name: "Alexander", second_name: "Poplovski"}]) // inserts multiple documents

db.collection.insertOne({created_at: ISODate()}) // generates automatically time on which inserted
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>

<h2 id="5" style="font-style: italic; color: lightgreen">Simple data reading</h2>

```js
db.collection.findOne() // finds very first document in a collection

db.collection.findOne({second_name: "Poplovski"}) // finds first document in a collection which has property "second_name" and it equals to "Poplovski"

db.collection.findOne({}, {first_name: 1}) // returns only "first_name" field from very first document in a collection

db.collection.findOne({second_name: "Poplovski"}, {first_name: 1}) // returns only "first_name" field from first document which has property "second_name" and it equals to "Poplovski"

db.collection.find() // Takes all of the documents inside of a collection and shows first 20 of them. To see next 20, write "it" in th shell. If amount of documents is less than 20, only existing ones will be shown

db.collection.find({first_name: "Serhii"}) // finds all the documents which have prperty "first_name" and the equal to "Serhii"

db.collection.find({}, {first_name: 1, second_name: 1}) // returns fields "first_name" and "second_name" from all the documents in a collection
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>

<h2 id="6" style="font-style: italic; color: lightgreen">Length of a collection</h2>

```js
db.collection.find().count() // counts all of the documents inside a collection

db.collection.countDocuments() // same as previous method

db.collection.find({age: 19}).count() // counts all documents, which have field "age" and it equals to 19

db.collection.countDocuments({age: 19}) // same as previous method
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>

<h2 id="7" style="font-style: italic; color: lightgreen">Comparasion, existence and logical operators</h2>

```js
db.collection.find({age: {$eq: 18}}) // finds all documents, where age equals to 18 (is same as {age: 18})

db.collection.find({age: {$gt: 18}}) // finds all documents, where age is greater than 18 (>)

db.collection.find({age: {$gte: 18}}) // finds all documents, where age is greater or equals to 18 (>=)

db.collection.find({age: {$lt: 99}}) // finds all documents, where age is less than 99 (<)

db.collection.find({age: {$lte: 99}}) // finds all documents, where age is less or equals to 99 (<=)

db.collection.find({age: {$ne: 27}}) // finds all documents, where age does not equal to 27 (!=)

db.collection.find({age: {$in: [18, 19, 20]}}) // finds all documents, where age equals one of the items in specified array (18, 19, or 20)

db.collection.find({age: {$nin: [27, 38, 44]}}) // finds all documents, where age does not equal to one of the items in specified array (27, 38 or 44)

db.collection.find({$or: [{first_name: "Serhii"}, {second_name: "Lebidsky"}]}) // grabs all documents, which have either "first_name" = "Serhii", or "second_name" = "Lebidsky"

db.collection.find({$and: [{age: 18}, {profession: "programmer"}]}) // grabs all documents, which have "age" = 18 as well as "profession" = "programmer"

db.collection.find({hobbies: {$exists: true}}) //takes all documents, which have "hobbies" field
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>

<h2 id="8" style="font-style: italic; color: lightgreen">Data sorting and limiting</h2>

```js
db.collection.find({age: 18}).limit(10) // Takes all documents, which have age = 18 and outputs first 10 of them

db.collection.find().sort({age: 1}) // outputs all the documents from a collection in increasing order (from least to greatest)

db.collection.find().sort({age: -1}) // outputs all the documents from a collection in decreasing order (from greatest to least)

db.collection.find().sort({first_name: 1}) // outputs all the documents from a collection alphabetically (because "first_name" field stores string values)

db.collection.find().sort({first_name: -1}) // outputs all the documents from a collection in reversed alphabetical order
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>

<h2 id="9" style="font-style: italic; color: lightgreen">Querying nested arrays and documents</h2>

```js
// For these examples we will use "hobbies" property, which is an array, and "adress" property, which is an object(nested document):
{
    first_name: "Serhii",
    second_name: "Vyshtak",
    age: 18,
    hobbies: ["gym", "basketball", "coding"],
    adress: {
        city: "Berlin",
        street: "Am Bischofskreuz",
        house: 18
    }
}

db.collection.find({hobbies: "coding"}) // In this case "hobbies" field must not be exactly equal to "coding". Because "hobbies" is an array, this expression means: hobbies.inlcludes("coding"). It returns all the documents, which match this expression

db.collection.find({hobbies: {$all: ["coding", "gym"]}}) // returns all documents, which have "coding" and "gym" values inside of "hobbies" array

db.collection.find({hobbies: {$size: 2}}) // returns all documents, which have two items in "hobbies" array

db.collection.find({"adress.city": "Berlin"}) // this expression queries nested object "adress" and checks if it's property "city" equals to "Berlin".
```
<a href="#top" style="color: gray;"><sub>To the top</sub></a>
