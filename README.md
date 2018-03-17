# CarND-Path-Planning-Project
--- 

## Introduction

The goal of the project is to autonomous vehicle safely navigate around a virtual highway with other traffic that driving +-10 MPH of the 50 MPH speed limit.

### Objectives
* Vehicle driving must not exceed the speed limit and not much slower than speed limit unless there is traffic.
* The vehicle does not exceed a total acceleration of 10 m/s^2 and a jerk of 10 m/s^3.
* Vehicle must drive safely with out any collision with other vehicles.
* Vehicle must stays in the lane unless except for lanae changing.
* Lane changing must be smooth and deosn't take more than 3 sec.
---

## Implementation
This section descripe the implemetation of driving safely on a highway. I used the car's localization and sensor fusion data to determine where the other vehicles in the road with respect to the vehicle. In addition, by using previous path information in order to ensure smooth transition from cycle to cycle. 
### Sensor Fusion
Localizing the other vehicles determines the state of the vehicle either it should stay in the lane, or to change lane in line 274 to 348 in `main.cpp` file. I used the sensor fusion provided by the simulation `sensor_fusion` which is vector of vector that contains ` [ id, x, y, vx, vy, s, d]`. the x, y values are in global map coordinates, and the vx, vy values are the velocity components in the global map and they are saved in `vx,vy` magnitude velocity in `check_speed`. s and d are the Frenet coordinates for that car`d, check_car_s`. Firstly, from line 290 to 295 it check if there a car ahead in distance smaller than 30m and if there is a car ahead the `too_close= true`.

```python
check_car_s+=((double)previous_size*.02*check_speed);

if ((check_car_s>car_s)&&(check_car_s-car_s<30)){
    cout<<"too close"<<i<<endl;
    too_close=true;
    }
```
From line 299 to 345 it determine either there is a cars on lane 0, 1, 2 and the state saved in `car_lane0, car_lane1, car_lane2`. it check either the cars ahead or back within 30m so while lane changing the our vehicle should to ensure that no close car's in ahead or back.

From line 346 to 392 it detrmine the next state of the car either stay in lane, change lane left or shift lane right. It starts with decreasing the speed by .5 s/m in eche cycle and if the target lane is free the car's lane is setted to a new target lane according to the logic in the code. 

### Trajectory Generating

From line 400 to 492 in `main.cpp` based on determined target lane from sensor fusion and hestorical pathe, it computes the trajectory. The Fernet Coordinates `(s, d)` is used for path planning based on `ref_vel` which is maximum 49.5 m/s and the assigned lane to `lane`. At 30 meters interval a three waypoints as anchor points and these interpolate a smooth path between these using spline interpolation.  A constant acceleration is added to the reference velocity to ensure acceleration stays under 10m/s^2. The three anchor points are converted to the local coordinate space (via shift and rotation), and interpolated points are evenly spaced out such that each point is traversed in 0.02 seconds (the time interval).

---

## Result
The vehicle navigates safely around the virtual highway without any collision and in the speed limit.


