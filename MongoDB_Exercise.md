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
```
db.createCollection("films")
db.createCollection("favourite-films")
db.films.drop()
db['favourite-films'].insertOne({name: "Inception", year: 2010,director: "Christopher Nolan"})
db['favourite-films'].insertMany([{name: "Titanic", year: 1997,director: "James Cameron"},
  {name: "Avatar", year: 2009,director: "James Cameron"}])
```

