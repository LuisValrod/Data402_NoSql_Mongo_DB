# MongoDB Exercises

### Commands we did in the lecture:
- show dbs
- use <database_name>
- db.createCollection(“name_of_collection”)
- db.name_of_collection.insertOne({key:value})
- db.name_of_collection.insertMany([ {key": "Value", "key2": "value2"}, {"
new_key": "new_value", "new_key2": "new_value2"}])
- db.name_of_collection.insertOne({key: "value", key2: "value2", key3: {sub-key1: "sub-value1", sub-key2: "sub-value2"}})

**Specific Case**:
```
db.students.insertOne({"name": "Mr S. Global", "year": NumberInt(2024), score: 88.2, address: {city: "Birmingham"}, "course": "Data"})
db.students.updateOne({name:"Mr S. Global"}, {$set:{score: 92.5, newField: true}})
db.students.updateMany({}, {$set: {year: NumberInt(2025)}})
db.students.updateOne({name: "Mr S. Global"}, {$unset: {newField: null}})
db.students.deleteOne({'name': 'Mr S. Global'})
```
<hr>

### Exercise 1
**Create a collection to store information about your favourite films. Add appropriate validation rules, then insert at least 3 documents. Practice using both .insertOne() and .insertMany(). You may want to type commands into a text editor then paste into the shell**

*My solution along with some exploration*:
```shell
db.createCollection("films")
db.createCollection("favourite-films")
db.films.drop()
db['favourite-films'].insertOne({name: "Inception", year: 2010,director: "Christopher Nolan"})
db['favourite-films'].insertMany([{name: "Titanic", year: 1997,director: "James Cameron"},
  {name: "Avatar", year: 2009,director: "James Cameron"}])
```
<br>

### Exercise 2
**Add a new document to the collection, add a new field to that document, remove that field and then remove the document entirely.**

```shell
// Step 1: Add a new document to the collection
db['favourite-films'].insertOne({ 'name': 'Fight Club', 'year': 1999, 'director': 'David Fincher' })

// Step 2: Add a new field to that document
db['favourite-films'].updateOne({ 'name': 'Fight Club' }, { $set: { 'genre': 'Drama' } })

// Step 3: Remove that field from the document
db['favourite-films'].updateOne({ 'name': 'Fight Club' }, { $unset: { 'genre': null } })

// Step 4: Remove the document entirely
db['favourite-films'].deleteOne({ 'name': 'Fight Club' })
```
<br>

### Exercise 3
**Install mongotools. Add the path to it's bin folder to the PATH variable (will be inside MongoDB folder)**<br><br>
*DONE*

### Exercise 4
**Download StarWars.zip. Extract it in a reasonable location. In the terminal (not mongosh) navigate to the folder, make sure it is the one that has all of the json file. Then run the following command to add each to a new db called "starwars"**

```shell
for i in *.json; do
    mongoimport --db starwars --collection characters --jsonArray --file "$i"
done
```
*DONE* <br>

### Exercise 5
**Write a query that finds the Luke Skywalker document**
```shell
use starwars
db.characters.findOne({name: 'Luke Skywalker'})
```
<br>

**Return the value of name and eye_colour only, from the "chewbacca" document**

```shell
db.characters.findOne({name:'Chewbacca'},{_id: 0, name: 1, eye_color: 1})
```

<br>

**Find a way to check the species name of admiral ackbar, this is in an embedded document ("Species")**
```shell
// Check Species to see the structure of this embedded document
db.characters.findOne({name: 'Ackbar'}, {species: 1})

// once seen the structure, execute the following command
db.characters.findOne({name: 'Ackbar'}, {"species.name": 1})
```
