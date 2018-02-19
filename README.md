# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
## Generating Paths
### 1. Moving the vehicle
* I used five points to generate the spline for moving the vehicle forward. 2 points from the previous path and 3 new points with a gap of 30m.
* Points were created in Frenet coordinates and converted to XY values using getXY function. 
* Then I converted points in vehicle frame of reference and fitted a spline through them using spline.h library.
* Using this spline new points were appended to the next_x_values and next_y_values depending on the velocity desired, distance between points tells the vehicle velocity. Points were converted back to global frame of reference before appending them to the vectors.

### 2. Staying in Lane
* To stay in lane I used Frenet coordinates.  Points generated had there d value equal to (2 + 4*lane). This will keep the new points generated in the center of the lane, since width of each lane was 4m. 

### 3. Maintaining Speed Limit
* Speed of the vehicle depend on the the distance between the points appended to next_x_values and next_y_values. Formulas used were N = (t_dist/(0.02*ref_v/2.24)) and x_p = x_add + (t_x/N) in line 387 & 388 of the code. If the ref_v value is large N is small and distance between points is large.
* Used for loop to increase the value of ref_v to 49.5 mph slightly below the desired speed limit.

### 4. Keeping Max Acceleration and Jerk below desired limit
* To keep the acceleration within desired limit velocity was increased in the steps of 0.225 mph. Also when decelarating when vehicle was close velocity was reduced in steps of 0.225 mph.
* To prevent side ways jerk we used splines to generate the new path and also used points from previous path for path generation. This ensured smooth lane shift by vehicle. 

### 5. Collision avoidance
* To avoid collision vehicles sensor fusion data was used. d and s value of vehicles in front of our vehicle were used. When the difference between car_s and s value of front vehicle in same lane became less than 30, velocity of our vehicle was reduced by 0.225 mph until distance increases. 

### 6. Changing Lane
* To change the ego vehicle's lane, behavior planner was generated. I built two cost functions - Inefficiency cost function and change lane cost function.
* Inefficiency cost increases as the vehicle velocity became less than target velocity. It is 0 when velocity is equal to target velocity and increases exponentially as the difference increases.
* Change lane cost fuction uses sensor fusion data. When the vehicle in either left or right lane from the eqo vehicle is in the range of -12m to 35m cost function returned 1 else it returned 0.
* These two cost functions where used decide the state of vehicle. Whether to keep lane, change lane right or change lane left. 
* Vehicle maintained the lane until any vehicle came in front of it and velocity decreased.
* Then our cost functions gave values which tell us whether there are vehicles close by in the right or left lane. 
* Lane with the minimum cost was chosen to move the vehicle. 

### 7. Safety Features to avoid collision
* The car will slow down if distance from front vehicle becomes less than 30m.
* The car will slow down very very quickly if distance from front vehicle becomes less than 2.5m.
* Car can only change lanes if its velocity is greater than 35mph, this will prevent car getting hit from behind.
* If d value of cars in neighbouring lanes comes very close to cars d value, car will slow down quickly so that cars in the neighbouring lanes can go ahead without hitting the car. This generally means cars in side lanes are changing lane or behaving erraticaly. 
* Car can only change lane if there is safe gap of 50m in the new lane.



