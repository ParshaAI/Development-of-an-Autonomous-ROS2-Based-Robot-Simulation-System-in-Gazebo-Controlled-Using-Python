# Development-of-an-Autonomous-ROS2-Based-Robot-Simulation-System-in-Gazebo-Controlled-Using-Python

This section aims to provide a detailed overview of the implementation strategy used for developing an AI-based cognitive architecture to control a robot’s behaviour. The architecture is designed to help the robot perceive, decide, and act in response to its environment, simulating human-like behaviour. The report covers the implementation method, challenges encountered, solutions, and a proposed system diagram.
This project mainly focuses on the development of an autonomous four-wheeled robot simulation using ROS2 Humble and Gazebo, combining a Lidar-based obstacle avoidance system and a camera for real-time environmental awareness. The system helps the robot navigate virtual environments while avoiding obstacles and streaming live images from the camera.

The primary objectives of this project:	
•	Creating a mobile robot model in Ignition Gazebo with a rectangular base, four wheels, and a camera mounted on a rod.
•	Implement ROS2 Actions using client-server to control the front two wheels and the camera’s movement.
•	Combining Lidar-based obstacle avoidance system within the server script to enable autonomous navigation.
•	Develop a Python-based ROS2 node to control the robot’s movement using data from the sensors.
•	Ensure seamless interaction between the robot model and ROS2 communication framework via bridges.

Key components of the architecture:
The architecture is divided into three main layers:
Perception layer: processes sensor data to understand where it is located.
Decision layer: uses the data retrieved by the sensors to make decisions regarding which one is the best action given a situation.
Action layer: converts these decisions into actions through executable commands for the robot’s movement.


2.	System Design and the Implementing Strategy
The robot was designed via lines of html code and was executed to open in Ignition Gazebo. The key components of the robot are:
•	Rectangular base: provides a structure.
•	Four wheels: two front wheels are controllable. Rear wheels don’t have any movement.
•	Lidar sensor: placed at the front to enable distance measurement for obstacle detection.
•	Motorised camera: mounted on a rod with a revolute joint, allowing independent angle adjustments via ROS2 actions.

The model was built using SDF files to define its structure, sensor placements, and movement restrictions.

3.	ROS2-based control architecture
The system relies on ROS2 nodes for communication between components. The core modules are:
Robot Action Server Node and Robot Action Client Node (robo_action_server.py and robo_action_client.py)
It subscribes to the Lidar sensor data which was divided into three main sections (left, front, right) and determines the closest obstacles using rule-based logic. If an obstacle is in front, it checks for space in the side regions and picks the side with the most space by reversing and moving in that direction. Otherwise, it moves forward when it sees no obstacles. These instructions are sent by the client node that gathers feedback and sends actions to the server node. The server node then publishes commands to /model/vehicle_blue/cmd_vel to control the robot’s movement. 

4.	AI based cognitive architecture
The Decision-making approach:
The robot follows a hybrid cognitive architecture, blending reactive and rule-based decision-making:
Architecture	Characteristics	Implementation
Subsumption 	No memory, responds to sensor input in real time	Lidar-based obstacle detection and avoidance
Sense-Think-Act 	Pre-defined IF-THEN rules for navigation	Turn left or right based on where the obstacle is located
Hybrid	Combines real-time reactions with simple decisions	Moves based on obstacle positions
Table 4.1
Reasoning:
The subsumption architecture is a reactive architecture where the robot responds directly to sensory input without storing previous states or planning for the future. It can subsume the roles of lower layers. 
The sense-think-act architecture is based on a rule system. It senses the environment and makes decisions based on pre-defined rules such as if an object is here move left or right stuff. It is simple and deterministic. 
The hybrid architecture blends both real-time reactive decision-making and higher-level rule-based decision making. It takes the characteristics of both by being fast and effective. 

AI system diagram:
The AI-based cognitive system contains these layers:
 
Fig 4.1
Reasoning:
The environmental layer represents the Gazebo simulation environment where the robot operates and interacts with the world. The sensor layer handles the input from the lidar sensor and the camera sensor for detecting obstacles and for live image feed. The processing layer has ROS2 nodes responsible for processing the sensor data and for its behaviour. The decision layer handles the decision-making part. The robot uses a hybrid approach which includes reactive control and rule-based logic. The last layer, the execution layer is where all the communication occurs with the robot actuators. It includes ROS2 topics for sensor data like laser_scan and image_raw and the movement command like cmd_vel. The action server controls them.
This structure ensures that each component has its own role, with clear communication with Ignition Gazebo and ROS2 using bridges to map values. The hybrid approach blends reactive and rule-based logic decisions to help the robot navigate its environment.
5.	Challenges and solutions
There were several challenges when it came to implementing the robot. Most of the time was utilised to figure out how the communication system works between ROS2 and Ignition Gazebo. There were several plugins, and it was important to ensure the right plugin was chosen for the robot’s movement. Each plugin has its own version of Gazebo, and it was necessary to understand what version was suitable for this robot. The plugin challenge was overcome by going through the tutorials of the module where the plugin suitable for this version was found. Then construction of the bridge between ROS2 and Ignition for robot movement enabled the robot to move on its own.
The second challenge was obstacle avoidance which was the hardest one to overcome. The lidar was able to detect the obstacles but the logic that processes this sensor data was failing to control the robot’s wheels. Multiple attempts were made to mitigate the issue, and lastly, a solution was found by using the server nodes.
The other challenge was the location of the lidar as initially the lidar was failing to scan what was ahead of the robot and scanned what was below. This issue was overcome by carefully understanding the position of the ‘chassis’ the base, and positioning the lidar with respect to chassis, thus, ensuring that the lidar was correctly positioned later. 
6.	Conclusion and future work
In this project, an autonomous robot was successfully developed, capable of navigating through its environment using Lidar sensor for obstacle detection and streaming real-time images from the camera. The robot’s decision making was guided by a hybrid cognitive system which combined both sense-think-act approach and the subsumption approach for obstacle detection and avoidance.
For future work, the robot’s cognitive architecture could be further enhanced by including more advanced AI techniques such as machine learning algorithms for improved object recognition instead of just detection and also include predictive navigation. In addition to that, the system’s accuracy can be optimised by integrating more diverse sensors, refining the decision-making process. 




7.	References:
Setting up a robot simulation (gazebo) (no date) Setting up a robot simulation (Gazebo) - ROS 2 Documentation: Humble documentation. Available at: https://docs.ros.org/en/humble/Tutorials/Advanced/Simulators/Gazebo/Gazebo.html#launch-the-simulation (Accessed: 31 March 2025). 

Writing a simple service and client (python) (no date) Writing a simple service and client (Python) - ROS 2 Documentation: Foxy documentation. Available at: https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.html (Accessed: 31 March 2025). 

Writing a simple publisher and subscriber (python) (no date) Writing a simple publisher and subscriber (Python) - ROS 2 Documentation: Foxy documentation. Available at: https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.html (Accessed: 31 March 2025). 

Kabilankb (2024) Building a simple ROS2 object avoidance robot using Python, Medium. Available at: https://medium.com/@kabilankb2003/building-a-simple-ros2-object-avoidance-robot-using-python-962f5b8485d7 (Accessed: 31 March 2025). 





