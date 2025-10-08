.. raw:: html 
   
   <meta name="robots" content="noindex">
   
############################
Robot Kinematics
############################

In FIRST Robotics Competition (FRC), kinematics is the study of how a robot's drivetrain moves, relating the robot's overall movement (like speed and turning) to the speeds of its individual wheels. It's crucial for calculating how to move the robot and for estimating its position on the field, with key classes like SwerveDriveKinematics and MecanumDriveKinematics used in the WPILib software library to manage the complex translations between robot and field-relative speeds for different drive types. 

Kinematics is the study of the geometry of motion. Kinematics is the branch of classical mechanics that describes various aspects of motion such as velocity, acceleration, displacement, time, and trajectory.  There is no description of the forces that cause the motion to happen. The word “kinematics” comes from a Greek word “kinesis” meaning motion, and is related to other English words such as “cinema” (movies) and “kinesiology” (the study of human motion).

In robotics we are mostly interested in the motion of groups of objects that are connected, or jointed, together into a system.  When objects are connected together they can be subject to certain constraints that restrict their movement.  For wheeled robots we are interested in determining how the robot can move with reference to the constraints imposed by the motion of the wheels. So that's where we'll start.

We want to examine the constraints that exist by the connection of the wheels to the robot.  A wheel is connected to an axel which is typically driven by a motor.  The motor causes the wheel to rotate which produces forward, or backward, motion of the robot.  Nothing too suprising there!  What is of note though, is that the wheel cannot move sideways to its direction of motion, so here's where we find our first constraint. 

Our next constraint comes in when we have more than one wheel on the robot, which is of course the common configuration.  When we have multiple wheels we must ensure that they have a common *Instantaineous Center of Curvature (ICC)*.  This means that the wheels must be connected in a way that they are all aligned towards the same point.  If this is not the case then the wheels will slip in a sideways direction making it extremely difficult to control the motion of the robot.  This is illustrated below.  Typically, there are two wheels on the same axel which causes them to be aligned towards the same center point.  However, with a 4-wheeled robot there are two sets of wheels and two axels which means that the back wheels will always slip.  These are called skid drive robots.

.. image:: /images/RobotConcepts/FRCKinematics&Odometry.001.jpeg 

Swerve Drive Robots
*******************************************

The function `toSwerveModuleStates()` performs inverse kinematics to get the module states from a desired chassis velocity. 

The function `toChassisSpeeds()` performs forward kinematics to return the resulting chassis state from the given module states. This method is often used for odometry -- determining the robot's position on the field using data from the real-world speed and angle of each module on the robot.

The function `desaturateWheelSpeeds()` renormalizes the wheel speeds if any individual speed is above the specified maximum. Sometimes, after inverse kinematics, the requested speed from one or more modules may be above the max attainable speed for the driving motor on that module. To fix this issue, one can reduce all the wheel speeds to make sure that all requested module speeds are at-or-below the absolute threshold, while maintaining the ratio of speeds between modules.

Inverse Kinematics
***********************************

FRC Inverse Kinematics is a robotic system where the desired target position and orientation of the robot's end-effector are given, and the system calculates the necessary joint angles (or motor rotations) to reach that target. It involves finding the appropriate configuration of joints to achieve a specific goal, such as reaching for an object, and is crucial for controlling robotic arms in the FIRST Robotics Competition (FRC).  

How are they Calculated? 
*************************************

FRC kinematics are calculated by defining a system to represent robot motion (like the ChassisSpeeds class in WPILib), and then using either forward or inverse kinematic equations to relate the robot's desired movement to the motor speeds and module angles required. Forward kinematics calculates the robot's overall movement (linear and angular) from individual wheel speeds, while inverse kinematics calculates the wheel speeds needed for a desired robot movement. The specific equations depend on the drivetrain type (e.g., tank or swerve), with WPILib providing a powerful abstraction for common FRC drivetrains.


References
******************************************

* FRC Documentation - `Kinematics and Odometry <https://docs.wpilib.org/en/latest/docs/software/kinematics-and-odometry/index.html>`_  