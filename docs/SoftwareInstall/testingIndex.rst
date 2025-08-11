.. Heading Key 
   ####### - H1 
   ******* - H2 
   ======= - H3 
   ------- - H4 
   ^^^^^^^ - H5 
   """"""" - H6 

Testing Introduction
###################################

Once you have written some code and you are ready to test, there are several platforms and software tools on which to test.  

- **Robot Simulator** 
    The Robot Simulator allows you to test your code without a physical robot. Simulators are used extensively in the robotics industry to work out issues before deploying code that could severely damage expensive robots.  They are also useful for detecting code bugs, since an issue such as a divide-by-zero that will crash the code on a robot will also crash the code in the simulator. Detailed information about the `Robot Simulator <https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-simulation/introduction.html>`_ can be found in the FRC documentation.

- **Sweet Bench** 
    The Sweet Bench is an FRC robot in a box. It has a RoboRio, FRC Radio and many of the motors we use in competitions. All on a benchtop chassis that does not move. It allows you to test your code using real motors, and try out new ideas and practice your programming skills without causing too much damage.

- **Programming Test Robot** 
    Once you have your code working in the simulator and on the sweet bench, you can start *porting* it onto the programming team practice robot. (The Practice robot is the first build of the competition robot-built very quickly so it may not have all the features and finishes of the competition robot, but it allows the programming team to verify thier code will work on the competition robot.) In software the word *Porting* generally means to adapt code to a different hardware platform.  Code that worked in simulation may need changes to work on the practice robot. (Such as different motor ID's and motor tuning such as PID's.) However, the structure and logic of your code will remain the same we just need to be sure that it is connected to the right hardware.

- **Shuffleboard**  
    When you are running code on a robot it's very useful to view how data is passing through the system as it operates.  This is referred to as *Telemetry*.  Telemetry is the science of using automatic equipment to make measurements and transmit them to a receiving station.  Telemetry is critical to fine tuning a system and figuring out what went wrong in the event of a crash. The main FRC tool for visualizing telemetry is `Shuffleboard <https://docs.wpilib.org/en/stable/docs/software/dashboards/shuffleboard/index.html>`_. It is important that you do not put every variable in your code on the Shuffleboard. Transmitting data to the receiving station takes time and in a competition time is critical, so it is very important that you only put data that you need to improve you programming on the Shunffleboard!

- **Phoenix Tuner** 
    `The Phoenix Tuner <https://docs.ctre-phoenix.com/en/stable/ch03_PrimerPhoenixSoft.html#what-is-phoenix-tuner>`_ is used to configure the motors supplied by *Cross The Road Electronics CTRE*.

.. image:: /images/FRCTools.021.jpeg  


 
.. - [Romi Testing Environment](../Romi/SC/romiDev.md)
.. - [Romi Software](romiPiSoftware.md)
.. - [Install RoboRIO & Radio Software](rioSoftware.md)
.. - [Install Third-Party Libraries](vendorSoftware.md)
