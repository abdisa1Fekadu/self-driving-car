# Self-Driving Vehicle Control
This respository explains an approach implemented for the final project of the course, Introduction to Self-Driving Cars from the Self-Driving Cars Coursera Specialization.

The aim of this project was to implement a controller in Python to drive a car around a track in the Carla Simulator. To control successfuly the vehicle, both longitudinal and lateral controllers were implemented, in order to obtain the throttle, brake and steering control signals.

<p align = "center">
<img src = "./demo_gif/Video_GIF.gif" height = "360" width = "630">
</p>

## Tools
The tools used for this project were:
* Python
* Carla Simulatior

## Script
Access the [Python script](https://github.com/kmilo7204/Self-Driving_Vehicle_Control/blob/master/controller2d.py) of the project. The `controller2d.py` contains the implementation of both controllers. The outputs of each controller are sent to the script `module_7.py` which connects our controllers with the Carla Simulator.


## Controller

### Longitudinal controller
For the vehicle longitudinal control, a PID controller was selected due to its great results provided.

The PID controller takes as input the error speed, defined as the difference between the desired speed and the current speed of the vehicle and outputs the throttle and the brake. The PID controller was implemented into a feedback architecture.

In this case, I avoid the use of a low level controller (After the PID) as the desired speed are relatively low and steady. A Feedforward controller could be implemented as well to obtain better results, but in this case not enough information was provided.

>**Note:** I used the Tustin discretization method to implement the controller in Python.

### Lateral controller
For the vehicle lateral control, the Stanley controller was selected due to its great performance and the huge amount of information found.

The Stanley controller uses the centre of the front axle as the reference point. It looks at both the error in heading (Heading error) and the error in position relative to the closest point on the path (Crosstrack error) to define an intituive steering law.

The next three steps were followed to implement successfuly the controller in Python:

1. Firstly, using the front axle coordinates and the closest waypoint coordinates to the vehicle, I calculated the Crosstrack error.

2. To estimate the heading error, I calculated the current angle of the road (Using the current and the next waypoint), then, to this same value, I substracted the current yaw angle of the vehicle to obtain the Heading error.

3. Finally, to find the total steering angle I added both the crosstrack and the heading errors.

### Results
#### Longitudinal control
To evaluate the Longitudinal controller the next image is provided. In the graph, the speed profile proposed to drive the car around the track is in orange, in blue the real vehicle speed obtained by using the PID controller.

<p align = "center">
<img  src = "./controller_output/Grade.png">
</p>

>During the whole trajectory, the controller provided the right outputs to the vehicle to mantain the desired speed within the proposed profile; however, at some point (between the 500 to 750 waypoints) the controller did not reach the desired speed. 


#### Lateral control
Similar to the Longitudinal controller, a image is provided to shown the performance of the Lateral controller. In the graph, the trajectory proposed to drive the car around the car is shown in blue, in orange is the trayectory followed by the vehicle, obtained by using the Stanley Controller.

<p align = "center">
<img  src = "./controller_output/Waypoints%20Solution.png">
</p>

>In this case, the vehicle followed the desired trajectory. The Stanley controller had a great performance overall and its precision was good enough for this case.


A possible solution for this issue is to force a brake input to helps helps the vehicle to reduce the speed and reach the desired speed.
Implementing an MPC 
