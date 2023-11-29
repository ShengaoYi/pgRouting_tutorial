# Workbook for pgRouting

# Introduction

## Overview of GIS and Routing

Geographic Information Systems (GIS) are powerful tools used to gather, manage, and analyze data. They are crucial in understanding spatial aspects and patterns of various data points. GIS combines cartography, statistical analysis, and database technology.

### Brief Overview of GIS

GIS allows us to visualize, question, analyze, and interpret data to understand relationships, patterns, and trends in spatial context. It is used across various fields such as urban planning, environmental science, transportation, and many more.

**Key Components of GIS:**
- **Maps**: Visual representation of spatial data.
- **Data Layers**: Different types of data (like streets, buildings, water bodies) overlaid on maps.
- **Spatial Analysis Tools**: For analyzing the data in a geographic context.

### Definition and Importance of Routing in GIS

Routing in GIS refers to the process of determining the most efficient path between two or more locations on a map. This is not just about finding the shortest path, but also the most optimal one based on various criteria like distance, time, or even cost.

**Why is Routing Important?**
- **Efficiency**: Determines the best routes for transportation and logistics.
- **Planning**: Helps in urban planning and infrastructure development.
- **Emergency Response**: Crucial for optimizing routes for emergency services like ambulances and fire trucks.
- **Navigation**: Powers modern GPS navigation systems used in everyday life.

### Key Concepts in Routing

- **Network Analysis**: Routing uses network analysis where roads, railways, and paths are represented as a network of interconnected lines.
- **Cost Function**: A crucial part of routing is defining the 'cost' of travel, which could be distance, time, fuel consumption, etc.
- **Algorithm Selection**: Different routing problems require different algorithms, for example, Dijkstra's for shortest path, A* for heuristic-based search, etc.

In the following chapters, we will delve deeper into these concepts, explore various routing algorithms, and learn how to apply them using pgRouting with PostGIS.

### Importance and Applications of Routing in GIS

Routing in Geographic Information Systems (GIS) plays a crucial role in a wide range of applications, impacting various aspects of everyday life as well as numerous industries. Its importance lies in its ability to provide efficient and effective solutions to complex spatial problems.

#### Real-World Applications

1. **Transportation and Logistics**
   - Optimizing delivery routes for e-commerce and postal services.
   - Managing public transportation systems to improve efficiency and connectivity.
   - Route planning for freight and shipping to minimize travel time and fuel consumption.

2. **Urban Planning and Infrastructure**
   - Designing road networks and infrastructure for new developments.
   - Traffic management and congestion analysis in urban areas.
   - Planning public transportation routes and schedules.

3. **Emergency Services**
   - Route optimization for ambulances, fire trucks, and police vehicles to ensure the fastest response times.
   - Disaster response and management, including evacuation planning and resource allocation.

4. **Travel and Navigation**
   - Powering GPS-based navigation systems for personal and commercial use.
   - Planning and optimizing travel itineraries for tourism and exploration.

5. **Environmental Conservation**
   - Wildlife tracking and habitat analysis to create corridors for species movement.
   - Planning and managing protected areas and conservation zones.

6. **Utilities and Services**
   - Managing and maintaining utility networks such as water, gas, and electricity.
   - Route planning for service appointments and maintenance work.

# Chapter 1: Background on pgRouting

## History and Development of pgRouting

### Origins of pgRouting
pgRouting is an extension of the PostGIS spatial database, which itself is an extension of the PostgreSQL database. The development of pgRouting began as a project to extend PostGIS capabilities to include geospatial routing functionalities, allowing for the use of SQL for creating complex routing queries.

### Evolution over Time
- **Early Developments**: Initial versions focused on basic routing algorithms and network analysis.
- **Community Contributions**: Over time, contributions from a global community of developers expanded pgRouting's capabilities.
- **Current State**: Today, pgRouting offers a wide range of advanced routing functionalities, rivaling and even surpassing proprietary routing solutions.

## Overview of pgRouting Capabilities

