# Encoders and Autonomous Routines

## Introduction

We introduce one of the most common sensors used in FRC: the encoder.  The encoder is a sensor that keeps track of how many times something has spun - sort of like a "distance tracker".  We will use the encoder to make the robot drive for a certain distance before stopping.

We will run this as an autonomous routine, so that the robot executes the "drive for distance" behavior when the robot enters autonomous mode.

## Quadrature Encoders

Encoders are devices that measure how many times something has rotated.  It does this by measuring how many pulses (or ticks) have gone by as the motor is turning.  For example, the XRP encoders measure one rotation every 585 ticks; so once it has measured 585 ticks, then you know the motor shaft (and wheel) has made one full rotation.

We use can this to measure the distance a wheel has travelled for example.  For a slightly more in-depth discussion on quadrature encoders, see the link below.

{% embed url="https://kb.vex.com/hc/en-us/articles/360039512851-Using-the-V5-3-Wire-Optical-Shaft-Encoder" %}

## Code Setup

Your code needs to at least have motors and a controller.  We'll use code from the [Using a Controller - Tank Drive](../joysticks-and-tank-drive.md) project.

{% hint style="info" %}
If you're following along with the projects in order, and just came from the If-Statements - Controller Buttons and Servos project, then that is fine as well.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" expandable="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController

public class Robot extends TimedRobot {
  
  // Creating our controller and motor objects
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);

  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
  }

  @Override
  public void teleopPeriodic() {
    // Getting the values from the joysticks    
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    
    // Setting the motor speeds using our joystick values
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
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

## Importing the Encoder

Import the Encoder class into your program.

{% tabs %}
{% tab title="Java" %}
```java
import edu.wpi.first.wpilibj.Encoder;
```
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

* `Encoder(#A, #B)` - An encoder plugged into the channels given by `#A` and `#B`.  These must be integers.

{% hint style="info" %}
Note that it is fine in this case for both the servo and the encoder to be "plugged into port 4", because the XRP does this weird thing where servo ports are separate from the encoder ports.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.Encoder;
// other imports not shown

public class Robot extends TimedRobot {
  Encoder m_leftEncoder = new Encoder(4, 5);
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
* <mark style="color:red;">**Encoder count to inches conversion factor**</mark> - calculated using the circumference and encoder count per wheel revolution (its resolution)

Since we want to run this as an autonomous routine, make sure you add back or uncomment the `autonomousInit` and `autonomousPeriodic` functions.

{% hint style="info" %}
If you would like to understand **where** the mathematics came from, or **why** the code works, then make sure you read the section below on Calculating Distance per Pulse.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.Encoder;
// other imports not shown

public class Robot extends TimedRobot {
  Encoder m_leftEncoder = new Encoder(4, 5);  // channels A, B on ports 4 and 5
  
  // Using Java's built-in variable for the number pi
  double wheelDiameter = 2.3622;  // inches
  double wheelCircumference = wheelDiameter * Math.PI;  // C = pi * D
  
  double countsPerWheelRev = 585; // counts per one revolution of the wheel
  double convFactor = wheelCircumference / countsPerWheelRev;
  
  @Override
  public void autonomousInit() {
    // Resetting the encoder measurement to 0 on startup
    m_leftEncoder.reset();
    
    // Setting the conversion factor using the value we calculated
    m_leftEncoder.setDistancePerPulse(convFactor);
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

We want to set the distance per pulse of the encoder so that whenever we read the distance from the encoder, we get a meaningful answer back (because getting a reading of `687` ticks is hard to make meaning out of, but getting a reading of `8.715` inches traveled _is_ meaningful).  By setting the distance per pulse of each encoder, the encoder will convert encoder counts to its equivalent distance for us, so that we do not have to.

To find the distance per pulse, we first find the distance travelled when the wheel turns once.  This is given by the circumference of the wheel, since when the wheel turns once, it has travelled a length exactly equal to its circumference.  We then divide this by the number of pulses per revolution, to get a final value in units of distance per pulse.

The dimensional analysis to find the distance per pulse is given below.  Notice that the units of revolutions cancels out, and you obtain distance (from the circumference which is in units of length) per pulses, which is what we wanted.

$$
\text{distance per pulse} = \dfrac{\text{circumference}}{1\text{ revolution}}\cdot\dfrac{1\text{ revolution}}{\text{number of pulses}}
$$

### Driving for a Distance

We complete our project by having our robot drive straight for a certain distance.  We'll have the robot execute this maneuver whenever we press the X button on our XBox Controller (or the square button on a PlayStation controller).  We add this code to `teleopPeriodic()`.

You can get the distance measured by the encoder by using the dot operator, and telling the encoder to `getDistance()` .  The distance returned is affected by the conversion factor given in `setDistancePerPulse`.

{% hint style="info" %}
Note that when measuring the distance travelled, we can use the average of the distance between the two encoders for more accurate readings.  You can technically measure from only the left or the right encoder since the robot is only driving straight in this case.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
@Override
public void autonomousInit() {
  m_leftEncoder.reset();
  m_leftEncoder.setDistancePerPulse(convFactor);
}

@Override
public void autonomousPeriodic() {
  // Speed of the motors is arbitrary as long as the robot is driving forward
  if (m_leftEncoder.getDistance() < 5) {  // 5 inches
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
