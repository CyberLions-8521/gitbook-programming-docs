# Encoders and Autonomous Routines

## Starter Code

We will use the starter code found here: [https://github.com/CyberLions-8521/xrp-making-hardware](https://github.com/CyberLions-8521/xrp-making-hardware)

If you have not already, then clone the starter code repository to your computer by following the steps in [Getting Starter Code](../../../getting-started/getting-started/getting-starter-code.md).

## Overview

We introduce one of the most common sensors used in FRC: the encoder.  The encoder is a sensor that keeps track of how many times something has spun - sort of like a "distance tracker".  We will use the encoder to make the robot drive for a certain distance before stopping.

We will run this as an autonomous routine, so that the robot executes the "drive for distance" behavior when the robot enters autonomous mode.

## Quadrature Encoders

Encoders are devices that measure how many times something has rotated.  It does this by measuring how many pulses (or ticks) have gone by as the motor is turning.  For example, the XRP encoders measure one rotation every 585 ticks; so once it has measured 585 ticks, then you know the motor shaft (and wheel) has made one full rotation.

We use can this to measure the distance a wheel has travelled for example.  For a slightly more in-depth discussion on quadrature encoders, see the link below.

{% embed url="https://kb.vex.com/hc/en-us/articles/360039512851-Using-the-V5-3-Wire-Optical-Shaft-Encoder" %}

## Code Setup

Your code needs to at least have motors and a controller.  We will continue based off of the code from the [Joysticks and Driving](../joysticks-and-driving.md) project.

{% hint style="info" %}
If you're following along with the projects in order, and just came from the Controller Buttons and Servos project, then that is fine as well.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" expandable="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;

import frc.robot.util.KeyboardController;

import edu.wpi.first.wpilibj.Encoder;
import edu.wpi.first.wpilibj.xrp.*;

public class Robot extends TimedRobot {
  private final KeyboardController m_controller = new KeyboardController(0);
  private final XRPMotor m_leftMotor = new XRPMotor(0);
  private final XRPMotor m_rightMotor = new XRPMotor(1);
  private final XRPServo m_servo = new XRPServo(4);
  
  private double fwd;
  private double turn;

  public Robot() {
    m_rightMotor.setInverted(true);
  }    

  @Override
  public void teleopInit() {
    m_servo.setPosition(0.5);
  }

  @Override
  public void teleopPeriodic() {
    fwd = -m_controller.getWSAxis();
    turn = -m_controller.getADAxis();
    
    m_leftMotor.set(fwd - turn);
    m_rightMotor.set(fwd + turn);
    
    if (m_controller.getZButton()) {
      m_servo.setPosition(1.0);
    } else {
      m_servo.setPosition(0.0);
    }
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

## Driving for Distance

We will have the robot drive for a certain distance.  The "drive for distance" behavior will be triggered during the autonomous mode.  Here are the steps we will take:

1. <mark style="color:red;">**Create**</mark> the encoders.
2. <mark style="color:red;">**Configure**</mark> the encoders within `autonomousInit` to produce meaningful distance results.
3. When the <mark style="color:red;">**autonomous mode starts, reset**</mark> the encoders.
4. While the <mark style="color:red;">**autonomous mode is running, drive**</mark> the robot until it reaches a set distance.  Stop the robot after it has driven that far.

### Creating an Encoder

Create a variable for the left encoder.  The data type is `Encoder` and we will name it `m_leftEncoder`.  The left encoder is plugged into digital input/output (DIO) ports 4 and 5 for channels A and B respectively ([WPILib Getting to know your XRP](https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html)).

<table><thead><tr><th width="334">Constructor</th><th>Description</th></tr></thead><tbody><tr><td><code>Encoder(int channelA, int channelB)</code></td><td>The constructor.  Creates an encoder plugged into ports <code>channelA</code> and <code>channelB</code>, which are integers.  For the XRP, the left encoder is in ports 4 and 5, and the right encoder is in ports 6 and 7.</td></tr></tbody></table>

{% hint style="info" %}
Note that it is fine in this case for both the servo and the encoder to be "plugged into port 4", because the XRP does this weird thing where servo ports are separate from the encoder ports.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" %}
```java
public class Robot extends TimedRobot {
  private final Encoder m_leftEncoder = new Encoder(4, 5);
  // other code not shown
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Configuring the Encoder

We will configure the encoders within `teleopInit` to do two things:

* <mark style="color:red;">**Reset the encoders**</mark> to a measurement of 0 - `reset()`&#x20;
* <mark style="color:red;">**Set a conversion factor**</mark> on the encoder so that it converts encoder counts to inches - `setDistancePerPulse(#distance)`.

To set the conversion factor, we need to calculate the distance per pulse using dimensional analysis to get the correct units.  To make programming the mathematics easier, we will make variables for the following ([WPILib Getting to know your XRP](https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html)):

* <mark style="color:red;">**XRP wheel diameter**</mark> - 60 mm (2.3622 inches)
* <mark style="color:red;">**XRP wheel circumference**</mark> - calculated using the diameter
* <mark style="color:red;">**Encoder count per single wheel revolution**</mark> - 585 counts per revolution
* <mark style="color:red;">**Encoder count to inches conversion factor**</mark> - calculated using the circumference and encoder count per wheel revolution (its resolution).

<table><thead><tr><th width="369">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>setDistancePerPulse(double distPerPulse)</code></td><td>Sets the distance that should be measured every time the encoder measures a pulse (or tick).  For example, a <code>distPerPulse</code> of <code>0.5</code> (inches) would mean that the robot travels half an inch every time the encoder measures a pulse.</td></tr><tr><td><code>reset()</code></td><td>Resets the encoder distance to zero.</td></tr></tbody></table>

{% hint style="info" %}
If you would like to understand **where** the mathematics came from, or **why** the code works, then make sure you read the section below on Calculating Distance per Pulse.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
public class Robot extends TimedRobot {
  private final Encoder m_leftEncoder = new Encoder(4, 5);  // channels A, B on ports 4 and 5
  
  // Using Java's built-in variable for the number pi
  private final double wheelDiameter = 2.3622;  // inches
  private final double wheelCircumference = wheelDiameter * Math.PI;  // C = pi * D
  
  private final double countsPerWheelRev = 585; // counts per one revolution of the wheel
  private final double convFactor = wheelCircumference / countsPerWheelRev;
  
  public Robot() {
    // Configurations for hardware occur in Robot constructor
    // Setting the conversion factor using the value we calculated
    m_leftEncoder.setDistancePerPulse(convFactor);
  }
  
  @Override
  public void autonomousInit() {
    // Startup code for autonomous mode
    // Resetting the encoder measurement to 0 on startup
    m_leftEncoder.reset();
  }
  /** additional code not shown **/
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

### Calculating Distance Per Pulse

{% hint style="info" %}
Note that this section can be skipped if you do not care for the math.  Just update your code so that it matches the example above (as in, set the distance per pulse of each encoder).  Continuing reading on below if you want to understand the mathematics behind the code.\
\
Note that an encoder pulse, encoder count, encoder tick all mean the same thing.
{% endhint %}

We want to set the distance per pulse of the encoder so that whenever we read the distance from the encoder, we get a meaningful answer back (because getting a reading of `687` ticks is hard to make meaning out of, but getting a reading of `8.715` inches traveled _**is**_**&#x20;meaningful**).  By setting the distance per pulse of each encoder, the encoder will convert encoder counts to its equivalent distance for us, so that we do not have to.

To find the distance per pulse, we first find the distance travelled when the wheel turns once.  This is given by the circumference of the wheel, since when the wheel turns once, it has travelled a length exactly equal to its circumference.  We then divide this by the number of pulses per revolution, to get a final value in units of distance per pulse.

The dimensional analysis to find the distance per pulse is given below.  Notice that the units of revolutions cancels out, and you obtain distance (from the circumference which is in units of length) per pulses, which is what we wanted.

$$
\text{distance per pulse} = \dfrac{\text{circumference}}{1\text{ revolution}}\cdot\dfrac{1\text{ revolution}}{\text{number of pulses}}
$$

### Driving for a Distance

We complete our project by having our robot drive straight for a certain distance.  We'll have the robot execute this maneuver whenever we press the X button on our Keyboard Controller.  We add this code to `autonomousPeriodic()`.

You can get the distance measured by the encoder by using the dot operator, and telling the encoder to `getDistance()` .  The distance returned is affected by the conversion factor given in `setDistancePerPulse()`.

<table><thead><tr><th width="369">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>getDistance()</code></td><td>Gets the distance the robot has driven since the last time the encoder was reset.  This value is affected by <code>setDistancePerPulse()</code></td></tr></tbody></table>

{% hint style="info" %}
Note that when measuring the distance travelled, we can use the average of the distance between the two encoders for more accurate readings.  You can technically measure from only the left or the right encoder since the robot is only driving straight in this case.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
public Robot() {
  // Configuration code happens in Robot constructor
  m_leftEncoder.setDistancePerPulse(convFactor);
  m_rightMotor.setInverted(true);
}

@Override
public void autonomousInit() {
  // Startup code for autonomous mode
  m_leftEncoder.reset();
}

@Override
public void autonomousPeriodic() {
  // Speed of the motors is arbitrary as long as the robot is driving forward
  if (m_leftEncoder.getDistance() < 5) {  // inches - based on conversion factor
    m_leftMotor.set(0.5);
    m_rightMotor.set(0.5);
  } else {
    m_leftMotor.set(0.0);
    m_rightMotor.set(0.0);
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

### Simulate and Test Code

Connect to your XRP robot and simulate your code.  You can test your "drive for distance" behavior by switching to autonomous mode.