### Key Features
- **Versatility**: Supports a variety of routing algorithms for different needs.
- **Integration with PostGIS**: Seamless integration with GIS data and spatial functions.
- **Customizability**: Ability to customize and extend for specific use cases.

### Comparison with Other Routing Tools
- **Open-Source vs. Proprietary**: Unlike many proprietary tools, pgRouting is open-source and free to use.
- **Flexibility**: More control and flexibility compared to online routing services like Google Maps or Mapbox.

# Chapter 2: Core Concepts and Algorithms

## Basic Concepts in Network Routing

### Graph Theory Basics
- **Nodes and Edges**: Networks are modeled as graphs with nodes (points) and edges (lines) representing connections.
- **Weights/Costs**: Edges have weights (or costs) representing distance, time, or other metrics.

### Representation of Road Networks in GIS
- **Spatial Data**: Road networks are represented using spatial data types in GIS.
- **Topology**: Importance of network topology for accurate routing.

## Detailed Discussion on Algorithms

### Dijkstra's Algorithm
- **Overview**: Finds the shortest path between nodes in a graph.
- **Application**: Commonly used for simple shortest path calculations.

### A* Algorithm
- **Overview**: An extension of Dijkstra's algorithm that uses heuristics to speed up pathfinding.
- **Application**: Efficient for larger and more complex networks.

### Traveling Salesperson Problem (TSP)
- **Overview**: Seeks the shortest possible route visiting a set of nodes and returning to the start.
- **Application**: Useful in logistics and delivery route optimization.

### Others
- **Floyd-Warshall**: Finds shortest paths between all pairs of nodes.
- **Johnson’s Algorithm**: Efficient for sparse graphs with negative weights but no negative cycles.


# Chapter 3: Setting Up the Environment

## Introduction to PostGIS and PostgreSQL

### Overview of PostGIS
- **What is PostGIS**: An extension of PostgreSQL that adds support for geographic objects, allowing for spatial queries and data analysis.
- **Features**: Includes support for GIS objects, spatial functions, and indexing for efficient querying of spatial data.

### Role of PostgreSQL in GIS
- **Database Management**: PostgreSQL serves as a robust database management system, ideal for handling large and complex spatial datasets.
- **Integration**: Seamless integration with other GIS software and tools, making it a popular choice for GIS applications.

## Installation Requirements

### System Requirements
- **Hardware**: Recommendations for processing power, memory, and storage based on the size of your datasets.
- **Operating System**: Compatibility with various operating systems including Windows, Linux, and macOS.

### Software Dependencies
- **PostgreSQL**: The latest stable version of PostgreSQL.
- **PostGIS**: Ensure compatibility with your PostgreSQL version.

## Installing PostGIS and pgRouting

### Step-by-step Installation Guide
1. **Install PostgreSQL**: Download and install the latest version of PostgreSQL.
2. **Install PostGIS**: Add the PostGIS extension to your PostgreSQL installation.
3. **Enable pgRouting**:
   - Connect to your PostgreSQL database using `psql`.
   - Execute the command to create the pgRouting extension:
     ```sql
     CREATE EXTENSION pgrouting;
     ```
   - Verify the installation with `SELECT pgr_version();`.

# Chapter 4: Getting Started with pgRouting

## Basic pgRouting Queries

### Basic SQL Queries for Routing
- **Query Structure**: Understanding the structure of a routing query in pgRouting.
- **Selecting Nodes and Edges**: How to write queries to select nodes and edges from your network.

### Examples and Use Cases
**Finding the Shortest Path**:
   - Use the `pgr_dijkstra` function to find the shortest path between two points.
   - Example query:
     ```sql
     SELECT * FROM pgr_dijkstra(
       'SELECT id, source, target, cost FROM your_network_table',
       start_id, end_id, directed := true
     );
     ```

### Understanding Graphs and Networks in pgRouting

pgRouting extends the capabilities of PostGIS and PostgreSQL by providing geospatial routing functionalities. A fundamental concept to grasp in pgRouting is how it represents and works with graphs and networks.

#### How pgRouting Represents Networks

