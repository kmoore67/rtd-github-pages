.. raw:: html 
   
   <meta name="robots" content="noindex">
   
###############################
Subsystems
###############################

Subsystems are the basic units of functionally for our robot, such as drive trains and mechanical arms.  They encapsulate low-level hardware objects (motor controllers, sensors, etc) and provide methods that can be used by Commands.  We represent subsystems in code by using `Objects </Programming/objects>`_. Objects are composed of `Data Structures </Programming/dataStructures>`_ and `Procedures </Programming/procedures>`_, as explained in `Introduction to Programming </Programming/programmingIndex>`_. You should learn those concepts before we look at the subsystems that are on the Robot.  

 For this module the majority of the code implementation can be found in the `Subsystems <https://docs.wpilib.org/en/latest/docs/software/commandbased/subsystems.html>`_ section of the FRC Documentation.  

.. image:: /images/RobotBasics/Subsystems.001.jpeg 

DriveTrain Subsystem
==============================

.. important:: TODO Define drive train & graphic 

The primary job of the *Drivetrain* subsystem is to send speed commands to its motors.  This is done in the `arcadeDrive()` method where we pass in the required translational and rotational speed.  The *DifferentialDrive* object will take care of controlling the speed to the left and right motors based on the kinematics of the drivetrain type. 

.. code-block:: Java 

    public void arcadeDrive(double xaxisSpeed, double zaxisRotate) {
        m_diffDrive.arcadeDrive(xaxisSpeed, zaxisRotate);
      }

Other procedures in the Drivetrain class will take care of resetting and reading the wheel encoders.  It'll also translate the wheel encoder values into distances, as explained in the Pose Estimation (../../Concepts/OptimalEstimation/poseEstimation.md) module.  Also checkout `Using the WPILib Classes to Drive your Robot <https://docs.wpilib.org/en/stable/docs/software/actuators/wpi-drive-classes.html?highlight=speedcontroller#multi-motor-differentialdrive-with-speedcontrollergroups>`_ for more information on implementing a drive train in FRC.

Gyro Subsystem
============================
The RomiGyro subsystem reads values from its gyro in order to perform [Pose Estimation](../../Concepts/OptimalEstimation/poseEstimation.md). The raw data that comes from gyros is very complex and difficult to intepret.  The RomiGyro subsystem translates the data into simple angles and rates-of-turn that are much easier to understand.

Lab - Subsystems
=============================
This lab builds on the lab that you did in the [Basic Robot Structure](romiStructure.md) section of the training guide.  You'll learn about the following Java programming concepts:

- `Java Objects </Programming/objects>`_ are programming constructs used to represent physical objects and ideas in the real world.

- `Java Constructors <https://www.w3schools.com/java/java_constructors.asp>`_ special methods that are used to initialize objects.

- `Java Methods <https://www.w3schools.com/java/java_methods.asp>`_, are declared within a class, and are used to perform certain actions.

- `Abstract Classes <https://www.w3schools.com/java/java_abstract.asp>`_ that show only essential things to the user and hides the internal details.  Abstraction lets you focus on what the object does instead of how it does it.

- `Interfaces <https://www.w3schools.com/java/java_interface.asp>`_, which are used to group together related methods that specify the behavior of a class.

There are two updates for this lab:

- Create a method to get the current heading of the robot.
- Add comments to divide the *Drivetrain* class into subsections.

Create Heading Method
==================================

In later programs we're going to need to get the current heading of our robot.  The heading is obtained from the *RomiGyro* class.  The *RomiGyro* is defined as a subsystem of the *Drivetrain* class.

Update the *Drivetrain* class with a new method called `getHeading()` that will be used to get the current heading of the robot.  You can place this method at around line `132` of the *Drivetrain* class just after the `getGyroAngleZ()` method.  The method will return a `Rotation2d()` value and be defined as `public`.  When you're done it should look like this:

.. code-block:: Java 

    public Rotation2d getHeading() {
        return new Rotation2d(getGyroAngleZ() * (Math.PI/180));
    }

We're now done creating our new heading method!

Use the `this.` Syntax for Member Variables
===============================================

In most of the code examples used by WPILib the member variables are denoted by using the `m_` syntax.  Our team uses the `this.` convention so we'll change our code accordingly.  This will give you a chance to learn the *Replace* function in the VSCode editor.

Open the *Drivetrain* class and select one of the occurences of `m_` with the mouse. Make sure that you only select those two characters. Then right mouse click and select *Change All Occurences*.  Type `this.` to replace all occurences of `m_`.  You'll see a whole bunch of errors.  This is because the `this.` keyword cannot be used when you first define the member variable.  To fix this, remove the `this.` from all the lines 20 through 36.

Repeat this process for all of the other files. 

Add Comments
===========================

As you add functionality to your code some of the files may get quite long, so it's a good idea to divide them into sections by using comment separators.  In the *Drivetrain* class we'll add the following comment separators to group together methods that are logically related.  This will make them easier to find.  The comment sections will be:

- **Initialization** - Includes everything required to construct and initialize the *Drivetrain* object.
- **Control Output** - Any method that causes the robot, or subsystem, to react in some way.
- **Sensor Input** - Methods that give information on the current state of the *Drivetrain*.
- **Process Logic** - Mostly logic that occurs during the `periodic()` loop, or processes/interprets incoming data before performing some kind of *Control Output* to the robot.  Incoming data will normally be provided by methods found in the *System State* subsection.

The comments will span most of the line width to form a separator and will look like this:

.. code-block:: Java 

    // -----------------------------------------------------------
    // System State
    // -----------------------------------------------------------

Think about where these file separators will go and what code will go in each section.  Here are a couple tips:

- If a method is returning a value, such as `double`, then it's most likely describing the Sensor Input.

- If a method return type is declared as `void` then it's probably some kind of Control Output.

That's all for this update!

References
=========================
- FRC Documentation - `Subsystems <https://docs.wpilib.org/en/latest/docs/software/commandbased/subsystems.html>`_ 
  
- FRC Documentation - `Differential Drive Robots <https://docs.wpilib.org/en/stable/docs/software/actuators/wpi-drive-classes.html>`_ 

- FRC Documentation - `Using the WPILib Classes to Drive your Robot <https://docs.wpilib.org/en/stable/docs/software/actuators/wpi-drive-classes.html?highlight=speedcontroller#multi-motor-differentialdrive-with-speedcontrollergroups>`_ 

- Java Tutorial on `W3Schools <https://www.w3schools.com/java/default.asp>`_ 

- QUT Robot Academy `Measuring Motion <https://robotacademy.net.au/masterclass/measuring-motion/>`_ 

