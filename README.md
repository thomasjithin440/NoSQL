# README: Movie Recommendation System Using NoSQL Databases

## **Project Overview**
This project demonstrates the integration of three NoSQL databases (**MongoDB**, **Neo4j**, and **Redis**) to build a scalable and efficient **Movie Recommendation System**. Each database is utilized for its strengths:

1. **MongoDB (Document Database)**: Stores and queries hierarchical data such as movies and user profiles.
2. **Neo4j (Graph Database)**: Manages relationships between users and movies to enable recommendation generation.
3. **Redis (Key-Value Database)**: Caches frequently accessed data like recommendations and user watchlists for faster retrieval.

---

## **Features**
- **MongoDB**:
  - Stores movie details and user profiles.
  - Provides functionalities for adding, updating, deleting, and querying data.
- **Neo4j**:
  - Represents user-movie interactions as graph nodes and edges.
  - Generates recommendations based on shared preferences among users.
- **Redis**:
  - Caches recommendations and watchlists for quick access.
  - Tracks trending movies using counters.

---

## **Technologies Used**
- **Programming Language**: Python
- **Databases**:
  - MongoDB (via Atlas cluster)
  - Neo4j (via `neo4j` Python driver)
  - Redis (via Redis Cloud)
- **Libraries**:
  - `pymongo` for MongoDB
  - `neo4j` for Neo4j
  - `redis` for Redis

---

## **Setup Instructions**

### **1. Prerequisites**
- Python 3.8 or higher
- Install the required libraries:
  ```bash
  pip install pymongo neo4j redis
  ```

### **2. MongoDB Setup**
- Use a MongoDB Atlas cluster.
- Create a database named `movie_system` with the following collections:
  - **`movies`**: Stores movie details.
  - **`users`**: Stores user profiles and ratings.
- Example configuration in code:
  ```python
  uri = "mongodb+srv://<username>:<password>@cluster0.mongodb.net/"
  client = MongoClient(uri)
  db = client["movie_system"]
  ```

### **3. Redis Setup**
- Configure a Redis Cloud instance.
- Use the following details in your code:
  ```python
  r = redis.Redis(
      host="<your-redis-endpoint>",
      port=<your-redis-port>,
      password="<your-redis-password>",
      decode_responses=True
  )
  ```

### **4. Neo4j Setup**
- Install Neo4j locally or use a hosted instance.
- Use the following connection setup:
  ```python
  from neo4j import GraphDatabase

  uri = "bolt://localhost:7687"
  driver = GraphDatabase.driver(uri, auth=("neo4j", "<password>"))
  ```

---

## **How to Run**
1. Ensure MongoDB, Neo4j, and Redis are running.
2. Populate MongoDB with sample data using the provided scripts.
3. Use Neo4j scripts to define nodes and relationships.
4. Execute Redis caching functions for testing recommendations.
5. Run the main Python workflow to:
   - Query MongoDB for user and movie data.
   - Use Neo4j to generate recommendations.
   - Cache results in Redis for faster future retrievals.

---

## **Example Queries and Workflows**

### **MongoDB**
- Retrieve all movies:
  ```python
  def get_movies_from_mongodb():
      return list(movies_collection.find({}, {"_id": 0}))
  ```

### **Neo4j**
- Fetch recommendations for a user:
  ```python
  MATCH (u:User {user_id: 1})-[:LIKED]->(m:Movie)<-[:LIKED]-(other:User)-[:LIKED]->(rec:Movie)
  WHERE NOT (u)-[:LIKED]->(rec)
  RETURN DISTINCT rec.movie_id AS RecommendedMovie
  ```

### **Redis**
- Cache and retrieve recommendations:
  ```python
  def cache_recommendations(user_id, recommendations):
      key = f"recommendations:{user_id}"
      r.set(key, ",".join(map(str, recommendations)))

  def get_cached_recommendations(user_id):
      key = f"recommendations:{user_id}"
      cached_data = r.get(key)
      return list(map(int, cached_data.split(","))) if cached_data else []
  ```

---

## **Workflow**
1. **Data Storage**:
   - Store movie and user data in MongoDB.
2. **Recommendation Generation**:
   - Use Neo4j to process user-movie relationships and generate recommendations.
3. **Caching**:
   - Cache recommendations and frequently accessed data in Redis.

---

## **Team Members**
- [Add your names here]

---

## **GitHub Repository**
- [Provide the link here]

---

## **Additional Notes**
- Ensure data consistency across all databases.
- Extend the project by integrating a front-end application for user interaction.
- Use the provided sample scripts and queries to test the workflow end-to-end.