- **Graph Theory Basics**: In pgRouting, networks are modeled as graphs. A graph consists of nodes (or vertices) and edges (or links) that connect these nodes.
- **Nodes**: These are typically intersections, points of interest, or termini in a road network.
- **Edges**: Represent the road segments or paths that connect nodes. Each edge has attributes such as length, cost (which could be distance, time, or other metrics), and directionality (one-way or two-way).

#### Building and Querying Network Data

1. **Creating a Topological Network**:
   - Example SQL:
     ```sql
     -- Transform the 'geom' column from EPSG:4326 (WGS 84) to EPSG:2263 (NAD83/New York Long Island) projection
     ALTER TABLE nyc_road_direction_speed
     ALTER COLUMN geom TYPE geometry(MultiLineString, 2263)
       USING ST_Transform(geom, 2263);

     -- Add columns that are typically used for graph representation in routing algorithms
      ALTER TABLE nyc_road_direction_speed
      ADD COLUMN "source" INTEGER,
      ADD COLUMN "target" INTEGER,
      ADD COLUMN cost DOUBLE PRECISION,
      ADD COLUMN reverse_cost DOUBLE PRECISION;
      
      -- Update values in the table
      -- Modify existing rows in the nyc_road_direction_speed table based on the values in the trafdir column.
      -- Set the cost column and reverse_cost column according to different conditions
      UPDATE nyc_road_direction_speed
      SET cost = CASE
              WHEN trafdir = 'FT' THEN shape_leng::double precision  -- Forward direction is passable, set cost as the shape_leng
              WHEN trafdir = 'TF' THEN -1 -- Forward direction is not passable, set a high cost
              WHEN trafdir = 'TW' THEN shape_leng::double precision -- Both directions are passable, set cost as the shape_leng
              WHEN trafdir = 'NV' THEN -1 -- Both directions are impassable, set a high cost
              ELSE -1 -- For any other value, set as impassable
          END,
          reverse_cost = CASE
              WHEN trafdir = 'FT' THEN -1 -- Reverse direction is not passable, set a high cost
              WHEN trafdir = 'TF' THEN shape_leng::double precision -- Reverse direction is passable, set cost as the shape_leng
              WHEN trafdir = 'TW' THEN shape_leng::double precision -- Both directions are passable, set cost as the shape_leng
              WHEN trafdir = 'NV' THEN -1 -- Both directions are impassable, set a high cost
              ELSE -1 -- For any other value, set as impassable
          END;
      -- The pgr_createTopology function is a part of the pgRouting extension for PostgreSQL,
      -- used to create a network topology from a road network table. 
      -- The function takes several parameters, including the name of the table 
      -- containing the road network data, tolerance value, geometry column name, and primary key column name.
         
      -- 'nyc_road_direction_speed': This is the name of the table containing the road network data.
         
      -- 0.00001: This is the tolerance value. It determines the distance within which two network nodes 
      -- will be considered equal. Nodes that are closer than this tolerance value will be snapped together 
      -- to create a network topology.
         
      -- 'geom': This is the name of the geometry column in the nyc_road_direction_speed table. 
      -- It holds the geometric information (such as LINESTRING) representing the road segments.
         
      -- 'gid': This is the primary key column in the nyc_road_direction_speed table. 
      -- It uniquely identifies each row in the table.
         
      -- When you run this query, the pgr_createTopology function will process the road network data,
      -- snap nearby nodes together based on the specified tolerance, and create a network topology suitable for 
      -- routing and analysis using pgRouting functions.
      SELECT pgr_createTopology(
         'nyc_road_direction_speed', 
         0.00001,
         'geom',
         'gid',
         'source',
         'target'
      );
     ```

