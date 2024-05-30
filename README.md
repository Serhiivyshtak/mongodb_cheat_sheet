<style>
    .h {
        font-weight: bold;
        color: green;
        text-decoration: underline;
    }
    .sh {
        font-style: italic;
        color: lightgreen;
    }
</style>


<h1 class="h">Mongo DB Cheat Sheet<h1>

<h2 class="sh">Contents</h2>

- <a href="#1">Show databases</a>
- <a href="#2">Switch databases</a>
- <a href="#3">Show collections</a>
- <a href="#4">Insert data</a>
- <a href="#5">Simple data reading</a>


<h2 id="1" class="sh">Show databases</h2>

```js
dbs // prints all databases
db // prints current one
```

<h2 id="2" class="sh">Switch databases</h2>

```js
use <database name>
```

<h2 href="3" class="sh">Show collections</h2>

```js
show collections
```

<h2 id="4" class="sh">Insert data</h2>


```js
db.collection.insertOne({first_name: "Serhii", second_name: "Vyshtak"}) // inserts a single document

db.collection.insertMany([{first_name: "Serhii", second_name: "Vyshtak"}, {first_name: "Alexander", second_name: "Poplovski"}]) // inserts multiple documents

db.collection.insertOne({created_at: ISODate()}) // generates automatically time on which inserted
```

<h2 id="5" class="sh">Simple data reading</h2>

```js
db.collection.findOne() // finds very first document in a collection

db.collection.findOne({second_name: "Poplovski"}) // finds first document in a collection which has property "second_name" and it equals to "Poplovski"

db.collection.findOne({}, {first_name: 1}) // returns only "first_name" field from very first document in a collection

db.collection.findOne({second_name: "Poplovski"}, {first_name: 1}) // returns only "first_name" field from first document which has property "second_name" and it equals to "Poplovski"

db.collection.find() // Takes all of the documents inside of a collection and shows first 20 of them. To see next 20, write "it" in th shell. If amount of documents is less than 20, only existing ones will be shown

db.collection.find({first_name: "Serhii"}) // finds all the documents which have prperty "first_name" and the equal to "Serhii"

db.collection.find({}, {first_name: 1, second_name: 1}) // returns fields "first_name" and "second_name" from all the documents in a collection
```
