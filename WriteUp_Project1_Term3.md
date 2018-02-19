# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
## Generating Paths
1. Moving the vehicle
* I used five points to generate the spline for moving the vehicle forward. 2 points from the previous path and 3 new points with a gap of 30m.
* Points were created in Frenet coordinates and converted to XY values using getXY function. 
* Then I converted points in vehicle frame of reference and fitted a spline through them using spline.h library.
* Using this spline new points were appended to the next_x_values and next_y_values depending on the velocity desired, distance between points tells the vehicle velocity. Points were converted back to global frame of reference before appending them to the vectors.

2. Staying in Lane
* To stay in lane I used Frenet coordinates.  Points generated had there d value equal to (2 + 4*lane). This will keep the new points generated in the center of the lane, since width of each lane was 4m. 
*