2. **Preparing Data for pgRouting**:
   - Once you have your network data in the database, you need to assign source and target nodes to each edge and calculate the cost for each edge.
   ```sql
   -- The results are used to update the pickup_node and dropoff_node columns, respectively, in the taxiorder table.
   UPDATE taxiorder
   SET pickup_node = ( -- Assign a value to the pickup_node column in the taxiorder table.
     SELECT node.id 
     FROM nyc_road_direction_speed_vertices_pgr AS node -- A subquery that retrieves the id column from the nyc_road_direction_speed_vertices_pgr table aliased as node.
     ORDER BY node.the_geom <-> ST_SetSRID(ST_MakePoint(pickup_longitude::double precision, pickup_latitude::double precision), 4326) -- Calculate the distance between the pickup location (specified by pickup_longitude and pickup_latitude) and the geometries (the_geom) in the nyc_road_direction_speed_vertices_pgr table. It orders the results by proximity, with the closest node first.
     LIMIT 1 -- Limits the result to the nearest node, ensuring that only one node is selected.
   );
   
   UPDATE taxiorder
   SET dropoff_node = (
     SELECT node.id
     FROM nyc_road_direction_speed_vertices_pgr AS node
     ORDER BY node.the_geom <-> ST_SetSRID(ST_MakePoint(dropoff_longitude::double precision, dropoff_latitude::double precision), 4326)
     LIMIT 1
   );
     ```

Understanding how to model your data as a network in pgRouting is key to performing efficient routing queries. Each edge and node in your network can have multiple attributes that can be used in your routing algorithms, allowing for complex and real-world routing scenarios.

## Chapter 5: Advanced Topics in pgRouting

### Routing for Multiple Pairs
pgRouting offers capabilities to handle routing for multiple pairs of origins and destinations efficiently. This feature is particularly useful in scenarios like fleet management, where you need to determine optimal routes for several vehicles simultaneously.

```sql

ALTER TABLE nyc_road_direction_speed
ADD COLUMN x1 DOUBLE PRECISION,
ADD COLUMN y1 DOUBLE PRECISION,
ADD COLUMN x2 DOUBLE PRECISION,
ADD COLUMN y2 DOUBLE PRECISION;

-- Update the newly added columns (x1, y1, x2, y2) using the spatial functions ST_X and ST_Y. 
-- It updates the nyc_road_direction_speed table by matching the source and target IDs from the table 
-- with the corresponding IDs in the nyc_road_direction_speed_vertices_pgr table 
-- and extracting the X and Y coordinates for start and end points of road segments.
UPDATE nyc_road_direction_speed n
SET x1 = ST_X(sn.the_geom),
    y1 = ST_Y(sn.the_geom),
    x2 = ST_X(en.the_geom),
    y2 = ST_Y(en.the_geom)
FROM nyc_road_direction_speed_vertices_pgr sn, nyc_road_direction_speed_vertices_pgr en
WHERE n.source = sn.id
AND n.target = en.id; --Specify that the update should be performed only where the source column in the nyc_road_direction_speed table matches the id column in the sn table, and the target column in the nyc_road_direction_speed table matches the id column in the en table. This ensures that the correct spatial data is associated with the corresponding road segments in the nyc_road_direction_speed table.

-- Prepare for the order data
-- Alter the taxiorder table in the database by adding two new columns: pickup_node and dropoff_node, 
-- both of type INTEGER. 
ALTER TABLE taxiorder
ADD COLUMN pickup_node INTEGER,
ADD COLUMN dropoff_node INTEGER;


-- A function call to pgr_astar, a pgRouting function used for A* shortest path calculations.
SELECT
  o.id,
  o.pickup_node,
  o.dropoff_node,
  pgr_astar(
    'SELECT gid as id, source, target, cost, x1, y1, x2, y2 FROM nyc_road_direction_speed',
    o.pickup_node,
    o.dropoff_node,
    directed := false -- This parameter indicates that the graph is undirected, meaning that paths can be traversed in both directions
  ) as route
FROM
  taxiorder o;
```

