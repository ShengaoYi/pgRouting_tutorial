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

### 1. Creating a Topological Network,  https://docs.pgrouting.org/latest/en/pgr_createTopology.html

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


1. **Network Creation:** Begin by running the network creation script to add necessary columns and create the topology.
2. **Data Preparation:** Execute the data preparation queries to calculate the nearest nodes for pickup and dropoff points.
3. **Multiple Pair Routing:** Use the provided routing script to calculate routes for multiple orders at once.
4. **Directional Routing Analysis:** Compare the results of routing with one-directional and two-directional considerations.
5. **Speed Limit Adjustment:** Adjust your network data for speed limits and recalculate routes based on time cost.
6. **TSP Solution:** Finally, explore the TSP solution using pgRouting functions.

## Contributing

Contributions to this tutorial are welcome! Please feel free to fork the repository, make your changes, and submit a pull request.

## License

This pgRouting Tutorial is provided under the MIT License.

## Contact

If you have any questions or feedback regarding this tutorial, please open an issue in this repository.
