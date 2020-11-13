# ST-Matching
A Python implementation of the ST-Matching algorithm for low-sampling-rate GPS trajectories [1]. 

> [1] Lou, Y., Zhang, C., Zheng, Y., Xie, X., Wang, W. and Huang, Y., 2009, November. Map-matching for low-sampling-rate GPS trajectories. In Proceedings of the 17th ACM SIGSPATIAL international conference on advances in geographic information systems (pp. 352-361). ACM.

Note that codes of the 'S (spatial)' part of the algorithm is currently unavailable in this distribution.

# Getting Started

Files containing the geographical information of the road network you want to match your trajectories to are requied:

(1) node file: a comma-separated file cotaining at least three columns: ['node', 'lng', 'lat'], where 'node' is the identification of the breakdown points of your road network, 'lng' and 'lat' the longitudes and latitudes of nodes. 

(2) edge file: a comma-separated file cotaining at least three columns: ['edge', 's_node', 'e_node', 's_lng', 's_lat', 'e_lng', 'e_lat', 'c_lng', 'c_lat'], where 'edge' is the identification of the road segments of your road network, 's_node' and 'e_node' the begining nodes and ending nodes of segments, 's_lng', 's_lat' the coordinates of 's_node', 'e_lng', 'e_lat' the coordinates of 'e_node', 'c_lng', 'c_lat' the coordinates of the mid points of edges.

(3) network properties file: a comma-separated file cotaining at least four columns: ['section_id', 's_node', 'e_node', 'length'], where 'section_id' is the identification of the road segments of your road network, 's_node' and 'e_node' the begining nodes and ending nodes of segments, 'length' the lengths of road segments.

The path of above three files should be specified in ```STmatching_distribution_ver.py```.

The trajectory file must be a comma-separated file cotaining at least three columns: ['TRAJ_ID', 'LON', 'LAT'], where 'TRAJ_ID' is the identification of each trajectory.

An example to run this library in parallel:
```
from STmatching_distribution_ver import *
from multiprocessing import cpu_count, Pool

trajectory = pd.read_csv('your_trajectory_path')
trajectory = data_convert(trajectory)
pool = Pool()
match_results = pool.map(trajectory_matching, trajectory)
pool.close()
match_results = pd.concat(match_results, ignore_index=True)
```
Matching results are restored in a dataframe match_results: ['TRAJ_ID', 'MATCHED_EDGE', 'MATCHED_NODE'], where 'MATCHED_EDGE' is the matched sequence of road segments, and 'MATCHED_NODE' the matched sequence of road nodes.

# License
ST-Matching is under Apache License. See LICENSE file for full license text.
