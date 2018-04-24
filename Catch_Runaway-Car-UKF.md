# Catch_Runaway_Carproject
## GitHub Repo

[CarND-Catch-Run-Away-Car-UKF](https://github.com/QuantumCoherence/CarND-Catch-Run-Away-Car-UKF)

This project was completed on Ubuntu 16.04.

The ecbuild subfolder contains all ECLIPSE project files, including compiled binary.

The build subfolder contains all the binaries and the makefile for compiling the project at the prompt with no formal IDE. 

**Compiling on your system: CMakelists.txt**

Use the CMakeLists.txt in the repo root folder to create the make file and compile on your system.


***The file UKF-Catch-Runaway-Car-result.jpeg is a screendump of the simulator at the end of the catch attempt.***

![Simulator Screen Dump](https://github.com/QuantumCoherence/CarND-Catch-Run-Away-Car-UKF/blob/master/UKF-Catch-Run-Away-Car-result.jpeg?raw=true)



## Strategy

**Use the UKF from previous project, find a way to put the chasing vehicle in front of the runawy car**

	
*Tigther Turn Strategy*

Since we know the runaway car is following a circle pattern and the chasing vehicle cannot go faster, all we need to do is put the chasing vehicle on a shorter route to get to the same place the robot is going, namely turn tigther than the runaway car, on a trajectory that will bring the chasing vehicle back on the same circular pattern.

The follwing equation tells the chasing vehicle to follow the position of the runaway car.

> target_x = ukf.x_(0);
> target_y = ukf.y_(0);

The future posistion of the robot car at time (now + dt) is given by its current position plus the longitudinal speed v multiplied by the the time. In x and y coordinates, this means multiplying the speed vector by the cos/sin of the angular direction of the speed vector, which give us the opportunty to make the turn tighter than the runaway vehicle.

We chose dt = 1 to start with. 
To make the turn tighter we chose to multiply the angular direction of the runaway car, by a small factor dpsi = 1.01. 

The following equations tell the chasing vehicle to aim at a position slightly on the left of the expected position of the runaway car. 

> target_x = ukf.x_(0) +  dt * cos(dpsi * ukf.x_(3));
> target_y = ukf.x_(1) +  dt * sin(dpsi * ukf.x_(3));

As the chasing vehicle turns tigther, its trajctory will turn left of the runaway car and cross into its path. 
As the relative positon will now be in front of the runaway car, the distance to the target will decrease , slowing down the chasing vehicle just enough to be in front of the runaway car, which will catch up to the chasing vehicle, getting caught. Trial and error with the dt and dpsi, lead to a final value of dpsi = 1.0001 with unchanged dt at dt=1; 

*State Machine an other strategies *

Other strategies include simply estimating the radius and center of the circular path and simply cross it and wait for the runawy car to come by and just get caught.

A state machine was used to implement this strategies. Unfortunately the simulator tends to return unstable hunter_heading values when target position and speed are conflicting, which is likley to happen when the runaway vehicle and the chasing vehicle are heading against each other, rather than in the same direction. 


**Remarks**

The current code includes such state machine, but it is not really used for the "Tigher Turn Strategy".
The video of the robot capture is ziped with gzip. Download from this link below and unzip to watch.

**Video**
[Chasing Video - download and unzip](https://github.com/QuantumCoherence/CarND-Catch-Run-Away-Car-UKF/blob/master/vokoscreen-2018-04-23_13-35-36.mkv.gz)

