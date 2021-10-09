# Cartographer Notes
## Questions
1. We use several scan of Tof to based a pseudo_scan, within the combining
    - How to we deal with the odometry error?
    - Also when combining a batch of sensor data, how to solve the motion shift.
1. Regulary run pose optimization
    - Use the global constraint to re-arrange / shift the submaps?
1. Resolution Problem, if the map is slightly off, like rotate 10 degree, then how the map matching is totally off?
1. What is nodes and scan generated in nodes means in cartographer
1. In global SLAM, trajectory node means submap?
1. in global SLAM, what is residuals?
## Solved: 
1. scan matcher, use the scan to match, then the constrains are:
    - between scan_x and submap_1 [Scan to Submap]
1. Pose Extrpolator
    - Other sensor to predict where is the next scan location


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
    - sensor data delivered "in batch"
    - point cloud downsampling(voxel_filter)
- Local SLAM Algo
    - inset new (assembled) scan into current submap, using initial guess from pose extrpolator.
    - (poseExtrapolator/realTimeCorrelativeScanMatcher) + CeresScanMatcher
    - CeresScanMatcher is a non-linear least square solver
    - RealTimeCorrelativeScanMatcher, search for similar scans within searching window, not that to evaluate between translation and rotation, you can manually set the weight for each of them. (When you think you understand your robot)
    - motion filterm only insert a new scan into submap[Grid2D] when the motion is above certain distance.
        - max_time, max_distance, max_angle
    - when Local SLAM algo stop? when it received certain number of scan.
    -submap representation:
        - Probability Grids, Occupancy Grid Map (https://www.youtube.com/watch?v=v-Rm9TUG9LA&ab_channel=CyrillStachniss)
            - static state binary bayes filter
            - assumption1 voxel is either occupancy or empty
            - world is static
            - cells are independent
            - p(m)/(1-p(not_m)) = uses_latest + recursive_term - prior
            - visit different prior model (we are similar to laser beam one)
            - using one beam, how to find the voxels that we need to update, bresenham's line algorithm
        - Truncated Signed Distance Fields (Description Link: https://www.youtube.com/watch?v=QgzxBN1m9WE&t=23s&ab_channel=PeterClaes])
- Global SLAM Algo
    - Run in batch
    - find constrain between nodes and submaps
    - non-global VS global constrains
        - non-global (intra submaps)
        - global (inter submaps; need to close enough)
    - all global constrains are downsampled (pure randomly)
    - Implementation:
        - FastCorrelativeScanMatcher (course) with Branch and bound
        - Cere Scan Matcher (fine)
    - global optimization takes multiple residuals



        


###
- finished submap and scan -> loop closure
    - scan matcher try to find the scan in the submap
    - if close enough --> add as a loop closing constraint


## ToCheck
    




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

### Related Materials
1. Particle Filter.
1. Graph-based SLAM.
1. What's the diff between scan-to-scan matching vs scan-to-map matching.
1. Branch-and-bound approach.