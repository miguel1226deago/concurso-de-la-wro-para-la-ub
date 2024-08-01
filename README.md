The modules that make up the code are based on the H-Bridge Arduino L293d TO connect the Arduino with the other components to control the car with stability.
without maneuvering or controlling the car, giving all that work to the infrared sensor to be able to complete the job successfully.

The electromechanical components that make up all that to put the car into operation are more adapted to the infrared, proximity and color sensors.
so that they give a signal where the car has to move if it is to the right or to the left 
The motors are used to operate all the components of the car so that it works correctly. 
the cables to send the corresponding energy

1. Robot Design:
The first step in creating the robot was designing the chassis and selecting it. We chose a suitable chassis that allowed us to mount all the necessary components, such as the wheel motors, sensors, and the Arduino board. The chassis needed to be robust enough to support the weight of all the components and ensure stability during movement.

2. Sensor Installation:
To enable the robot to perceive its surroundings, we installed several sensors:

TCS3200 Color Sensor: This sensor was placed at the front of the robot, allowing it to detect the colors of obstacles. 
We connected the sensor’s output pins to the Arduino's analogical pins 

Infrared (IR) Sensor: We installed two IR sensors on the bottom of the robot to detect the track’s lines or barriers. 
The IR sensors were connected to the Arduino's analogical pin

HC-SR04 Ultrasonic Sensor: We placed the ultrasonic sensor at the front of the robot to measure the distance to obstacles. 
The sensor’s trigger and echo pins were connected to the Arduino's digital pins 

3. Motor Connection:
To enable the robot’s movement, we connected the motors to an Motor shield 4x4, L293D, which was then connected to the Arduino and the battery.
 This setup allowed the Arduino to control the direction and speed of the motors.

4. Arduino Programming:
We developed the necessary code to control the motors and sensors.
 The code was designed so that the robot would follow the track’s lines, avoid obstacles, and make decisions based on the color of the obstacles. 
We used the TCS3200 color sensor to detect green and red obstacles, the ultrasonic sensor to measure the distance to obstacles,
 and the IR sensors to detect the track’s lines and counts them while calculating the turns that the vehicle must make