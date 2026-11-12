# Controller Buttons and Servos

## Starter Code

We will use the starter code found here: [https://github.com/CyberLions-8521/xrp-making-hardware](https://github.com/CyberLions-8521/xrp-making-hardware)

If you have not already, then clone the starter code repository to your computer by following the steps in [Getting Starter Code](../../getting-started/getting-started/getting-starter-code.md).

## Overview

We will plug in a servo to the XRP, and run the servo when a button is pressed.  A servo can be thought of as a motor.  The non-continuous servos that the XRP robots come with can only spin up to half a circle (180 degrees).

* When a button on the controller is pressed, then the servo arm will go up
* When the button on the controller is released, then the servo arm will go down

## Code Setup

We will continue based off of the code from the [Joysticks and Driving](joysticks-and-driving.md) project.

{% hint style="info" %}
In the code samples below, we'll hide the code from previous projects so that you can focus on the "new" code for the servo and if-statements that come from this project.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" expandable="true" %}
```java
public class Robot extends TimedRobot {
  private final KeyboardController m_controller = new KeyboardController(0);
  private final XRPMotor m_leftMotor = new XRPMotor(0);
  private final XRPMotor m_rightMotor = new XRPMotor(1);
  
  double forwardSpeed;
  double turnSpeeed;

  public Robot() {
    m_rightMotor.setInverted(true);

  @Override
  public void teleopInit() {

  }

  @Override
  public void teleopPeriodic() {
    forwardSpeed = -m_controller.getWSAxis();
    turnSpeed = -m_controller.getADAxis();
    
    // Setting the motor speeds using our joystick values
    m_leftMotor.set(forwardSpeed - turnSpeed);
    m_rightMotor.set(forwardSpeed + turnSpeed);
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

## Programming the Servo

Here are the steps we will take to control the servo:

1. <mark style="color:red;">**Create**</mark> the servo
2. <mark style="color:red;">**Initialize**</mark> the servo to a "starting" position
3. <mark style="color:red;">**If the A-button is pressed**</mark>, then lift the servo.  Otherwise, the servo goes back to its starting position.

### Creating a Servo

Create a variable for the `XRPServo` , and name it `m_servo`.  Use the constructor to set it to port 4 ([WPILib Getting to know your XRP](https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html)).

<table><thead><tr><th width="237">Constructor</th><th>Description</th></tr></thead><tbody><tr><td><code>XRPServo(int deviceNum)</code></td><td>The constructor.  Creates a Servo with the given device number.  The input <code>deviceNum</code> must be an integer.  For the XRP, the two device numbers for servos are 4 and 5.</td></tr></tbody></table>

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
public class Robot extends TimedRobot {
  private final XRPServo m_servo = new XRPServo(4);  // creating the servo
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

We will initialize the servo to 90 degrees (or a raw position of 0.5).

<table><thead><tr><th width="277">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>setAngle(double angle)</code></td><td>Commands the servo to move to the given position in degrees.  The input angle must be a decimal number (<code>double</code>) between <code>0.0</code> and <code>180.0</code>.</td></tr><tr><td><code>setPosition(double position)</code></td><td>Commands the servo to move to the given position.  The input position must be a decimal number (<code>double</code>) between <code>0.0</code> and <code>1.0</code>.</td></tr></tbody></table>

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

Depending on how you'd like to program the behavior of your robot, you can use any of the following functions of the `KeyboardController`:

{% hint style="info" %}
Replace the "Z" with "X", "C", or "V" to use the other buttons for the `KeyboardController`.
{% endhint %}

<table><thead><tr><th width="207">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>getZButton()</code></td><td>Evaluates to <code>true</code> as long as the button is pressed.  Holding down the button makes this function continue to be <code>true</code>.</td></tr><tr><td><code>getZButtonPressed()</code></td><td>Evaluates to <code>true</code> exactly one time when the button is initially pressed.  This function will not evaluate to <code>true</code> again until the button is pressed again.  In other words, holding down the button will not make this function continue to be <code>true</code>.</td></tr><tr><td><code>getZButtonReleased()</code></td><td>Evaluates to <code>true</code> exactly one time when it detects that the button has been released from a pressed state.</td></tr></tbody></table>

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
// Other code is hidden

@Override
public void teleopPeriodic() {
    if (m_controller.getZButtonPressed()) {
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

Simulate the code to test it on the XRP (see the instructions in [Driving the XRP](../xrp-level-0-new-to-programming/driving-the-xrp.md) or in [Running Code on an XRP)](../running-code-on-an-xrp.md).

***

## Extension

* **Challenge A -** The code above shows only one way to get the servo arm to wave.  Can you modify your code so that it uses two if-statements instead?  What about a single if-else statement?
* **Challenge B -** Attach and program a second servo to your robot.

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" expandable="true" %}
```java
// Sample for Challenge A

package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;

import frc.robot.util.KeyboardController;
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
    
    // Setting the motor speeds using our joystick values
    m_leftMotor.set(fwd - turn);
    m_rightMotor.set(fwd + turn);
    
    // When testing code, only use one of the options below
    // Option 1 - single if-else statement
    if (m_controller.getZButton()) {
      m_servo.setPosition(1.0);
    } else {
      m_servo.setPosition(0.0);
    }
    
    // Option 2 - two separate if-statements
    if (m_controller.getZButtonPressed()) {
      m_servo.setPosition(1.0);
    }
    
    if (m_controller.getZButtonReleased()) {
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
