2026-03-23 12:55

# 2 - Data Models and Query Languages

## Data Model

- Data model define how the software is written and how we think about the problem that we are solving
- Most applications are built by layering data model one on top of another

## Relational Model vs Document Model

### Relational Model
- Data organized into relations (table) and each relation is a collection of tuples (row)
- RDBMS and SQL were its implementation

### Document Model
- NoSQL - _not only SQL_
- Driving forces
  - Greater scalability
  - Specialised query operations
  - Dynamic and expressive data model

### Object-Relational Mismatch
- Object-oriented programming required an awkward translation layer for SQL data model
- ORM framework can minimise boilerplate code but not completely solve
- For example, 1-to-many relation required a messy join

### Many-to-one and Many-to-Many
- Some 1 to many values like a user can be in different industries should be standardize and make into M:M (e.g. user select industry from dropdown)
- This is known as _normalization_
- We then store them using ID and link to the text rather than as a free text for every user
- Provide several benefits
  - Enable search
  - Consistent style and spelling
  - Avoid ambiguity
  - Ease of update
  - Better locality support
- Normalization usually not supported in Document Model because they have weak join support
- Developer have to decide whether denormalize (duplicate the data) or perform manual join in application code using (document reference - similar to foreign key)

### Network Model (CODASYL model)
- Relational data model was made to solve normalization problem of hierarchical data model
- Another competing data model is the network model, instead of foreign key it utilise pointers-like key
- To access a record we need to follow an access path
- Make most efficient use of hardware but make application code highly complex

### Schema Flexibility in Document Model
- Document model is schema-on-read, means schema is not explicitly enforced by the database
- Fields can be added just by changing application code
- In contract, schema change in SQL require migration and typically downtime
- Hence, document model is useful for heterogenous data which does not have fixed schema

### Data Locality
- If application often access the entire document, document model provide storage locality
- In contract, SQL require multiple index lookups
- However, this advantage allow shows if we usually need large part of the document. There is no partial load, we need to load the whole document to read or edit it
- Therefore, general recommendation is to keep the document small
- Storage locality not only applies to Document Model, e.g. like Column-Table, Cluster Table

### Convergence
- There are SQL database like Postgres that support JSON document
- There are document database like Mongo that perform automatic join

## Query Languages for Data

### Declarative Language
- SQL introduce declarative query language instead of imperative language like IMS and CODASYL
- Imperative tells computer how to do
- Declarative tells computer what you want
- Declarative more attractive
  - Simple to work with
  - Abstract away implementation, allow database engine to perform optimization on its own without code rewrite
- This advantage apply for other language like CSS

### MapReduce Querying
- Programming model for processing large amounts of data in bulk across many machines
- A limited form of MapReduce is supported by some NoSQL datastore
- Somewhere between imperative and declarative
- Based on map (collect) and reduce (fold/inject)

```js
db.observations.mapReduce(
  function map() {
    var year = this.observationTimestamp.getFullYear();
    var month = this.observationTimestamp.getMonth() + 1;

    emit(year + "-" + month, this.numAnimals);
  },

  function reduce(key, values) {
    return Array.sum(values);
  },

  {
    query: { family: "Sharks" },
    out: "monthlySharkReport",
  },
);
```
- The `map` and `reduce` functions must be pure functions (no additional db queries or side effect), this allow them to run on any order and any machine

## Graph Model
- Relational model can handle normal M:M but not when they become very complex
- Graph consists of vertices (nodes/entities) and edges (relationship/arcs)
- Example of data that can be modeled to graph
  - Social graph
  - Web graph
  - Road/rail networks
- Well-known algorithms can operates on graph, car navigation system, PageRank, etc.

### Property Graph
Each vertex have
- A unique identifier
- A set of outgoing and incoming edge
- A collection of properties
  Each edge consists of
- A unique identifier
- Start and end vertex
- Label
- A collection of properties
  Important aspects of the model