### One-Way vs. Two-Way Streets in Routing
Routing through a network of streets requires understanding the directionality of each segment.
```sql
-- The pgr_dijkstra function is a part of the pgRouting extension for PostgreSQL and is used for 
-- finding the shortest path in a graph using Dijkstra's algorithm.

-- The first parameter of pgr_dijkstra is a subquery that selects the necessary columns from your road network table.
-- Here, gid is used as the ID, source and target represent the start and end nodes of each edge, 
-- cost represents the cost of moving from the source to the target node, 
-- and reverse_cost represents the cost of moving in the opposite direction.

-- The second parameter (335) represents the source node ID from which you want to find the shortest path.

-- The third parameter (796) represents the target node ID where you want the shortest path to end.

-- The directed := true argument indicates that the graph is directed. 
-- Directed graphs have edges with direction, meaning you can only travel in one direction along each edge.

-- This query will find the shortest path in the graph from node 335 to node 796 using Dijkstra's algorithm,
-- considering the provided edge costs and directions. 

-- The result will include the sequence of nodes and edges that form the shortest path, 
-- along with the total cost of the path.

SELECT
  seq,
  path_seq,
  node,
  edge,
  dijkstra.cost,
  agg_cost,
  geom
FROM pgr_Dijkstra(
  'select gid as id, source, target, cost, reverse_cost from nyc_road_direction_speed',
  335, 796, true) as dijkstra
JOIN nyc_road_direction_speed as edges
ON dijkstra.edge = edges.gid;

```

### Considering Speed Limits in Routing
Incorporating speed limits allows for routing based not just on distance, but on estimated travel time.
```sql

-- Consider speed limit
-- Add new columns for time-based cost
ALTER TABLE nyc_road_direction_speed 
ADD COLUMN time_cost DOUBLE precision,
ADD COLUMN time_reverse_cost DOUBLE PRECISION;

-- Change the speed limit 0 to 25. (In this data, 25 is the majority)
UPDATE nyc_road_direction_speed
SET postvz_sl = '25'
WHERE postvz_sl = '0';

-- Update the new columns with calculated time-based cost
-- The time_cost and time_reverse_cost columns are populated with time-based costs calculated using the formula: 
-- (length in meters) / (speed limit in miles per hour * 1609.34 meters per mile / 3600 seconds per hour). 
-- If the cost or reverse cost is greater than 0 (indicating a valid road segment), the time-based cost is 
-- calculated; otherwise, the column is set to -1.
UPDATE nyc_road_direction_speed
SET time_cost = CASE
                  WHEN cost > 0 THEN shape_leng::double precision / (postvz_sl::double precision * 1609.34 / 3600)
                  ELSE -1
                END,
    time_reverse_cost = CASE
                          WHEN reverse_cost > 0 THEN shape_leng::double precision / (postvz_sl::double precision * 1609.34 / 3600)
                          ELSE -1
                        END;
-- Use the pgr_Dijkstra function to find the shortest paths in the nyc_road_direction_speed road network. 
-- The function is provided with a SQL subquery that selects the necessary columns, including 
-- the calculated time_cost and time_reverse_cost. 
-- The function then calculates the shortest path based on these time-based costs. 
-- The first query considers the graph as directed (true), while the second query considers it as undirected (false).
 SELECT * FROM pgr_Dijkstra(
  'select gid as id, source, target, time_cost as cost, time_reverse_cost as reverse_cost from nyc_road_direction_speed',
  15916, 3253, true);

-- QGIS
-- Defines a Common Table Expression (CTE) named shortest_path.
-- CTE stands for Common Table Expression and is defined using the WITH keyword. 
-- It is a named temporary result set in a SQL query that can reference within the context of a SELECT, INSERT, 
-- UPDATE, or DELETE statement. 

WITH shortest_path AS (
    SELECT * FROM pgr_Dijkstra(
        'SELECT gid AS id, source, target, cost, reverse_cost FROM nyc_road_direction_speed',
        15916, 3253, true
    )
)
SELECT * 
FROM shortest_path
JOIN nyc_road_direction_speed AS edges
ON shortest_path.edge = edges.gid;
```

