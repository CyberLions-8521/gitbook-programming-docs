# Controller Buttons and Servos

## Introduction

We will plug in a servo to the XRP, and run the servo when a button is pressed.  A servo can be thought of as a motor.  The non-continuous servos that the XRP robots come with can only spin up to half a circle (180 degrees).

* When a button on the controller is pressed, then the servo arm will go up
* When the button on the controller is released, then the servo arm will go down

## Code Setup

We will continue based off of the code from the [Using a Controller - Tank Drive](joysticks-and-tank-drive.md) project.

{% hint style="info" %}
In the code samples below, we'll hide the code from previous projects so that you can focus on the "new" code for the servo and if-statements that come from this project.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" expandable="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.xrp.XRPServo;
import edu.wpi.first.wpilibj.XboxController; // XboxController

public class Robot extends TimedRobot {
  
  // Creating our controller and motor objects
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  XRPServo m_servo = new XRPServo(4);

  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    m_servo.setPosition(0.5);
  }

  @Override
  public void teleopPeriodic() {
    // Getting the values from the joysticks    
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    
    // Setting the motor speeds using our joystick values
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
    
    // When testing code, only use one of the options below
    // Option 1 - single if-else statement
    if (m_controller.getAButton()) {
      m_servo.setPosition(1.0);
    } else {
      m_servo.setPosition(0.0);
    }
    
    // Option 2 - two separate if-statements
    if (m_controller.getAButtonPressed()) {
      m_servo.setPosition(1.0);
    }
    
    if (m_controller.getAButtonReleased()) {
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

## Importing Classes

Import the `XRPServo` class into your project so that you can use it in your code.

{% tabs %}
{% tab title="Java" %}
```java
import edu.wpi.first.wpilibj.xrp.XRPServo;
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

## Controller Buttons

Here are the steps we will take to control the servo:

1. <mark style="color:red;">**Create**</mark> the servo
2. <mark style="color:red;">**Initialize**</mark> the servo to a "starting" position
3. <mark style="color:red;">**If the A-button is pressed**</mark>, then lift the servo.  Otherwise, the servo goes back to its starting position.

### Creating a Servo

Create a variable for the `XRPServo` , and name it `m_servo`.  Use the constructor to set it to port 4 ([WPILib Getting to know your XRP](https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html)).

The constructor for the `XRPServo` is `XRPServo(#)`, where `#` denotes the port.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.xrp.XRPServo;   // XRPServo
// other imports not shown

public class Robot extends TimedRobot {
  XRPServo m_servo = new XRPServo(4);  // creating the servo
  // other code not shown
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Initializing the Servo

A servo position can be measured in either degrees (from 0 to 180 degrees) or as a "raw position" from 0 to 1.  Note that because of the ranges, a position of 90 degrees (or 0.5) is the middle position.

* `setAngle(#)` - Sets the servo to the given angle of `#`.  The input is a decimal number (`double`) between `0.0` and `180.0` degrees.
* `setPosition(#)` - Sets the servo to the given position of `#`, where `#` is a `double` between `0.0` and `1.0`.

We will initialize the servo to 90 degrees (or a raw position of 0.5).

{% tabs %}
{% tab title="Java" %}
{% code title="" lineNumbers="true" %}
```java
@Override
public void teleopInit() {
    m_servo.setPosition(0.5);    // initialize the servo position to the middle
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### If-Statements

There are three "kinds" of if-statements that you will generally see in FRC code.&#x20;

<table><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><pre><code>if (condition) {
    // execute code
}
</code></pre></td><td>The code within the braces is executed if the <code>condition</code> evaluates to <code>true</code>.  The code is skipped if the condition is <code>false</code>.</td></tr><tr><td><pre><code>if (condition) {
    // execution when true
} else {
    // execution when false
}
</code></pre></td><td>The code within the first set of braces is executed if the <code>condition</code> evaluates to <code>true</code>.  Otherwise, the code in the second set of braces is executed.</td></tr><tr><td><pre><code>if (conditionA) {
    // code A
} else if (conditionB) {
    // code B
} else {
    // code C
}
</code></pre></td><td>A combination of the above two cases.  The <code>conditionA</code> is checked first, and if it evaulates to <code>true</code>, then the code in the first set of braces is executed.<br><br>If <code>conditionA</code> evaluates to <code>false</code>, then <code>conditionB</code> is checked.  If it's true, then the code in <code>code B</code> is executed.<br><br>The last <code>else</code> is optional.  If the first two conditions fail, then the <code>else</code> is executed.<br><br>Only one set of braces can ever be executed.  Once code in a set of braces executes, the rest of the if-else-if block is skipped.</td></tr></tbody></table>

### Using the Servo

Depending on how you'd like to program the behavior of your robot, you can use any of the following functions of the `XBoxController`:

* `getAButton()` - Evaluates to `true` as long as the button is pressed.  Holding down the button makes this function continue to be `true`.
* `getAButtonPressed()` - Evaluates to `true` exactly one time when the button is initially pressed.  This function will not evaluate to `true` again until the button is pressed again.  In other words, holding down the button will not make this function continue to be `true`.
* `getAButtonReleased()` - Evaluates to `true` exactly one time when it detects that the button has been released from a pressed state.

{% hint style="info" %}
Feel free to use other buttons, and not just the A-button.  The function names are all the same, with "A" being replaced by B, X, Y, etc.\
\
For PlayStation controllers, you can use Square, Cross, Triangle, and Circle.  For example, `getCircleButtonPressed()`.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
// Other code is hidden

@Override
public void teleopPeriodic() {
    if (m_controller.getAButtonPressed()) {
        m_servo.setPosition(1.0);
    } else if (m_controller.getAButtonReleased()) {
        m_servo.setPosition(0.5);
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Simulating Robot Code

Connect to your XRP and simulate the code to test it.  The motors should be controlled by the joysticks, and the servo arm should "wave" when you press and release the A-button.

***

## Extension

The code above shows only one way to get the servo arm to wave.  Can you modify your code so that it uses two if-statements instead?  What about a single if-else statement?

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" expandable="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.xrp.XRPServo;
import edu.wpi.first.wpilibj.XboxController; // XboxController

public class Robot extends TimedRobot {
  
  // Creating our controller and motor objects
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  XRPServo m_servo = new XRPServo(4);

  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    m_servo.setPosition(0.5);
  }

  @Override
  public void teleopPeriodic() {
    // Getting the values from the joysticks    
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    
    // Setting the motor speeds using our joystick values
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
    
    // When testing code, only use one of the options below
    // Option 1 - single if-else statement
    if (m_controller.getAButton()) {
      m_servo.setPosition(1.0);
    } else {
      m_servo.setPosition(0.0);
    }
    
    // Option 2 - two separate if-statements
    if (m_controller.getAButtonPressed()) {
      m_servo.setPosition(1.0);
    }
    
    if (m_controller.getAButtonReleased()) {
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
