# pgRouting Tutorial

Welcome to the pgRouting Tutorial repository. This tutorial provides practical guidance on implementing network routing with pgRouting, an extension of PostGIS and PostgreSQL. Within, you will find SQL scripts for various tasks in network analysis.

## Overview

The tutorial covers several operations:

1. **Creating a Topological Network:** Initializes the network topology for routing.
2. **Preparing Order Data:** Associates order locations with the closest network nodes.
3. **Routing for Multiple Pairs:** Determines optimal paths for multiple pairs using `pgr_aStar`.
4. **Directional Routing Differences:** Demonstrates one-way vs. two-way street routing with `pgr_dijkstra`.
5. **Speed Limit Considerations:** Adjusts routes to consider varying speed limits.
6. **Traveling Salesperson Problem (TSP):** Solves the TSP for a set of locations.

## Prerequisites

Ensure you have the following before starting:

- PostgreSQL with PostGIS and pgRouting extensions installed.
- A spatially enabled database with `nyc_road_direction_speed` and `order` tables present.

## Usage

Follow these steps to use the tutorial:

### 1. Creating a [Topological Network](https://docs.pgrouting.org/latest/en/pgr_createTopology.html)

```sql

SET search_path to public;

-- Check pgr version
select pgr_version();

ALTER TABLE nyc_road_direction_speed
ADD COLUMN "source" INTEGER,
ADD COLUMN "target" INTEGER,
ADD COLUMN cost DOUBLE PRECISION,
ADD COLUMN reverse_cost DOUBLE PRECISION;

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

SELECT pgr_createTopology(
	'nyc_road_direction_speed', 
	0.00001,
	'geom',
	'gid',
	'source',
	'target'
);

```
### 2. Preparing Order Data

```sql

UPDATE "order"
SET pickup_node = (
  SELECT node.id
  FROM nyc_road_direction_speed_vertices_pgr AS node
  ORDER BY node.the_geom <-> ST_SetSRID(ST_MakePoint(pickup_longitude::double precision, pickup_latitude::double precision), 4326)
  LIMIT 1
);

UPDATE "order"
SET dropoff_node = (
  SELECT node.id
  FROM nyc_road_direction_speed_vertices_pgr AS node
  ORDER BY node.the_geom <-> ST_SetSRID(ST_MakePoint(dropoff_longitude::double precision, dropoff_latitude::double precision), 4326)
  LIMIT 1
);

```

### 3. Routing for [Multiple Pairs](https://docs.pgrouting.org/latest/en/pgr_aStar.html)
```sql

ALTER TABLE nyc_road_direction_speed
ADD COLUMN x1 DOUBLE PRECISION,
ADD COLUMN y1 DOUBLE PRECISION,
ADD COLUMN x2 DOUBLE PRECISION,
ADD COLUMN y2 DOUBLE PRECISION;

UPDATE nyc_road_direction_speed n
SET x1 = ST_X(sn.the_geom),
    y1 = ST_Y(sn.the_geom),
    x2 = ST_X(en.the_geom),
    y2 = ST_Y(en.the_geom)
FROM nyc_road_direction_speed_vertices_pgr sn, nyc_road_direction_speed_vertices_pgr en
WHERE n.source = sn.id
AND n.target = en.id;

SELECT
  o.id,
  o.pickup_node,
  o.dropoff_node,
  pgr_astar(
    'SELECT gid as id, source, target, cost, x1, y1, x2, y2 FROM nyc_road_direction_speed',
    o.pickup_node,
    o.dropoff_node,
    directed := false
  ) as route
FROM
  "order" o;

```

### 4. [Directional Routing Differences](https://docs.pgrouting.org/latest/en/pgr_dijkstra.html)
```sql
 SELECT * FROM pgr_Dijkstra(
  'select gid as id, source, target, cost, reverse_cost from nyc_road_direction_speed',
  15916, 3253, true);
  
  SELECT * FROM pgr_Dijkstra(
  'select gid as id, source, target, cost, reverse_cost from nyc_road_direction_speed',
  15916, 3253, false);
```

### 5. Speed Limit Considerations
```sql
 -- Add new columns for time-based cost
ALTER TABLE nyc_road_direction_speed 
ADD COLUMN time_cost DOUBLE PRECISION
ADD COLUMN time_reverse_cost DOUBLE PRECISION;

-- Change the speed limit 0 to 25.
UPDATE nyc_road_direction_speed
SET postvz_sl = '25'
WHERE postvz_sl = '0';

-- Update the new columns with calculated time-based cost
UPDATE nyc_road_direction_speed
SET time_cost = CASE
                  WHEN cost > 0 THEN shape_leng::double precision / (postvz_sl::double precision * 1609.34 / 3600)
                  ELSE -1
                END,
    time_reverse_cost = CASE
                          WHEN reverse_cost > 0 THEN shape_leng::double precision / (postvz_sl::double precision * 1609.34 / 3600)
                          ELSE -1
                        END;
                    
 SELECT * FROM pgr_Dijkstra(
  'select gid as id, source, target, time_cost as cost, time_reverse_cost as reverse_cost from nyc_road_direction_speed',
  15916, 3253, true);
 
 SELECT * FROM pgr_Dijkstra(
  'select gid as id, source, target, time_cost as cost, time_reverse_cost as reverse_cost from nyc_road_direction_speed',
  15916, 3253, false);
 
```
### 6. [Traveling Salesperson Problem](https://docs.pgrouting.org/latest/en/pgr_TSP.html)
```sql
 SELECT * FROM pgr_TSP(
  $$SELECT * FROM pgr_dijkstraCostMatrix(
    'SELECT gid as id, source, target, cost, reverse_cost FROM nyc_road_direction_speed',
    ARRAY[9039, 36392, 47812, 59919, 31622],  -- List of node IDs to visit
    directed => false) $$);
```
## Contributing

Contributions to this tutorial are welcome! Please feel free to fork the repository, make your changes, and submit a pull request.

## License

This pgRouting Tutorial is provided under the MIT License.

## Contact

If you have any questions or feedback regarding this tutorial, please open an issue in this repository.