- Any vertex can have any edge connect to any vertex, without schema restriction
- Given any vertex, we can efficiently find its incoming and outgoing edge and traverse the graph
- Different labels allow storage different relationship

### Cypher Query Language
- Declarative query language for property graphs created for Neo4j graph database

```SQL
    CREATE (NAmerica:Location {name:'North America', type:'continent'}), (
        USA:Location {name:'United States', type:'country' }
    ), (
        Idaho:Location {name:'Idaho', type:'state' (
        Lucy:Person {name:'Lucy' }
        ),
        (
            Idaho
        ) -[:WITHIN]-> (
            USA
        ) -[:WITHIN]-> (
            NAmerica
        ),
        (
            Lucy
        ) -[:BORN_IN]-> (
            Idaho
        )
```

Query

```SQL

    MATCH (person) -[:BORN_IN]-> (
    ) -[:WITHIN*0..]-> (
        us:Location {name:'United States'}
    ), (
        person
    ) -[:LIVES_IN]-> (
    ) -[:WITHIN*0..]-> (
        eu:Location {name:'Europe'}
    ) RETURN person.name
```

The query can be read as follows:
Find any vertex (call it person) that meets both of the following conditions:

1. person has an outgoing BORN_IN edge to some vertex. From that vertex, you can follow a chain of outgoing WITHIN edges until eventually you reach a vertex of type Location, whose name property is equal to "United States".
2. That same person vertex also has an outgoing LIVES_IN edge. Following that edge, and then a chain of outgoing WITHIN edges, you eventually reach a vertex of type Location, whose name property is equal to "Europe".
   In Cypher, `:WITHIN*0..` expresses that fact very concisely: it means “follow a WITHIN edge, zero or more times.” This is hard to replicate in SQL.

```SQL
-- set of vertex IDs of all locations within the United States
    in_usa(vertex_id) AS (
    SELECT
        vertex_id
    FROM
        vertices
    WHERE
        properties->>'name' = 'United States'
    UNION
    SELECT
        edges.tail_vertex
    FROM
        edges
    JOIN
        in_usa
            ON edges.head_vertex = in_usa.vertex_id
    WHERE
        edges.label = 'within' ),
```

### Triple-Stores and SPARQL
- Mostly equivalent to the property graph model
- All information is stored in the form of three-part statements `(subject, predicate, object)`, e.g. `(Jim, likes, bananas)`
- Subject = vertext
- Object is one of 2 things
  - A value in primitive data types
  - Another vertex in the graph (in that case, predicate is an edge)
- Can be expressed using Turtle language

![[Attachments/Pasted image 20260405192444.png]]

#### Semantic Web
- The triple-store data model is independent of the semantic web
- But they often discuss together
- Based on the idea: making websites publish info that is machine-readable

#### RDF Data Model
- Resource Description Framework
- Intended as mechanism for different websites to publish data in consistent format
- Written in Turtle language or XML

#### SPARQL query language
- Query language for triple-stores using RDF model
```
(person) -[:BORN_IN]-> () -[:WITHIN*0..]-> (location) # Cypher 

?person :bornIn / :within* ?location. # SPARQL
```

### Graph Database vs Network Model (CODASYL [[#Network Model (CODASYL model)]])
- Network model has schema but graph db no
- CODASYL require explicit travel path
- CODASYL used ordered set for children of a record
- CODASYL use imperative query compared to Cypher or SPARQL


### Datalog
- Much older language than SPARQL or Cypher
- Similar with triple
```
name(namerica, 'North America'). 
type(namerica, continent).
```
- A subset of Prolog, required definition of predicates
```
within_recursive(Location, Name) :- name(Location, Name). /* Rule 1 */

within_recursive(Location, Name) :- within(Location, Via),
within_recursive(Via, Name)
```


## Summary
- Hierarchical model -> Network (obsolete) & Relational Model (SQL, schema-on-write) -> Document & Graph Model (NoSQL, schema-on read)

# References
