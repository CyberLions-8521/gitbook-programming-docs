# Autonomous Routines

## Starter Code

We will use the starter code found here: [https://github.com/CyberLions-8521/xrp-intro-autonomous](https://github.com/CyberLions-8521/xrp-intro-autonomous)

If you have not already, then clone the starter code repository to your computer by following the steps in [Getting Starter Code](../../getting-started/getting-started/getting-starter-code.md).

## Overview

This tutorial will have us creating a simple autonomous routine that has the robot driving forward for a certain distance.

## The Code

The motors in the XRP have a sensor called an **encoder.**

An encoder keeps track of how many times the motor has turned, allowing us to keep track of (and measure) how far the robot has travelled.

<table><thead><tr><th width="217">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>resetEncoders()</code></td><td>Resets the distance measured by the encoders back to <code>0.0</code>.</td></tr><tr><td><code>getLeftDistanceInch()</code></td><td>Gets the distance traveled by the left wheel in inches.</td></tr><tr><td><code>getRightDistanceInch()</code></td><td>Gets the distance traveled by the right wheel in inches.</td></tr></tbody></table>

1. We will write our code within the `Robot.java` file.
2. We will create variables to keep track of how far we want the robot to travel, as well as the distance the robot has traveled so far.

{% hint style="info" %}
Making these variables is entirely optional.  We are making them so that our code is more readable, and to build good habits.
{% endhint %}

3. <pre class="language-java" data-title="Robot.java" data-line-numbers><code class="lang-java">double currentDist = 0.0;
   double travelDist = 6.0;

   @Override
   public void autonomousInit() {
       resetEncoders();
   }

   @Override
   public void autonomousPeriodic() {
       currentDist = getLeftDistanceInch();
       if (currentDist &#x3C; travelDist)
       {
           driveForward();
       }
       else
       {
           stop();
       }
   }

   @Override
   public void teleopInit() {

   }

   @Override
   public void teleopPeriodic() {

   }
   </code></pre>
4. The logic of the code is:
   1. At the beginning of the autonomous period (`init`), reset the encoder measurements back to 0.
   2. Get the current distance measured by the encoder.
   3. If the current distance traveled is less than the desired distance we want to travel, then drive forward.
   4. Otherwise, stop driving.
5. Copy the code below, and simulate the code to test it on the XRP (see the instructions in [Driving the XRP](driving-the-xrp.md) or in [Running Code on an XRP)](../running-code-on-an-xrp.md).

## More Accurate Measurements

We can get more accurate distance measurements by taking the average of the distance measured by both the left and right encoders.  To average the measurements, we add them together and divide by 2.

{% code title="Robot.java" lineNumbers="true" %}
```java
double currentDist = 0.0;
double travelDist = 6.0;

@Override
public void autonomousInit() {
    resetEncoders();
}

@Override
public void autonomousPeriodic() {
    currentDist = (getLeftDistanceInch() + getRightDistanceInch()) / 2;
    if (currentDist < travelDist)
    {
        driveForward();
    }
    else
    {
        stop();
    }
}

@Override
public void teleopInit() {

}

@Override
public void teleopPeriodic() {

}
```
{% endcode %}

## Extension

* **Modification 1** - Modify the code so that the robot drives for a different amount of distance before stopping.
* **Modification 2 -** Modify the code so that you can customize the speed that it drives.  In other words, use a different function than `driveForward()`.
* **Challenge 1 -** A gyroscope is a sensor that keeps track of the robot's heading, or direction that it is facing in degrees.  Program the robot so that instead of driving a certain distance, it turns until it is facing a certain heading.
  * The initial forwards heading is 0 degrees.
  * Counterclockwise turning increases the heading.  Clockwise turning decreases the heading, and gets into negative degree values.
  * Making multiple full revolutions measures angles beyond 360 degrees.
* **Challenge 2 -** An ultrasonic sensor measures the distance the XRP is from an object (or a wall) in front of it.  Write an autonomous routine that has the robot drive forward until it is within 4 inches of an object.

<table><thead><tr><th width="232">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>drive(fwd, turn)</code></td><td>The robot drives forward with the effort specified by <code>fwd</code> and turns with the effort specified by <code>turn</code>.  The range of allowed values for <code>fwd</code> and <code>turn</code> are between <code>-1.0</code> and <code>1.0</code>.</td></tr><tr><td><code>resetGyro()</code></td><td>Resets the gyroscope measurement to 0.  This effectively makes the current direction the robot is facing the new "forward".</td></tr><tr><td><code>getHeading()</code></td><td>Returns the current angle the robot is facing in degrees.  If we consider the robot to be facing initially "north", then an angle of 90 degrees means that the robot is facing "west"; a measurement of -90 degrees is "east".</td></tr><tr><td><code>getUltrasonicInches()</code></td><td>Returns the distance the XRP is from an object in front of it in inches.  Objects (or distances) that are farther than 157 inches are reported as 157 inches.</td></tr></tbody></table>

