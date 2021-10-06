# Cartographer Notes
## Questions
1. Branch-and-bound approach
1. What's the diff between scan-to-scan matching vs scna-to-map matching
1. Particle Filter 
1. Graph-based SLAM
1. We use several scan of Tof to based a pseudo_scan, within the combining, how to we deal with the odometry error?
1. Regulary run pose optimization
    - Use the global constraint to re-ordered / shift the submaps?
1. scan matcher, use the scan to match, then the constrains are:
    - between scan_x and submap_1
    - OR
    - between submap_x and submap_1
1. Resolution Problem, if the map is slightly off, like rotate 10 degree, then how the map matching is totally off?
1.what is nodes and scan generated in nodes means in cartographer


## Workflow
    - Overview
        - local SLAM
            - build succession of submaps
            - locally consistent
            
        - global SLAM
            - scan (generated in nodes)
            - find loop closure constrauins
    - Input
        - Depth Info go through a bandpass filter
            - manually set min, max range, numbers are choosen based on the sensor and robot task.
            - related parameters: 
                TRAJECTORY_BUILDER_nD.min_range, TRAJECTORY_BUILDER_nD.max_range
                TRAJECTORY_BUILDER_2D.missing_data_ray_length, max_z, min_z
    



###
- finished submap and scan -> loop closure
    - scan matcher try to find the scan in the submap
    - if close enough --> add as a loop closing constraint


## ToCheck
    - TRAJECTORY_BUILDER_nD.min_range, TRAJECTORY_BUILDER_nD.max_range
    - TRAJECTORY_BUILDER_2D.missing_data_ray_length, max_z, min_z




## Ideas
- Utilize the odometry range, in the map building process
    - Intoduction
        - What is the freq of inserting new scan into submap
        - Not searching the entire space. but only search the odometry range
    - Things ToDo
        1. collect data, in the test area, what is the odometry diff range
        1. formulate the range into math, and test in slam test dataset

- Presentation of the big map
    - after global optimizer, which part changed?
        - only the relation between the submaps
        - OR others also changed
    


<!-- ### branch-and-bound approach -->

###
