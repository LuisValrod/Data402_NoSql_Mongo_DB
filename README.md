# NoSQL and MongoDB

This README.md file provides a detailed overview of NoSQL databases and MongoDB, covering essential concepts, comparisons, and use cases.<hr>

## NoSQL

### What is it?
NoSQL, or "Not Only SQL," is a category of database management systems that do not adhere to the traditional relational database management system (RDBMS) principles.<br> 
NoSQL databases are designed to handle large volumes of data, scale out efficiently, and provide flexible schema design.

### Compare SQL to NoSQL
- **Data Model**:
  - SQL: Relational databases with tables, rows, and columns.
  - NoSQL: Various models including document, key-value, wide-column, and graph.
- **Schema**:
  - SQL: Fixed schema with predefined tables and columns.
  - NoSQL: Dynamic schema allowing flexibility in data structure.
- **Query Language**:
  - SQL: Structured Query Language (SQL).
  - NoSQL: Varies by database type (e.g., MongoDB uses JSON-like queries).
- **Transactions**:
  - SQL: Strong ACID (Atomicity, Consistency, Isolation, Durability) properties.
  - NoSQL: Varies; some support BASE (Basically Available, Soft state, Eventual consistency).
- **Scalability**:
  - SQL: Vertical scaling (adding more power to the existing machine).
  - NoSQL: Horizontal scaling (adding more machines to the database system).
 
### What language(s) can be used?
NoSQL databases can be interfaced with many programming languages, including JavaScript (Node.js), Python, Java, C#, Ruby, PHP, and others, depending on the specific database.

### Example NoSQL Schema design
A NoSQL schema in MongoDB:
```json
{
  "user_id": "12345",
  "name": "John Doe",
  "email": "johndoe@example.com",
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "zip": "12345"
  },
  "orders": [
    {
      "order_id": "111",
      "product": "Widget",
      "quantity": 2
    },
    {
      "order_id": "112",
      "product": "Gadget",
      "quantity": 1
    }
  ]
}
```
### The Concept of Scalability. Benefits and downsides
**Scalability:** NoSQL databases are designed to scale out by distributing data across multiple servers. This is known as horizontal scaling.<br>
- **Benefits:**
  - Handles large volumes of data and high traffic efficiently.
  - Flexible data models adapt to changing requirements.
  - High availability and fault tolerance.
<br>
- **Downsides:**
  - Consistency issues due to eventual consistency models.
  - Complex management and maintenance compared to SQL databases.
  - Limited support for complex queries and transactions.
 
### Types of NoSQL Databases
- Document Stores: e.g., MongoDB, CouchDB
- Key-Value Stores: e.g., Redis, Riak
- Wide-Column Stores: e.g., Cassandra, HBase
- Graph Databases: e.g., Neo4j, ArangoDB

<hr>

## MongoDB

### What is MongoDB?
MongoDB is a NoSQL document-oriented database that stores data in JSON-like format, which allows for flexible and dynamic schemas.

### What are collections in MongoDB?
Collections in MongoDB are equivalent to tables in relational databases. They store a group of documents.

### What are Documents?
Documents in MongoDB are records in a collection, similar to rows in a table. They are JSON-like objects.

### MongoDB Architecture, how does it work?
MongoDB architecture includes:

- Database: Contains collections of documents.
- Collection: Contains documents.
- Document: JSON-like data structure.
- Replica Sets: Groups of MongoDB instances that maintain the same data set, providing redundancy and high availability.
- Sharding: Distributes data across multiple servers to support horizontal scaling.

### What are replica sets?
Replica sets are a group of MongoDB servers that maintain the same data. They ensure redundancy and increase data availability by replicating data across multiple nodes.

### What is sharding?
Sharding is a method for distributing data across multiple machines. MongoDB uses sharding to support deployments with very large data sets and high throughput operations.

### Advantages of MongoDB?
- Flexible schema design.
- High scalability with sharding.
- High availability with replica sets.
- Good performance for read and write operations.

