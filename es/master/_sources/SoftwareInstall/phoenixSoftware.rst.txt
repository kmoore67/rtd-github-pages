.. raw:: html 
   
   <meta name="robots" content="noindex">
   
##############################################   
Installing Phoenix API Libraries
##############################################

To install the Phoenix API libraries copy the link found on `Phoenix Releases <https://v6.docs.ctr-electronics.com/en/latest/docs/installation/installation-frc.html>`_ page.  You'll need to click on the **Online** tab to find the link.  Click on the WPI pallet and type **Manage Vendor Libraries** then click on the menu option that appears. Click **Install new libraries (online)** and a textbox should appear. Paste the link into the textbox.

Migrating to Phoenix 6
===================================

To migrate a project to Phoenix6 API follow these steps:

    * Remove the older phoenix libraries. Go to `vendordeps` folder and delete the `phoenix.json` file.

    * Install `CTRE Phoenix 6 <https://pro.docs.ctr-electronics.com/en/latest/docs/installation/installation.html>`_ libraries in VSCode. See above.  

    * Remove any red-underlined imports in your code.

    * Remove the `WPI_` from all of the class definitions.  Use the mouse over to `import` new classes.  Note that the second `c` in the *CANcoder* class is lowercase.

    * Update your code to apply the new syntax for configuring Phoenix hardware. See `Configuration <https://pro.docs.ctr-electronics.com/en/latest/docs/api-reference/api-usage/configuration.html>`_ in the Phoenix 6 documentation. 

    * Update your code to apply hardware control requests.  See the `Control Requests <https://pro.docs.ctr-electronics.com/en/latest/docs/api-reference/api-usage/control-requests.html>`_ documentation.  
    
Another resource is the `Phoenix 6 Examples <https://github.com/CrossTheRoadElec/Phoenix6-Examples/tree/main/java>`_ example code on GitHub.  Also see `Phoenix 6 Migration <https://pro.docs.ctr-electronics.com/en/latest/docs/migration/migration-guide/index.html>`_ in the Phoenix documentation.

In order to select data values (referred to as *Signal Values*) from the Phoenix hardware you need to import these classes:

.. code-block:: 

    import com.ctre.phoenix6.BaseStatusSignal;
    import com.ctre.phoenix6.StatusSignal;


Installing the Phoenix Tuner X
=======================================

Phoenix Tuner is the companion application allowing you to configure, analyze, update and control device. See `Phoenix 6 Tuner X <https://pro.docs.ctr-electronics.com/en/latest/docs/tuner/index.html>`_ for installing the tuner.


Connecting the Phoenix Tuner X
==================================

There are multiple ways to connect the tuner to the RoboRio.  If you have a USB cable connected it defaults to `172.22.11.2` which is the roboRIO IP when connected via USB.  You can also connect via the *DriverStation* as long as it's connected to the RoboRio.  If the RoboRio is newly imaged and there is no robot program running you'll need to bring up a *Temporary Diagnostic Server*, which is done from the Tuner.  


.. image:: /images/FRCRoboRIO/FRCroboRIO.012.jpeg


See the `Phoenix Tuner X Devices <https://pro.docs.ctr-electronics.com/en/latest/docs/tuner/device-list.html>`_ page for information on upgrading devices.  The Tuner will automatically download the latest firmware when it's started.  See `Firmware Upgrade <https://pro.docs.ctr-electronics.com/en/latest/docs/tuner/device-details-page.html#field-upgrade-firmware-version>`_.

If you have a *CANivore* you can upgrade Phoenix device firmware directly from a PC.


.. image:: /images/FRCRoboRIO/FRCroboRIO.014.jpeg 


References
=====================

- CTRE - `Create a Project <https://docs.ctre-phoenix.com/en/latest/ch05a_CppJava.html>`_ 
