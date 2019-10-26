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
This part is coded line 124 through 201 from the main.cpp file.
At first we have to decide when the car need to change it's behavior. By default the car will go in a straight line at 49.5 MPH but then, if it find itself behind a slower car, it has to chose wether to slow down, go to the left or go to the right lane. To be sure of its decisions, the car will first have to know in which lane it is and if they are any other car at its left or right going to slow or to fast to let it change lane. For example, even if there is a car quite near in an other lane where we would like to go, if its going slower than our car, we can still go in its lane and be quite sure to avoid any collision.
Then, we also decide to go by default in the second line which allow us to turn left or right when we meet another slow car.
Finally, to be sure to finish every change of line we began, we use the 'changing' boolean which will be set as true while the car hasn't move from more than 3.3 meters in the 'd' coordinate.
Morover, if the car can't change lane, it will slow down to avoid to crash in the car in front and reaccelerate once the car is far enough. This part could be improve by using a PID controller to fit the front vehicle's speed.

All in all, to improve the model I can think at least of two way:
- First, we could use a cost function as explained during the course. This could be easier and more understandable while using more data like the accelerations of the other cars for example.
- Then, we could use a Deep Reinforcement Learning to train a neural network for the behavioral part. It could take the data of the sensor fusion as input and return which lane seems to be the better choice. We could reward it in function of it's main speed and give it malus when it hit another car.

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
