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

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Reflection 

### Describe the effect each of the P, I, D components had in your implementation

#### 1. P-Controller : Steering in proportion to the cte (cross track error)

In this video, it only used P-Controller with tau_p = 0.45832 to control the steering of the car.  You can see that the car adjusts the steering in proportion to the cte and oscillates around the lane line and often times it over-shoots for the correction.  

<a href="https://www.youtube.com/watch?v=l11zHNOlHg4&feature=youtu.be" target="_blank"><img src="http://img.youtube.com/vi/l11zHNOlHg4/0.jpg" alt="P Controller"/></a>


#### 2. PD-Controller : Steering in proportion to the cte and derivatives of the cte errors 

In this video, it only used PD-Controller with tau_p = 0.45832 and tau_d = 2.61739 to control the steering of the car.  You can see that the car adjusts the steering in proportion to the cte but reduces the error and avoids over-shooting of the steering adjustment.  

<a href="https://www.youtube.com/watch?v=URwPJn-I9vE&feature=youtu.be" target="_blank"><img src="http://img.youtube.com/vi/URwPJn-I9vE/0.jpg" alt="PD Controller"/></a>




#### 3. PID-Controller : Steering in proportion to the cte and derivatives of the cte errors and integrals of cte errors

In this video, it used PID-Controller with tau_p = 0.45832 and tau_d = 2.61739 and tau_i = 0.000802943 to control the steering of the car to adjust for the drifting effect.  But since the tau_i is so small, the effect of it is not obvious. 

<a href="https://www.youtube.com/watch?v=elYyhijNusw&feature=youtu.be" target="_blank"><img src="http://img.youtube.com/vi/elYyhijNusw/0.jpg" alt="PID Controller"/></a>


### Describe how the final hyperparameters were chosen

The hyperparameters were tuned iteratively using the Twiddle method:

1. I first set the initial parameters p[] = {0,0,0}, dp[] = {0.1,0.1,0.1}, iterations = 100 and the threshold = 0.1.  And I found that whenever the p[1] tau for integrated cte was not zero, the car change the orientation significantly.  So, I realized the dp[1] = 0.1 is probably to high.  After a few iterations, i updated the initial parameters p[] = {0.5,0,1.0}, dp[] = {0.1,0.001,0.1}, iterations = 200 and the threshold = 0.1.

2. With the updates in the intial parameters, the car drove more smoothly and the sum of dp[] is lower than the threshold at p[]={ 0.596141, 0.000526023, 1.22713}.  However, when I let the car run through the course, the car drove off track in one of the turns.  Therefore, i increased the iterations to 750 and lowered the threshold to 0.03 and trained it again. 

3. With a few more updates,I found that with p[] = {0.45832, 0.000802943, 2.61739} and dp[] = {0.0048779, 0.0000826086, 0.0134385}, iterations = 2000 and threshold = 0.2 and updated the "throttle" to 0.2 allowed the car drive reasonably smoothly within the course. 