### Solving TSP in pgRouting
The Traveling Salesperson Problem (TSP) is a classic problem in routing and logistics.
```sql
-- Traveling Salesperson Problem (TSP) https://docs.pgrouting.org/latest/en/pgr_TSP.html
SELECT * FROM pgr_TSP(
  $$SELECT * FROM pgr_dijkstraCostMatrix(
    'SELECT gid as id, source, target, cost, reverse_cost FROM nyc_road_direction_speed',
    ARRAY[15916, 3253, 31665, 626],  -- List of node IDs to visit
    directed => false) $$); --This inner query calculates the cost matrix for the specified nodes using Dijkstra's algorithm. It uses the pgr_dijkstraCostMatrix function to find the shortest paths between the specified nodes in the nyc_road_direction_speed road network. The SELECT statement creates a cost matrix based on the road network, considering both the forward and reverse costs. The ARRAY specifies the list of node IDs to visit, and directed => false indicates that the graph is undirected.
    -- This outer query solves the Traveling Salesman Problem using the TSP solver provided by pgRouting. It takes the output of the inner query (the cost matrix) as input. The pgr_TSP function finds the optimal order in which to visit the specified nodes (specified in the ARRAY) while minimizing the total travel cost based on the calculated cost matrix.

-- QGIS
WITH tsp_result AS (
  SELECT * FROM pgr_TSP(
    $$SELECT * FROM pgr_dijkstraCostMatrix(
      'SELECT gid as id, source, target, cost, reverse_cost FROM nyc_road_direction_speed',
      ARRAY[15916, 3253, 31665, 626],  -- List of node IDs to visit
      directed => false) $$
  )
), tsp_sequence AS (
  SELECT seq, node, lead(node) OVER (ORDER BY seq) AS next_node
  FROM tsp_result
  WHERE seq < (SELECT max(seq) FROM tsp_result)  -- Exclude the last node
),dijkstra_paths AS (
  SELECT 
    tsp.seq AS tsp_seq,
    dp.*
  FROM tsp_sequence tsp,
  LATERAL (
    SELECT d.* FROM pgr_dijkstra(
      'SELECT gid AS id, source, target, cost FROM nyc_road_direction_speed',
      tsp.node,
      tsp.next_node,
      directed := false
    ) AS d WHERE d.edge > -1
  ) AS dp
)SELECT * FROM dijkstra_paths
JOIN nyc_road_direction_speed as edges
ON dijkstra_paths.edge = edges.gid;

```

# Appendix A: Installation Instructions

## Installing PostGIS and pgRouting

### Detailed Installation Guide

1. **Install PostgreSQL**:
   Ensure PostgreSQL is installed. Download it from the [PostgreSQL Official Site](https://www.postgresql.org/download/).

2. **Install PostGIS**:
   PostGIS is available through most package managers. On Ubuntu, for instance, use:
   ```bash
   sudo apt-get install postgis
   
3. **Install pgRouting**:
Once PostGIS is installed, pgRouting can be added as an extension. Execute the following SQL commands in your PostgreSQL database:
```sql
CREATE EXTENSION postgis;
CREATE EXTENSION pgrouting;
```
### Verifying the Installation
To verify the installation, execute the following command in the PostgreSQL console:
```sql
SELECT pgr_version();
```
This command should return the installed version of pgRouting.

## Appendix B: Frequently Asked Questions
### FAQs on pgRouting
- Question: How does pgRouting handle one-way streets in routing?

  Answer: pgRouting considers one-way streets by using the reverse_cost parameter in its functions. If reverse_cost is set to a high value or -1, it indicates that the road segment cannot be traversed in the opposite direction.
- Question: What are the system requirements for pgRouting?

  Answer: pgRouting requires PostgreSQL and PostGIS to be installed. The system requirements are generally those required by these two components. A more powerful CPU and more memory can improve performance, especially for complex queries on large datasets.

## References
[Topological Network](https://docs.pgrouting.org/latest/en/pgr_createTopology.html)

[Multiple Pairs](https://docs.pgrouting.org/latest/en/pgr_aStar.html)

[Directional Routing Differences](https://docs.pgrouting.org/latest/en/pgr_dijkstra.html)

[Traveling Salesperson Problem](https://docs.pgrouting.org/latest/en/pgr_TSP.html)


