# Cartographer Notes
## Questions
1. Branch-and-bound approach
1. What's the diff between scan-to-scan matching vs scna-to-map matching
1. Particle Filter 
1. Graph-based SLAM
1. We use several scan of Tof to based a pseudo_scan, within the combining, how to we deal with the odometry error?
1. Regulary run pose optimization

## Workflow
- finished submap and scan -> loop closure
    - scan matcher try to find the scan in the submap
    - if close enough --> add as a loop closing constraint




## Ideas
- Utilize the odometry range, in the map building process
    - Intoduction
        - What is the freq of inserting new scan into submap
        - Not searching the entire space. but only search the odometry range
    - Things ToDo
        1. collect data, in the test area, what is the odometry diff range
        1. formulate the range into math, and test in slam test dataset

- 


<!-- ### branch-and-bound approach -->

###