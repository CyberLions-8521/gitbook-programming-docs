# XRP Timed Robot Drive Program

## Tank Drive

### Introduction

This page will walk you through programming the XRP as a Timed Robot using the Tank Drive operator control scheme.  This means that the left joystick of a gamepad will control the left wheel on the robot, and the right joystick of the gamepad will control the right wheel on the robot.

Note that using Arcade Drive as a control scheme takes only slight modification to the code.  Sample code for Arcade Drive is included at the bottom of this page.

### Code Setup

You should have already created and set up a WPILib project for your XRP robot.  If you have not already done so, follow the instructions in [Simple XRP Program Project Setup](../xrp-project-setup.md), and choose Timed Skeleton (Advanced) as your project template.

To understand the structure of a timed robot, be sure to read [Timed Robots](../../../frc-programming-theory/timed-robots.md).

In the left-hand file explorer sidebar of VSCode, open up the file named `Robot.java` in the `java/frc/robot` folder located within the `src/main` folder.

For this project, we will focus mainly on the Teleoperated mode, and so all other modes can be ignored, commented out, or deleted.

{% tabs %}
{% tab title="Java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;

public class Robot extends TimedRobot {
  @Override
  public void teleopInit() {}

  @Override
  public void teleopPeriodic() {}
}
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Importing Classes

The XRP robot uses a special `XRPMotor` class to interface with the motors on the XRP.  In order to use it in your program, you must have first downloaded the XRP 3rd Party Vendor Library, and include the following import statement at the top of your program, right underneath the Timed Robot import/include statement:

{% tabs %}
{% tab title="Java" %}
```java
import edu.wpi.first.wpilibj.xrp.XRPMotor;
```
{% endtab %}

{% tab title="C++ (Header)" %}
```cpp
#include <frc/xrp/XRPMotor.h>
```
{% endtab %}
{% endtabs %}

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

Here is a sample of what your code might look like at this point.

{% tabs %}
{% tab title="Java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController

public class Robot extends TimedRobot {
  @Override
  public void teleopInit() {}

  @Override
  public void teleopPeriodic() {}
}
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

### Creating Motor and Controller Objects

#### XBox Controller Methods

We will use the following constructors and methods for the Xbox Controller ([Java](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj/XboxController.html), [C++](https://github.wpilib.org/allwpilib/docs/release/cpp/classfrc\_1\_1\_xbox\_controller.html)).

<table><thead><tr><th width="296">Constructor/Method</th><th>Description</th></tr></thead><tbody><tr><td><code>XboxController(int port)</code></td><td>Constructs an Xbox Controller object in your code.  You must provide the port number you will be using the controller on.</td></tr><tr><td><code>double getLeftY()</code></td><td>Returns the current value of the left joystick in the y-direction (up/down).  Values are from -1 to 1.</td></tr><tr><td><code>double getRightY()</code></td><td>Returns the current value of the right joystick in the y-direction (up/down).  Values are from -1 to 1.</td></tr></tbody></table>

#### XRP Motor Methods

We will use the following constructors and methods for the XRP Motor ([Java](https://github.com/wpilibsuite/allwpilib/blob/main/xrpVendordep/src/main/java/edu/wpi/first/wpilibj/xrp/XRPMotor.java), [C++](https://github.com/wpilibsuite/allwpilib/blob/main/xrpVendordep/src/main/native/include/frc/xrp/XRPMotor.h)).

<table><thead><tr><th width="405">Constructor/Method</th><th>Description</th></tr></thead><tbody><tr><td><code>XRPMotor(int port)</code></td><td>Constructs an XRP Motor.  You must provide the port number the motor is plugged into.</td></tr><tr><td><code>void setInverted(boolean isInverted)</code></td><td>Sets the inversion state of the motor.  If the <code>isInverted</code> value is set to <code>true</code>, then positive speeds will result in the motor spinning counter clockwise (when viewing the motor with the wheel facing away from you).  Motors are not inverted by default.</td></tr><tr><td><code>void set(double speed)</code></td><td>Sets the speed of the motor.  Speeds can be any value between -1 and 1.</td></tr></tbody></table>

#### The Code

Create an XBox Controller object, give it a name (in the sample code, our XBox Controller object is named `m_controller`), and set it to port 0 (technically, you can set the controller to any other port if you wish).

Create two XRP Motor objects, one to represent the left motor, and one to represent the right motor.  The documentation provided by [WPILib for the XRP](https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html) states that the left motor is plugged into port 0, and the right motor is plugged into port 1.

{% tabs %}
{% tab title="Java" %}
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
  public void teleopInit() {}

  @Override
  public void teleopPeriodic() {}
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### TeleopInit

We want to invert the right motor so that when we apply positive output, the motor will still drive in the direction we consider to be forward.  Inver the right motor within the `teleopInit()` method, since we only need to invert the motor once, and at the beginning before our loop code starts.

{% hint style="info" %}
Alternatively, you can invert the motor within the `robotInit()` method, since we only need to invert the motor once, and `robotInit()` will run _exactly_ once when the robot turns on; the `teleopInit()` method does occur once, but it occurs once for every time that the robot switches to teleop mode.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
```java
@Override
public void teleopInit() {
  m_rightMotor.setInverted(true);
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### TeleopPeriodic

In tank drive, the left joystick will drive the left wheels, and the right joystick will drive the right wheels.  Within our periodic (or looping) code, we want to get the values of the joysticks from the controller, and use those values as the speeds to set our motors to.  Note that we need a minus sign when getting the Y-joystick values since pushing up on a joystick returns a negative value.  The minus sign will convert the negative values to positive ones when pushing the joystick up (and vice versa when pulling the joystick down).

{% hint style="info" %}
See [WPILib Coordinate System Documentation](https://docs.wpilib.org/en/stable/docs/software/basic-programming/coordinate-system.html) for additional explanation about FRC coordinate system conventions.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
```java
@Override
public void teleopPeriodic() {
    // Getting the values from the joysticks    
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    
    // Setting the motor speeds using our joystick values
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Tank Drive Sample Code

By now, your code might look like this.  You can now use WPILib to simulate your code on the robot.

{% tabs %}
{% tab title="Java" %}
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
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

{% hint style="info" %}
See [How to Simulate Code](../../../getting-started/wpilib-vscode-how-to/wpilib-simulate-code.md).
{% endhint %}

***

## Arcade Drive

This sample code shows how you could modify the program to drive the robot using Arcade Drive.  In this example, the forwards/backwards motion is controlled by moving the left joystick up and down, and the turning movement is controlled by moving the left joystick left and right.

{% tabs %}
{% tab title="Java" %}
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
    double forward = -m_controller.getLeftY();
    double turn = -m_controller.getLeftX();
    
    // Setting the motor speeds using our joystick values
    m_leftMotor.set(forward - turn);
    m_rightMotor.set(forward + turn);
  }
}
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}
