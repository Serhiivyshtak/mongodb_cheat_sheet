<h1 id="top" style="font-weight: bold; color: green;
text-decoration: underline;">Mongo DB Cheat Sheet<h1>

<h2 style="font-style: italic; color: lightgreen">Contents</h2>

- <a href="#1">Show databases</a>
- <a href="#2">Switch databases</a>
- <a href="#3">Show collections</a>
- <a href="#4">Inserting data (C)</a>
- <a href="#5">Simple data reading (R)</a>
- <a href="#6">Length of a collection (R)</a>
- <a href="#7">Comparasion, existence and logical operators (R)</a>
- <a href="#8">Data sorting and limiting (R)</a>
- <a href="#9">Querying nested arrays and documents (R)</a>
- <a href="#10">Updating data (U)</a>

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

<h2 id="4" style="font-style: italic; color: lightgreen">Inserting data</h2>


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

<h2 id="10" style="font-style: italic; color: lightgreen">Updating data</h2>

```js
db.collection.updateOne({}, {$set: {sex: "male"}}) // updates very first document in a collection and sets a new property "sex"

db.collection.updateOne({}, {$rename: {"sex": "gender"}}) // updates very first document in a collection and renames a property "sex" into "gender"

db.collection.updateOne({}, {$unset: {gender: 1}}) // updates very first document in a collection and removes a property "gender"

db.collection.updateOne({first_name: "Serhii"}, {$inc: {age: 1}}) // takes a document with first_name = "Serhii" and increases age by 1 (+1)

db.collection.updateOne({bank_pin: 9132, bank_number: 9987123817239713}, {$inc: {bank_account: -100}}) // takes a document with bank_pin = 9132 and bank_number = 9987123817239713 and increases by -100, namely decreases by 100 (-100)

db.collection.updateOne({first_name: "Alexander"}, {$set: {phone_number: 082349872348}, $currentDate: {last_modified: true}}) // finds a document with first_name = "Alexander", sets a new field "phone_number" and creates one more field "last_modified", which contains date on which updated

db.collection.updateOne({first_name: "Alexander", second_name: "Antonovic"}, {$push: {hobbies: "cycling"}}) // grabs a document, which has first_name = "Alexander" and second_name = "Antonovic", and pushes a new item "cycling" into "hobbies" array

db.collection.updateOne({first_name: "Alexander", second_name: "Antonovic"}, {$pull: {hobbies: "reading"}}) // grabs a document, which has first_name = "Alexander" and second_name = "Antonovic", and removes item "reading" from "hobbies" array

db.collection.updateOne({first_name: "Serhii"}, {$pop: {hobbies: 1}}) // removes last item from "hobbies" array in a document which has first_name = "Serhii"

db.collection.updateOne({first_name: "Serhii"}, {$pop: {hobbies: -1}}) // removes first item from "hobbies" array in a document which has first_name = "Serhii"

db.collection.updateOne({username: "Girl_0729", password: "ias89udsfsn"}, {$push: {products: {$each: ["prada dress XS", "Nike Air Force 1"]}}}) // finds a document with username = "Girl_0729" and password = "ias89udsfsn", and pushes multiple values into "products" array, namely "prada dress XS" and "Nike Air Force 1"

db.collection.updateOne({username: "Girl_0729", password: "ias89udsfsn"}, {$pullAll: {products: ["prada dress XS", "Nike Air Force 1"]}}) // finds a document with username = "Girl_0729" and password = "ias89udsfsn", and removes "prada dress XS" and "Nike Air Force 1" from "products" array

db.collection.updateOne({first_name = "Serhii", second_name: "Vyshtak"}, {$set: {"adress.street": "Hnata Yury", "adress.house": 17}}) // finds a document with first_name = "Serhii" and second_name: "Vyshtak", and changes values "street" and "house" in the nested document called "adress"

db.collection.updateMany({age: {$gt: 18}}, {$set: {is_adult: true}}) // takes all the documents from a collection, which have age greater than 18 and updates "is_adult" property to true
```

<a href="#top" style="color: gray;"><sub>To the top</sub></a>
