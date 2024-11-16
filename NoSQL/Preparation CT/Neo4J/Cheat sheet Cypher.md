---
tags:
  - nosql
---
### **1. Basic Commands**

- **Connect to Neo4j**:
  - Neo4j Browser: Open `http://localhost:7474` in your browser.
  - Default login: `neo4j / neo4j`.


### **2. Creating Nodes and Relationships**

- **Create a Node**:
  ```cypher
  CREATE (n:Label {property1: 'value1', property2: 'value2'})
  ```

- **Create Multiple Nodes**:
  ```cypher
  CREATE (n1:Label1 {property1: 'value1'}), (n2:Label2 {property2: 'value2'})
  ```

- **Create a Relationship Between Nodes**:
  ```cypher
  MATCH (n1:Label1 {property: 'value'}), (n2:Label2 {property: 'value'})
  CREATE (n1)-[:RELATIONSHIP_TYPE]->(n2)
  ```

### **3. Querying Data**

- **Return All Nodes**:
  ```cypher
  MATCH (n) RETURN n
  ```

- **Find Nodes with Specific Label**:
  ```cypher
  MATCH (n:Label) RETURN n
  ```

- **Find Nodes with Specific Properties**:
  ```cypher
  MATCH (n:Label {property: 'value'}) RETURN n
  ```

- **Find Relationships**:
  ```cypher
  MATCH (n1:Label1)-[r:RELATIONSHIP_TYPE]->(n2:Label2) RETURN n1, r, n2
  ```

- **Filter with Conditions**:
  ```cypher
  MATCH (n:Label) WHERE n.property > 'value' RETURN n
  ```

### **4. Updating Data**

- **Add Property to a Node**:
  ```cypher
  MATCH (n:Label {property: 'value'}) SET n.newProperty = 'newValue'
  ```

- **Update Property of a Node**:
  ```cypher
  MATCH (n:Label {property: 'value'}) SET n.property = 'newValue'
  ```

- **Remove Property from a Node**:
  ```cypher
  MATCH (n:Label {property: 'value'}) REMOVE n.property
  ```

- **Set a Label for Specific Nodes**:
  ```cypher
  MATCH (n:Label) SET n:NewLabel
  ```

### **5. Deleting Data**

- **Delete a Node**:
  ```cypher
  MATCH (n:Label {property: 'value'}) DELETE n
  ```

- **Delete a Relationship**:
  ```cypher
  MATCH (n1:Label1)-[r:RELATIONSHIP_TYPE]->(n2:Label2) DELETE r
  ```

- **Delete All Nodes and Relationships**:
  ```cypher
  MATCH (n) DETACH DELETE n
  ```

### **6. Working with Paths**

- **Return All Paths Between Nodes**:
  ```cypher
  MATCH p=((n1:Label1)-[:RELATIONSHIP_TYPE*]-(n2:Label2)) RETURN p
  ```

- **Return Shortest Path Between Two Nodes**:
  ```cypher
  MATCH (n1:Label1 {property: 'value1'}), (n2:Label2 {property: 'value2'})
  MATCH path = shortestPath((n1)-[:RELATIONSHIP_TYPE*]-(n2)) RETURN path
  ```

- **Exclude Specific Nodes in Paths**:
  ```cypher
  MATCH path = shortestPath((n1:Label1)-[:RELATIONSHIP_TYPE*]-(n2:Label2))
  WHERE NOT 'ExcludedNode' IN [n IN nodes(path) | n.property]
  RETURN path
  ```

### **7. Data Importing**

- **Load Data from CSV Files**:
  ```cypher
  LOAD CSV WITH HEADERS FROM 'file:///filename.csv' AS row
  CREATE (n:Label {property: row.column})
  ```

- **Relate Imported Data**:
  ```cypher
  LOAD CSV WITH HEADERS FROM 'file:///filename.csv' AS row
  MERGE (n1:Label1 {property: row.column1})
  MERGE (n2:Label2 {property: row.column2})
  CREATE (n1)-[:RELATIONSHIP_TYPE]->(n2)
  ```

### **8. Exporting Data**

- **Export Data to JSON**:
  ```cypher
  CALL apoc.export.json.query("MATCH (n:Label) RETURN n", "filename.json", {})
  ```
