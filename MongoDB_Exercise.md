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
**Create a collection to store information about your favourite films. <br>Add appropriate validation rules, then insert at least 3 documents.<br> Practice using both .insertOne() and .insertMany().<br> You may want to type commands into a text editor then paste into the shell**

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
**Download StarWars.zip. Extract it in a reasonable location. <br>In the terminal (not mongosh) navigate to the folder, make sure it is the one that has all of the json file. Then run the following command to add each to a new db called "starwars"**

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

### Exercise 6
**Write a query that gives us only the names + homeworld names of humans in the database?**

```shell
db.characters.find({'species.name': 'Human'}, {_id: 0, name: 1 , 'homeworld.name': 1})

```

### Exercise 7
**Write a query that gives us all the entries that have an eye_colour of either "yellow" or "orange"**

```shell
db.characters.find({'eye_color': {$in: ['orange', 'yellow']}})

// also for better format

db.characters.find({'eye_color': {$in: ['orange', 'yellow']}}).pretty()

```

### Exercise 8
**You can combine filters using $and or $or**<br>
**Write a query that filter for characters that have both blue eyes and are female <br>
Then write a query that filters for characters that have either blue eyes or are female**

```shell
// first query to filter female entries with blue eyes
db.characters.find({$and: [{ eye_color: "blue" },{ gender: "female" }]}).pretty()

// second query to filter characters that have blue eyes or are female

db.characters.find({$or: [{ eye_color: "blue" },{ gender: "female" }]}).pretty()

```

### Exercise 9
**You can use comparison operators in your queries**<br>
**Write a query that finds characters with a height over 200cm**

```shell
// Query to find characters with height over 200cm
db.characters.find({'height':{$gt: 200}}).pretty()

// Query to format the height to int
db.characters.find().forEach(function(doc) {
  if (typeof doc.height === 'string' && !isNaN(doc.height)) {
    db.characters.updateOne(
      { _id: doc._id },
      { $set: { height: parseInt(doc.height) } }
    );
  }
});

// // //

//Method 1 (simple)
//Unset/set method - First query selects all documents where height = "unknown" and removes the height field from those documents only.
 

db.characters.updateMany(
  {height: "unknown"},
  {$unset: {height: ""}}
)
 
The second query gets all records and if they have a height feild, converts the contents to an int

db.characters.updateMany(
  {},
  [{$set: {height: {$toInt: "$height"}}}]
)

// // //

db.characters.updateMany(
  { height: "unknown" }, // Filter for documents where height is "unknown"
  [
    { 
      $set: { 
        height: null // set the "unknown" to null      } 
    }
  ]
);
 
db.characters.updateMany(
  { height: { $exists: true, $type: "string" } }, // filter for documents where height exists (and is not null) and is a string
  [
    { 
      $set: { 
        height: { 
          $toInt: "$height" 
        } 
      } 
    }
  ]
);
 
db.characters.find({ height: { $gt: 200 } });

```

### Exercise 10
**Experiment with the following operators. What does each do?**
```shell
db.characters.find({ eye_color: { $eq: "blue" } }) // $eq -> return entries with blue eyes

db.characters.find({'height':{$gt: 200}}) // $gt -> return entries with height greater than 200

db.characters.find({'height':{$gte: 200}}) //$gte -> return entries with height greater than or equal to 200

db.characters.find({'eye_color': {$in: ['orange', 'yellow']}}) // $in -> return entries where the field eye_color equals any of the values in the array

db.characters.find({'height':{$lt: 200}}) //$lt -> return entries with height less than 200

db.characters.find({'height': {$lte: 200}}) // $lte -> return entries with height less than or equal to 200

db.characters.find({ eye_color: { $ne: "blue" } }) // $ne -> return entries where value of eye_color is not equal to blue

db.character.find({eye_color: {$nin: ['blue', 'yellow', 'orange', 'brown']}}) // $nin -> return entries where the value of eye_color is not any of the values in the array


```

### Exercises with pymongo:

```python
import pymongo

# Connecting to MongoDB
client = pymongo.MongoClient()
db = client["starwars"]

# Retrieve a document from the database

luke = db.characters.find_one({"name": "Luke Skywalker"})

# print(luke)

# getting only certain fields

luke_short = db.characters.find_one({"name": "Luke Skywalker"},
                                    {"name": 1, "eye_color": 1, "_id": 0})
# print(luke_short)

# Iterating through multiple records/documents

droids = db.characters.find({"species.name": "Droid"}, {"_id": 0, "name": 1})
# for droid in droids:
#     print(droid['name'])

# Exercise 1 - Find the height of Darth Vader, only return results for the name and the height.
darth_vader_height = db.characters.find_one({"name": "Darth Vader"}, {"_id": 0, "name": 1, "height": 1})
# print(darth_vader_height)

# Exercise 2 - Find all characters with yellow eyes, only return results for the names of the characters.
characters_yellow_eyes = db.characters.find({"eye_color": "yellow"}, {"_id": 0, "name": 1})
# for n in characters_yellow_eyes:
#     print(n['name'])

# Exercise 3 - Find male characters. Limit your results to only show the first 3.
males_characters = db.characters.find({'gender': 'male'}, {'_id': 0, 'name': 1}).limit(3)
# for n in males_characters:
#     print(n)

# Exercise 4 -Find the names of all the humans whose homeworld is Alderaan.
characters_from_alderaban = db.characters.find({'homeworld.name': 'Alderaan'}, {'_id': 0, 'name': 1, 'homeworld.name': 1})

for n in characters_from_alderaban:
    print(n)
```

### Mongo advanced
```shell
db.characters.aggregate({
$match: {"species.name": "Human"}
}, {
$group: {
_id: null,
total_height: {$sum: "$height"}}
})
//
db.characters.aggregate([
  {$match: {"species.name": "Human"}},
  {$group: {_id: "$gender", total: {$sum: "$height"}}}
])
//

// // //
// Find the max height pero homeworld
db.characters.aggregate([{
$group: {
_id: "$homeworld.name",
maxHeight: {$max: "$height"
}}}])

//
db.characters.aggregate([
  {$match: {"species.name": "Human"}},
  {$group: {_id: "$gender", total: {$sum: "$height"}}}
])
//

db.characters.count({})
//

db.characters.count({"species.name": "Human"})

//
# Max height
max_height = db.characters.aggregate([
    {"$group":
         {"_id": None, "max_height": {"$max": "$height"}}
    }
]).next()["max_height"]

for tallest in db.characters.find({"height": max_height}):
    print(tallest["name"], tallest["height"])

//
db.characters.distinct('species.name')










