# pgRouting Tutorial

Welcome to the pgRouting Tutorial repository. This tutorial is designed to help users understand and implement network routing using pgRouting, an extension of PostGIS and PostgreSQL. The provided SQL scripts walk through several common tasks and challenges in network analysis.

## Overview

The tutorial covers:

1. **Creating a Topological Network:** Set up your network topology for routing with `pgr_createTopology`.
2. **Preparing Order Data:** Associate order locations with the nearest network nodes.
3. **Routing for Multiple Pairs:** Use `pgr_aStar` to find the best paths for multiple source-destination pairs.
4. **Directional Routing Differences:** Explore the implications of one-way vs. two-way streets with `pgr_dijkstra`.
5. **Speed Limit Considerations:** Incorporate speed limits into route planning for more accurate travel time estimates.
6. **Traveling Salesperson Problem (TSP):** Solve routing sequences for multiple locations with `pgr_TSP`.

## Prerequisites

To use this tutorial, you'll need:

- PostgreSQL with PostGIS and pgRouting extensions.
- A spatially enabled database with the `nyc_road_direction_speed` and `order` tables.

## Usage

The SQL files in this repository are meant to be run sequentially. Each file corresponds to a section in the tutorial:

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
