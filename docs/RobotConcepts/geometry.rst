.. raw:: html 
   
   <meta name="robots" content="noindex">
   
############################
Robot Geometry
############################

In order to control a robot it’s necessary to know its position and orientation — where it is at any particular time.  Its position in 3-D space is defined using the set of coordinates X, Y, and Z.  This coordinate system is called the **Cartesian Coordinate System**.  When the robot moves from one position to another it’s referred to as a translation.  When it rotates about its center point then it has changed orientation. 

For the mobile robots that we use for competition we are mostly concerned 2D space so most of this section will focus on that. There will however be times when we would need to know our pose on 3D space so that will be covered later.

.. image:: /images/RobotConcepts/FRCGeometry.001.jpeg 

Moreover, the position needs to be specifed relative to a suitable reference frame.  When we are navigating in a car using a GPS system our reference frame is the Earth.  In the case of FRC, a suitable reference frame is the competition field.

2D Geometry
***************************

.. image:: /images/RobotConcepts/FRCGeometry.006.jpeg 

Let's start by examining the orientation or rotation of the robot.  This is commonly done by using *Euler Angles*.

.. image:: /images/RobotConcepts/FRCGeometry.003.jpeg 

In *First Robotics* we don't use flying robots so we usually don't need to keep track of objects in 3D space.  The *WPI Library* therefore has a set of data structures that keeps track of things in 2D space. Since we're looking at Euler Angles let's first examine the *Rotation2d* class that keeps track of rotations in 2D space.  When creating an object from this class you can pass it a starting angle that describes a rotation offset from the (𝑥, 𝑦) orientation of the game field. This offset represents the orientation at which you placed the robot down on the field.  If no starting angle is passed then it's assumed that you aligned the robot with the fields' (𝑥, 𝑦) orientation.

.. image:: /images/RobotConcepts/FRCGeometry.014.jpeg 

Once you have a *Rotation2d* object you can continuously track the robots' orientation by adding or subtracting changes that are sensed from the gyro. This is done using the rotation matrixes shown in a previous diagram. 

.. image:: /images/RobotConcepts/FRCGeometry.015.jpeg 

Translation - Change in Position
*********************************************

Translation is the change in position of an object. It's also referred to as *displacment* - how far has the object moved from the last position relative to a fixed frame of reference.  In our case, the fixed reference frame would be the competition field.  In order to represent how a robot changes its position we use the *Translation2d* data structure. When creating an object from this class you can pass in a starting position for the robot, which represents its (𝑥, 𝑦) displacement from the origin.  If no starting position is defined then it's assumed that the robot starts at the origin (0, 0).  Note that the origin of the reference frame can be different in various cases.  For the FRC field it's placed at the top left corner as you look down on the field.

.. image:: /images/RobotConcepts/FRCGeometry.010.jpeg 

When the robot moves from one position to another you can update the *Translation2d* data structure by sending it the change in position since the last update. The change is added to the current position to represent the robots' new location.


.. image:: /images/RobotConcepts/FRCGeometry.008.jpeg 

Robot Pose
******************************

The common way to keep track of the position and orientation of a robot is to create a class called a *Pose*.  This class comprises the two subclasses described previously, namely *Translation2d* and *Rotation2d*.  The translation class keeps track of the robot’s position whereas the rotation keeps track of its orientation.  Why use translation and rotation instead of position and orientation?  We want to know where the robot is as it travels from one position to another.  We also want to know the robot’s heading as is rotates from one orientation to another.

.. image:: /images/RobotConcepts/FRCGeometry.017.jpeg 

3D Geometry
**************************************

Robot orientation is described with roll, pitch, and yaw as sequential rotations around the robot's X, Y, and Z axes, respectively, in a specific order (like ZYX or XYZ) to define a 3D orientation. Roll rotates the tool about its center axis, pitch tilts it up and down, and yaw turns it left and right, analogous to a vehicle's movements.

Understanding the Rotations
-------------------------------------- 

   * Roll: Rotation around the tool's own axis, like turning a doorknob or spinning a screwdriver.
   * Pitch: Tilting the tool up and down, like nodding "yes".
   * Yaw: Turning the tool left and right, like shaking your head "no". 

How They Define Orientation
---------------------------------

   * Coordinate Frames: Start with a base coordinate frame (space frame) and the robot's body frame. 

   * Sequence of Rotations: The orientation is defined by a series of three rotations around the axes, typically denoted as {X, Y, Z} for the axes: 

       * First, rotate around one axis (e.g., Z for yaw).
       * Then, rotate around a second axis of the now-rotated frame (e.g., Y for pitch).
       * Finally, rotate around a third axis of the twice-rotated frame (e.g., X for roll). 

Order Matters
-----------------------

The result of the rotations depends on the sequence in which they are performed. For example, roll-pitch-yaw gives a different orientation than pitch-roll-yaw.   

Euler Angles and Rotation
-------------------------------------------

The Euler angles are three angles introduced by Leonhard Euler to describe the orientation of an object with respect to a fixed coordinate system. The order in which you specify a change in orientation is *Yaw, Pitch, Roll*.  One way to remember this is to think of plane about to take off from an airport.  While it’s on the ground it can only change its heading, which is done by a Yaw rotation.  At take-off the pilot will pull up the nose, changing its Pitch.  Finally, once the plane is in the air it’s free to execute a Roll.  Moving from one orientation to another is called a *Rotation*.  

A robot that can move in all three dimensions of space and can orient itself about all three axis would have 6-degrees-of-freedom (6-DOF).  An object in space can have at most only 6-degrees-of-freedom.  An example of such a robot would be a drone or aircraft. 

.. image:: /images/RobotConcepts/FRCGeometry.002.jpeg 

Each Yaw, Pitch, Roll maneuver can be represented by a rotation matrix that uses sines and cosines to describe the offset from the previous orientation.

References
****************************************

* FRC Documentation `Geometry <https://docs.wpilib.org/en/latest/docs/software/advanced-controls/geometry/index.html>`_ 

* QUT Robot Academy `2D Geometry <https://robotacademy.net.au/masterclass/2d-geometry/>`_ 

* QUT Robot Academy `3D Geometry <https://robotacademy.net.au/masterclass/3d-geometry/>`_

* FRC Documentation  `A Video Walkthrough of Model Based Validation of Autonomous in FRC <https://docs.wpilib.org/en/stable/docs/software/advanced-controls/video-walkthrough.html>`_ 

* Angela Sodemann `Rotation Matrix Video <https://www.youtube.com/watch?v=lVjFhNv2N8o>`_ 
