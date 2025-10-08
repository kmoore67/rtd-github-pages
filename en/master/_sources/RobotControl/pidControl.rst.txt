.. raw:: html 
   
   <meta name="robots" content="noindex">
   
############################
PID Control
############################

Types of PID Control
*************************************** 

There are two distinct types of PID control strategies, these are:

* Position PID Control 
* Velocity PID Control 

*Position PID Control* is used when you are trying to get a mechanism to a specific location. Such as "move the elevator 10 inches", or "move the robot 10 meters", or "move the shooter to 15 degrees". All of these have a goal position that we want the mechanism to hit. Our setpoint would be the goal we want to hit and our measurement would be a distance. 

*Velocity PID Control* is used when you want a mechanism to move at a target speed. Such as "run the shooter at 2,000 rpm" or "expel the intake at 200 rps". Both of these examples have a goal speed that we want the mechanism to hit. Our setpoint would be the speed we want to hit and our measurement would be the rate at which the mechanism is moving. 

.. important:: The setpoint units and the measurement units MUST be of the same unit type. For example, *Position PIDS* might be inches or meters. For *Velociy PIDS* this might be revolutions per second or rotations per second. The key point is that the developer must take great care that the setpoint units and the meansurement units match.   


Types of PID Control Implementation
************************************** 

There are two distinct types of PID Control implementations. The first implementation is WPILIB. A PID Controller has been implemented in WPILIB which means that the PID loop runs on the Roborio. And as explained previously, the loop will run every 20ms (50 times per second). This seems fast, but an even faster implementation is to run the PID controller on the motor controller. Both CTRE and REV have a PID control process that runs on the motor controller. You pass in the PID values and the setpoint, then turn it loose. The really great part is that it is running at 1ms - or 1,000 times per second! In addition, the Roborio can be running other things rather than a PID loop. 

.. Note:: Based on the performance and Roborio cpu savings our team prefers to run the PID loop on the motor controller. So our training will focus on using this method. However, there are circumstances when you must use the PID controller on the Roborio (such as multiple concurrent PID Loops) so it is important for you to know how to do both.     


Lab - Example Code
******************************

To help illustrate how to use a PID controller in our robot code it is best to use an example. You should download **ProgTrain3** from the teams github respository. The subsequent section will reference this code. 


Position PID Control
***********************************

Let's take a look at the **Turret** subsystem. This is a copy of the **Intake** subsystem that we have used in the previous sessions. Let's highlight the changes to the base **Intake** subsystem. 

The goal is to define 2 buttons on the driver controller. One will move the turret to 45 degrees to the left of center and the other will move the turret to 45 degrees to the right of center. As we want to move to a specific position on the turret, a Position PID Control is just what we need.

TurretConstants
-----------------------

We will start with **TurrentConstants**. The first thing to do is define the PID values for the motor. We will include both Voltage and Torque based PID values so that you can run the code in simulation and see the difference for yourself.   

.. code-block:: Java 

   ).withSlot0(
            new Slot0Configs()            /* Voltage-based position */
                .withKP(2.4)        // An error of 1 rotation results in 2.4v output
                .withKI(0.0)        // No output for integrated error
                .withKD(0.1)        // A velocity of 1 rps results in 0.1v output 
        ).withSlot1(
            new Slot1Configs()            /* Torque-based position  */
                .withKP(60.0)       // An error of 1 rotation results in 60 A output
                .withKI(0.0)        // No output for integrated error     
                .withKD(6.0)        // A velocity of 1 rps results in 6 A output 
            );

Turret Subsystem 
-------------------------------

In the **Turret Subsystem**, we define a new state for moving the turret, let's call  
it *GOTOANGLE*:

.. code-block:: Java 

   /* Define the Valid States for the Turret Subsystem  */
   public enum State {
      IDLE, 
      GOTOANGLE, 
      CHARACTERIZE, 
   }

Now, we define the left and right angles to move the turret. We want 45 degrees but the units for the motor are in **rotations**. So we determine the 45 degrees is equal to 20 motor rotations. Accordingly, we define a left angle of -20 rotations and a right angle of 20 rotations. We will assume that position zero will be straight forward.  

.. code-block:: Java 

   private double leftAngle = -20.0;
   private double rightAngle = 20.0;  

.. note:: In this example, we used 20 rotations as moving 45 degrees. A better solution would be to create a function that converts degrees into rotations and vice versa. 

