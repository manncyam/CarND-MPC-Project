# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---

## The Goal of the project is for the car to drive in autonomous mode using Model Predict Control
I will describe the precedure below in detail. 
1. How to choose N & dt
2. How to fit Polynomial and preprocess MPC input
3. How to handle latency
4. How to fine tune MPC model
5. Recording video of the car driving for one lap

## 1. How to choose N & dt
T = N * dt
I wanted to have value of T equal to 1. Here are some values of N and dt with output result.

| N | dt | Result|
|---|----|-------|
|25|0.04| see Figure 1.|
|10|0.1| see Figure 2.|
|8|0.125| see Figure 3.|

Next I tried the value of T equal to 1.5

| N | dt | Result|
|---|----|-------|
|25|0.06| see Figure 4.|
|10|0.15| see Figure 5.|
|8|0.1875| see Figure 6.|

Finally, I tried the value of T equal to 0.5.

| N | dt | Result|
|---|----|-------|
|25|0.02| see Figure 7.|
|10|0.05| see Figure 8.|
|8|0.0625| see Figure 9.|

According to the result about I decided to use N = 8 and dt = 0.125

## 2. How to fit Polynomial and preprocess MPC input
I am using function polyfit to get fit polynomial third degree and get the coeffs as the result.
Before fitting points x and pointx y, I choose the current point as the origin. It means px=0 and py=0 and convert the rest of the point with respect the car position.
## 3. How to handle latency
To handle the latency I used the prior to the previous delta and acceleration as shown in the code below
```
    // Handle latency by using the prior to the previous delta and acc
	  if(t > 1)
	  {
	    delta0 = vars[delta_start + t - 2];
        a0 = vars[a_start + t - 2];	  
	  }
```
## 4. How to fine tune MPC model
I declared some constant variables so I can fine tune their values to get the desirable outcome.
I started with the value below:
```
// Tuning factor 
const size_t cte_start_factor{1};
const size_t epsi_start_factor{1};
const size_t v_start_factor{1};
const size_t delta_start_factor{1};
const size_t  a_start_factor{1};
const size_t  dv_start_factor{1};
const size_t ave_v_start_factor{1};
const size_t ave_a_start_factor{1};
```
Then
// Tuning factor 
const size_t cte_start_factor{3};
const size_t epsi_start_factor{3};
const size_t v_start_factor{10};
const size_t delta_start_factor{1};
const size_t  a_start_factor{1};
const size_t  dv_start_factor{3};
const size_t ave_v_start_factor{1};
const size_t ave_a_start_factor{1};
```
Finally, I choose these values:
// Tuning factor 
const size_t cte_start_factor{3};
const size_t epsi_start_factor{3};
const size_t v_start_factor{10};
const size_t delta_start_factor{1};
const size_t  a_start_factor{1};
const size_t  dv_start_factor{3};
const size_t ave_v_start_factor{1};
const size_t ave_a_start_factor{1};
```
## 5. Recording video of the car driving for one lap


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
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Fortran Compiler
  * Mac: `brew install gcc` (might not be required)
  * Linux: `sudo apt-get install gfortran`. Additionall you have also have to install gcc and g++, `sudo apt-get install gcc g++`. Look in [this Dockerfile](https://github.com/udacity/CarND-MPC-Quizzes/blob/master/Dockerfile) for more info.
* [Ipopt](https://projects.coin-or.org/Ipopt)
  * Mac: `brew install ipopt`
       +  Some Mac users have experienced the following error:
       ```
       Listening to port 4567
       Connected!!!
       mpc(4561,0x7ffff1eed3c0) malloc: *** error for object 0x7f911e007600: incorrect checksum for freed object
       - object was probably modified after being freed.
       *** set a breakpoint in malloc_error_break to debug
       ```
       This error has been resolved by updrading ipopt with
       ```brew upgrade ipopt --with-openblas```
       per this [forum post](https://discussions.udacity.com/t/incorrect-checksum-for-freed-object/313433/19).
  * Linux
    * You will need a version of Ipopt 3.12.1 or higher. The version available through `apt-get` is 3.11.x. If you can get that version to work great but if not there's a script `install_ipopt.sh` that will install Ipopt. You just need to download the source from the Ipopt [releases page](https://www.coin-or.org/download/source/Ipopt/) or the [Github releases](https://github.com/coin-or/Ipopt/releases) page.
    * Then call `install_ipopt.sh` with the source directory as the first argument, ex: `sudo bash install_ipopt.sh Ipopt-3.12.1`. 
  * Windows: TODO. If you can use the Linux subsystem and follow the Linux instructions.
* [CppAD](https://www.coin-or.org/CppAD/)
  * Mac: `brew install cppad`
  * Linux `sudo apt-get install cppad` or equivalent.
  * Windows: TODO. If you can use the Linux subsystem and follow the Linux instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions


1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Tips

1. It's recommended to test the MPC on basic examples to see if your implementation behaves as desired. One possible example
is the vehicle starting offset of a straight line (reference). If the MPC implementation is correct, after some number of timesteps
(not too many) it should find and track the reference line.
2. The `lake_track_waypoints.csv` file has the waypoints of the lake track. You could use this to fit polynomials and points and see of how well your model tracks curve. NOTE: This file might be not completely in sync with the simulator so your solution should NOT depend on it.
3. For visualization this C++ [matplotlib wrapper](https://github.com/lava/matplotlib-cpp) could be helpful.

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/b1ff3be0-c904-438e-aad3-2b5379f0e0c3/concepts/1a2255a0-e23c-44cf-8d41-39b8a3c8264a)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./
