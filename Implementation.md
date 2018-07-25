# CarND-Path-Planning-Project

## Implementation details

I completed the project in three stages.

#### Stage 1. Initially I started with basic implementation of driving the car in the lane 1 with constant speed.
#### Stage 2. In the next stage, I used the sensor_fusion data to detect the vehicles in same lane to control the velocity so that the car won't collide with slower vehicles ahead.
#### Stage 3. In the final stage, I implemented lane changing behavior whenever there is slower vehicle ahead and a faster adjacent lane is available.

### Stage 1: 

In this stage, my goal is to drive the car at constant speed in lane 1. To do that 
1. I start with 5 waypoints which are sparsely spaced at 30m apart. The first two points are at either start at previous waypoints or car's current location and last 3 points are lane 1 with 30m apart. I start with frenet co-ordinates and use getXY() method to get correspoinding x,y co-ordinates for these waypoints.
2. I transform the co-ordinate system so that origin is at car's location and car's direction is at zero angle. This makes the math easy for next step
3. In this step, I generate a total of 50 continous waypoints so that car's speed is maintained at ref_vel. I used spline library to generate these waypoints.
4. Now I convert the co-ordinates back to global co-ordinates by doing reverse tranform of step 2 and pass them to simulator.

### Stage 2:

In this stage I iterate over sensor_fusion data to see if there any other cars in our lane and if so check the distance to see its less than 30m. If the distance is less than 30m then we reduce the car's target_velocity to the speed of vehicle ahead.

The speed of the vehicle is reduced in increments of 0.224 so that we don't violate max acceleration limit. This is implemented in generation of waypoints using spline library.

### Stage 3:

In this stage, whenever the vehicle's target velocity is less than speed limit, we try to see if we can change lane to drive faster. This is done by
1. Checking all the lanes for its availability and lane speeds. If there is no vechicle within 30m of of our car then the lane is marked as available otherwise it is marked as unavailable. The lane speed is set to speed_limit initially and if there is a vehicle wihtin 60m ahead with lesser speed then the lane speed is updated to its speed.
2. With above data, we change the lane if an adjacent lane is available and its lane_speed is faster than current car speed.
3. Once the lane is changed, then spline library will generate the waypoints to smoothly drive the car over a distance of 30m so it doesn't violate jerk limit.


