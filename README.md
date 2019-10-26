[image1]: Images/Highway_driving.png "Intro Pic"

# CarND Path Planning Project
In this project, the goal is to design a path planner that is able to create smooth, safe paths for the car to follow along a 3 lane highway with traffic.

![Driving Example][image1] 

### Introduction

In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

#### The map of the highway is in data/highway_map.txt
Each waypoint in the list contains [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.

### Description 

#### Trajectory generation
Most of this code comes from Aaron Brown's demonstration in the project FAQ. In order to create a smooth curve, we use the splines library which allows us to be sure that we do not have any oscillation in the trajectory while passing through all the points we choose.

#### Behavorial Planning
This part is coded on lines 124 to 201 of the main.cpp file.
First, we have to decide when the car should change its behaviour. By default, the car will go straight at 49.5 mph, but if it is behind a slower car, it will have to choose between slowing down, turning left or turning into the right lane. To be sure of its decisions, the car will first need to know which lane it is in and whether there is another car to its left or right that will be too slow or too fast to allow it to change lanes. For example, even if there is a car not far behind in another lane we would like to go, if it goes slower than our car, we can still go in its lane and be sure to avoid a collision.
Then, we also decide to go by default in the second line which allows us to turn left or right when we meet another slow car.
Finally, to be sure to complete each line change we have started, we use the "changing" Boolean which will be defined as true while the car has not moved more than 3.3 meters in the coordinate'd'.
In addition, if the car cannot change lanes, it will slow down to avoid crashing into the car in front of it and accelerate again once the car is far enough. This part could be improved by using a PID controller to adapt to the speed of the front vehicle.

Overall, to improve the model, I can think of at least two ways:
- First of all, we could use a cost function as explained during the course. This could be easier and more understandable if we used more data such as the accelerations of other cars for example.
- Then, we could use Deep Reinforcement Learning to train a neural network for the behavioral part. It could take the data of the sensor fusion as input and return which lane seems to be the best. We could reward it in function of it's main speed and give it malus when it hit another car. This should not require a very big neural network.

## Files description

The script files are in the `src` folder. All the other files or folders are there to ensure we can run the program successfully. 
Here are the main script files:
 
 - `helpers.h` contain some usefull function for the project.
 
 - `spline.h` is the library used for the spline functions.
 
 - `main.cpp` is of course the main script but take also care of the protocol.

### Getting Started

This project involves the Term 3 Simulator from Udacity which can be downloaded [here](https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).

Download the [CarND-Path-Planning repo](https://github.com/udacity/CarND-Path-Planning-Project).
(If you choose to use my repo to start, you should download this [Eigen-3.3](https://github.com/udacity/CarND-Path-Planning-Project/tree/master/src/Eigen-3.3) folder of libraries from the original repo and add it into your 'src' folder.)

Then install uWebSocketIO.
The Udacity repository includes two files that can be used to set up and install [uWebSocketIO](https://github.com/uWebSockets/uWebSockets) for either Linux or Mac systems. For windows you can use either Docker, VMware, or even [Windows 10 Bash on Ubuntu](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) to install uWebSocketIO.

Once the install for uWebSocketIO is complete, the main program can be built and run by doing the following from the project top directory.

1. mkdir build
2. cd build
3. cmake ..
4. make
5. ./path_planning


Then you only need to open the executable file in the `Term 3 Simulator` folder and launch the third project to watch the algorithm evovle.

### Other Important Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
