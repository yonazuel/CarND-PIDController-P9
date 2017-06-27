# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Reflection

### PID parameters

There are 3 parameters to tune: Kp, Ki and Kd.
I tuned all 3 parameters by hand:

* At first I used Kp = 1, which was too big. This caused the car to oscillate a lot, even in straight lines. So I decreased the Kp parameter untill Kp = 0.25.
* I started from a Ki parameter of 0.0001. This was a bit small and so the car did not stay at the center of the road (this could clearly be seen on the bridge). I increased this parameter up to 0.01, but this value was too big and the car failed to drive on the road in the steep turns. I finally use Ki = 0.005.
* At first I had Kd = 5. I increased it to 7 to avoid oscillations.

### Speed Control

The PID controller sets the steering angle, but I also need to set the throttle in order to control the speed of the car. I want the car to drive slowly when the steering angle is large and fast when the steering angle is low (straight line).

This is why I decided to first compute the steering angle via the PID controller and then compute a target speed to reach depending on this steering angle: `target_speed = min_speed + (max_speed - min_speed) * (1.0 - fabs(steer_value))`
I have fixed `min_speed = 10` and `max_speed = 50` (above 75 the car will fail to drive safely in the steep turns).

Then the throttle is simply the difference between the current speed and the target speed: `throttle = (target_speed - speed)`