.. note:: Assuming anything on a robot is quite dangerous. To keep things simple, we have omitted the code that would read the turret current position at robot start-up from the absolute encoder within the motor.     

Next, we add a routine to handle the **GOTOANGLE** state. We will need to call the **TurretKraken** class to run to a specific position, passing the number of rotations we want: 

.. code-block:: Java 

   switch (currentState) {

      case IDLE -> {
        // m_TurretKraken.stop();
      }

      case GOTOANGLE -> {
        //move turret to requested position 
        m_TurretKraken.runPosition(requestedRotations);
        turretGoal.setDouble(requestedRotations); 
        flipState(State.IDLE,0.0);
       
      }

      case CHARACTERIZE -> {}

    } // End of Switch 

We will also modify our **flip-state** function to pass in the requested rotation from the *command*.  

.. note:: This is an example of how to pass data from a command into a function. 

.. code-block:: Java 

   private void flipState(State inState, double reqRotations ) {
      System.out.println("Setting state...." + inState.name() + " Rotations-"+ reqRotations);
      requestedRotations = reqRotations;
      currentState = inState; 
  }

Next, we need to define some commands to go to the right or left angles. 

.. code-block:: Java 

    public Command GotoLeftAngleCommand() {
      // Subsystem::RunOnce implicitly requires `this` subsystem.
      return runOnce(
        () -> {
          /* one-time action goes here */
          flipState(State.GOTOANGLE,leftAngle);
      });
   }
   
   public Command GotoRightAngleCommand() {
      // Subsystem::RunOnce implicitly requires `this` subsystem.
      return runOnce(
        () -> {
          /* one-time action goes here */
          flipState(State.GOTOANGLE,rightAngle);
      });
   }

TurretKraken 
------------------------

Ok, now we move onto the changes for the **TurretKraken** class. First, let's define our position control commands: 

.. code-block:: Java 

   private final PositionVoltage m_positionVoltage = new PositionVoltage(0).withSlot(0);
   private final PositionTorqueCurrentFOC m_positionTorque = new PositionTorqueCurrentFOC(0).withSlot(1);

And finally, we define our **runPosition** function. Notice, that the nummber of rotations to move is passed in from the **Turret** class and put into the CTRE Motor control statement: 

.. code-block:: Java 

   public void runPosition(double desiredRotations) {      
      if(!useTorque) {
         /* Use velocity voltage */
         m_talonFX.setControl(m_positionVoltage.withPosition(desiredRotations));
      } else {
         /* Use velocity torque */
         m_talonFX.setControl(m_positionTorque.withPosition(desiredRotations));
      }
   }

And that's it. You now have example code to run a motor to a specific number of rotations. 

.. important:: You should run this example in simulation. Look under the Shuffle board-Turret tab. When you press the "X" button, notice the position moving to -20 rotations. When you press the "Y" button, the position should be moving to 20 rotations.  


Velocity PID Control 
***********************************

Let's take a look at the **Shooter** subsystem. This is a copy of the **Intake** subsystem that we have used in the previous sessions. Let's highlight the changes to the base **Intake** subsystem. 

The goal is to define 1 button on the driver controller that will run the shooter up to a constant velocity of 2,000 revolution per minute. As we want to move the shooter wheel at a specific velocity, a Velocit PID Control is just what we need.

ShooterConstants
-----------------------

In Shooter Constants we define the speed that we want the shooter wheel to run at.In this case 2,000 revolutions per minute.  

.. code-block:: Java 

   public static double kShooterSpeed = 2000.0;

Next, we add the PID values for a Velocity PID. First we use a Volatage-base strategy and then we define PID values for a Torque-based strategy. please note that these values are different from the position PID values defined for the Intake subsystem. 

.. code-block:: Java 
   
   ).withSlot0(
      new Slot0Configs()     /* Voltage-based velocity requires a velocity feed forward to account for the back-emf of the motor */
         .withKP(0.11)       // An error of 1 rotation per second results in 0.11 V output
         .withKI(0.0)        // No output for integrated error
         .withKD(0.0)        // No output for error derivative
         .withKS(0.1)        // To account for friction, add 0.1 V of static feedforward
         .withKV(0.12)       // Kraken X60 is a 500 kV motor, 500 rpm per V = 8.333 rps per V, 1/8.33 = 0.12 volts / rotation per second
   ).withSlot1(
      new Slot1Configs()     /* Torque-based velocity does not require a velocity feed forward, as torque will accelerate the rotor up to the desired velocity by itself */
         .withKP(5.0)        // An error of 1 rotation per second results in 5 A output
         .withKI(0.0)        // No output for integrated error
         .withKD(0.0)        // No output for error derivative
         .withKS(2.5)        // To account for friction, add 2.5 A of static feedforward    
   )

