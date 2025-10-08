.. raw:: html 
   
   <meta name="robots" content="noindex">
   
####################################
Slew Rate Filter
####################################

A common use for filters in FRC® is to soften the behavior of control inputs (for example, the joystick inputs from your driver controls). Unfortunately, a simple low-pass filter is poorly-suited for this job; while a low-pass filter will soften the response of an input stream to sudden changes, it will also wash out fine control detail and introduce phase lag. A better solution is to limit the rate-of-change of the control input directly. This is performed with a slew rate limiter - a filter that caps the maximum rate-of-change of the signal.

A slew rate limiter can be thought of as a sort of primitive motion profile. In fact, the slew rate limiter is the first-order equivalent of the Trapezoidal Motion Profile supported by WPILib - it is precisely the limiting case of trapezoidal motion when the acceleration constraint is allowed to tend to infinity. Accordingly, the slew rate limiter is a good choice for applying a de-facto motion profile to a stream of velocity setpoints (or voltages, which are usually approximately proportional to velocity). For input streams that control positions, it is usually better to use a proper trapezoidal profile.

You may have noticed that the movements of the robot are very sudden.  So much so that the wheels may even skid a little at the start of each motion.  In order to reduce that we can add a SlewRateLimiter filter.  Refer to the FRC `Slew Rate Limiter <https://docs.wpilib.org/en/latest/docs/software/advanced-controls/filters/slew-rate-limiter.html>`_ documentation to learn more about these filters.  In this lab we'll create a slew rate filter to give more control over the speed of the robot.

.. important:: This solution is for an arcade drivetrain and must be adapted for a swerve drivetrain.

You'll need a separate filter for the forward and backwards driving and for the turns.  These are defined as member variables in the *Drivetrain* class.  The rate parameter adjusts the speed.  You can increase this if you want the robot to go faster.

.. code-block:: Java 

        private final SlewRateLimiter m_filter = new SlewRateLimiter(0.5);
        private final SlewRateLimiter m_filter_turn = new SlewRateLimiter(0.5);

We don't want to use this filter unless we're very specific about it so create a new method called `rateLimitedArcadeDrive()` to use the filters

.. code-block:: Java 

    public void rateLimitedArcadeDrive(double xaxisSpeed, double zaxisRotate) {
        m_diffDrive.arcadeDrive(m_filter.calculate(xaxisSpeed), 
                                m_filter_turn.calculate(zaxisRotate));
    }

Update the *ArcadeDrive* command to use the new `rateLimitedArcadeDrive()` method of the Drivetrain.

.. code-block:: Java

    public void execute() {
        m_drivetrain.rateLimitedArcadeDrive(m_xaxisSpeedSupplier.get(), m_zaxisRotateSupplier.get());
    }
