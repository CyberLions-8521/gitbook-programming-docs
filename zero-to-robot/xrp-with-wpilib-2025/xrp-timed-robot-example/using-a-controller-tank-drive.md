# Using a Controller - Tank Drive

## Introduction

This page will walk you through programming the XRP as a Timed Robot using the Tank Drive operator control scheme.

* The <mark style="color:red;">**left joystick**</mark> of a controller controls the <mark style="color:red;">**left motor**</mark>
* The <mark style="color:red;">**right joystick**</mark> of a controller controls the <mark style="color:red;">**right motor**</mark>
* <mark style="color:red;">**Pushing up**</mark> on a joystick makes the motor <mark style="color:red;">**spin forward**</mark>
* <mark style="color:red;">**Pulling back**</mark> on a joystick makes the motor <mark style="color:red;">**spin in reverse**</mark>

## Code Setup

We will continue based off of the code from the [Make the Motors Spin](variables-make-the-motors-spin.md) project.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;

public class Robot extends TimedRobot {
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  
  
  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
  }

  @Override
  public void teleopPeriodic() {
    m_leftMotor.set(0.5);
    m_rightMotor.set(0.5);
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="C++" %}

{% endtab %}
{% endtabs %}

## Importing Classes

We will also be using a gamepad or controller to control our robot.  Import the controller you will be using into your program (either a PS4 Controller, or an XBox Controller).  This example will be using an XBox Controller, but a PS4 Controller can be used with minimal changes to the code.

{% tabs %}
{% tab title="Java" %}
```java
// You only need to include the one you use
import edu.wpi.first.wpilibj.PS4Controller;
import edu.wpi.first.wpilibj.XboxController;
```
{% endtab %}

{% tab title="C++ (Header)" %}
```cpp
// You only need to include the one you use
#include <frc/PS4Controller.h>
#include <frc/XboxController.h>
```
{% endtab %}
{% endtabs %}

## Driving with a Controller

Here are the steps we will take to drive our robot with a controller:

1. <mark style="color:red;">**Create**</mark> the controller
2. <mark style="color:red;">**Get the joystick value**</mark> of the controller
3. <mark style="color:red;">**Use the joystick value**</mark> to set the speed of the motor

### Creating a Controller

Create an XBox Controller variable, give it a name (in the sample code, our XBox Controller variable is named `m_controller`), and set it to port 0.  The controller port is simply where you want the controller plugged in as detected by the FRC Driver Station.  Controller ports are 0 - 5.

The constructor for the XBox Controller is `XBoxController(#)`, where `#` denotes the Driver Station port.

{% tabs %}
{% tab title="Java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController

public class Robot extends TimedRobot {
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);

  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
  }

  @Override
  public void teleopPeriodic() {
    m_leftMotor.set(0.5);
    m_rightMotor.set(0.5);
  }
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Getting the Joystick Value

We use the dot operator to tell the controller to use the `getLeftY` function in order to get the value of the left joystick.  The "Y" denotes the "y-axis", and will give us a number between `-1.0` and `1.0` that tells us how much the stick is being pushed or pulled up or down.

* If the joystick is pushed fully up, then the value is `-1.0`&#x20;
* If the joystick is pushed fully down, then the value is `1.0`
* If the joystick is in the center, then the value is `0.0`&#x20;

We will store the joystick value in a variable of type `double` within the `teleopPeriodic` function, so that the robot code is constantly grabbing the joystick value.

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" %}
```java
@Override
public void teleopPeriodic() {
  // Getting the value of the left stick, and storing it in a variable
  // Notice the negative sign, since fully up is -1.0, but we want
  // fully up to cause the robot to go forward and -(-1.0) = 1.0
  double leftSpeed = -m_controller.getLeftY();
  
  m_leftMotor.set(0.5);
  m_rightMotor.set(0.5);
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Setting the Motor Speed

The last thing we need to do is set the motor speed to the joystick value.  Since joystick values are between `-1.0` to `1.0`, we can simply give the stick value to the motor to tell it to run.

{% tabs %}
{% tab title="Java" %}
```java
@Override
public void teleopPeriodic() {
  double leftSpeed = -m_controller.getLeftY();
  
  m_leftMotor.set(leftSpeed);  // setting the speed of the motor
  m_rightMotor.set(0.5);
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Simulating Robot Code

Connect to your XRP and simulate the code to test it.  Right now, the left motor should be controlled by the left joystick of your controller, with up on the joystick making the left motor go forwards.

As it stands, the right motor is just running forever, since we have it set to run at 50% speed forever.

***

## Extension

Modify the code so that the right motor is being controlled by the right joystick on the controller.  You can grab the value of the up and down movement of the right joystick by using the `getRightY` function and by using the dot operator on your `m_controller` variable.

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