Shooter Subsystem 
-----------------------

The Shooter subsystem is a copy of the Intake subsystem, but converted into velocity instead of position. We define a new state which is *Shoot* rather than *Intake*. 

.. code-block:: Java 

   /* Define the Valid States for the shooter Subsystem  */
   public enum State {
      IDLE, 
      SHOOT, 
      RUNNING, 
      CHARACTERIZE;
   }


We change our state processing code to call the *runVelocity* method in the *shootKraken* class based on our new state. We also pass in the shooter speed we want to run the shooter wheel at. We get this from the *shooterConstants* file we deined previously. 

.. code-block:: Java 

   switch (currentState) {

      case IDLE -> {
        mShooterKraken.stop();
      }

      case SHOOT -> {
        //run shooter motor at a constant speed
        mShooterKraken.runVelocity(ShooterConstants.kShooterSpeed, 0.0);
        flipState(State.RUNNING);
      }

      case RUNNING -> {}

    
      case CHARACTERIZE -> {}

    } // End of Switch 

Then we add our new Shoot Command to give the operators a button to press:

.. code-block:: Java 

   public Command shootCommand() {
      // Subsystem::RunOnce implicitly requires `this` subsystem.
      return runOnce(
         () -> {
            /* one-time action goes here */
            flipState(State.SHOOT);
      });
   }


ShooterKraken
-----------------------

The **ShooterKraken** is a copy of the **IntakeKraken**. But it is modified for the velocity control. First, we define our *Velocity* control commands. One for *VelocityVoltage* and one for *VelocityTorque*. 

.. code-block:: Java 

   //Control 
   private final VelocityVoltage m_velocityVoltage = new VelocityVoltage(0).withSlot(0);    /* Start at velocity 0, use slot 0 */
   private final VelocityTorqueCurrentFOC m_velocityTorque = new VelocityTorqueCurrentFOC(0).withSlot(1);  /* Start at velocity 0, use slot 1 */


Finally, we define our *runVelocity* method to run the appropriate motor command based on the controlMode variable.  

.. code-block:: Java

   /*********************
   * The runVelocity function can run (based on the controlMode variable) either:
   *     0-means run Velocity Voltage Control Mode  
   *     1-means run Velocity Torque Control Mode  
   * 
   **********************/
   public void runVelocity(double RPM, double Feedforward) {      
      //Convert rotations per minute into rotations per second 
      double desiredRotationsPerSecond = (RPM/60.0); 

      switch (controlMode) {
         case 0:      /* Use velocity voltage */
            m_talonFX.setControl(m_velocityVoltage.withVelocity(desiredRotationsPerSecond));
         case 1:      /* Use velocity torque */
            m_talonFX.setControl(m_velocityTorque.withVelocity(desiredRotationsPerSecond));
      }
   }

That's it! These are all the changes necessary to run the shooter wheel in velocity control mode at a defined speed. 


References
*********************************

- Video - `Everything You Need to Know About Control Theory <https://resourcium.org/journey/companion-resources-everything-you-need-know-about-control-theory>`_ by Brian Douglas.

- Video Resource - `Control Theory <https://engineeringmedia.com/videos>`_ by Brian Douglas.

- FRC Documentation - `PID Basics <https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/index.html>`_ 

- FRC Documentation - `PID Control through PIDSubsystems and PIDCommands <https://docs.wpilib.org/en/latest/docs/software/commandbased/pid-subsystems-commands.html#>`_ 

- FRC  Documentation - `PID Control in WPILib <(https://docs.wpilib.org/en/stable/docs/software/advanced-controls/controllers/pidcontroller.html>`_ 

- FRC Programming Done Right - `PID Control <https://frc-pdr.readthedocs.io/en/latest/control/pid_control.html>`_ 

- TexasRobots - `Motion Magic Video <https://www.youtube.com/watch?v=xlQW8vGJWEs&ab_channel=texasRobots>`_ YouTube
  