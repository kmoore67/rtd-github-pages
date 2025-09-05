.. raw:: html 
   
   <meta name="robots" content="noindex">
   
#######################################
Install REV Robotics Software
#######################################

The install of the REV Robotics libraries is similar to that of the Phoenix software for Mac and Linux.  
- Download the zip file from the `REV Robotics Software <https://docs.revrobotics.com/sparkmax/software-resources/spark-max-api-information#c++-and-java>`_ releases page.
- Place the `maven/com/revrobotics` and `vendordeps/REVLib.json` folders inside the WPILib installation folder. This is typically located in `~/wpilib/2022`.
- Once installed you have to bring the libraries into your project.  This can be done from the WPI palette **Manage Vendor Libraries** option, as explained above.

.. Note:: You may have to uninstall and reinstall the libraries using **Manage Vendor Libraries** -> **Manage Current Libraries** in order to get IntelliSense to recognise them.