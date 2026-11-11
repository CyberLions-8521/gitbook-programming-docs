# Make the Motors Spin

## Starter Code

We will use the starter code found here: [https://github.com/CyberLions-8521/xrp-making-hardware](https://github.com/CyberLions-8521/xrp-making-hardware)

If you have not already, then clone the starter code repository to your computer by following the steps in [Getting Starter Code](../../getting-started/getting-started/getting-starter-code.md).

## Overview

The starter code in the Level 0 section had the robot hardware and behaviors pre-coded so that we could focus on the robot logic.

Now, we will begin to learn how to create and represent the robot hardware in the code, and be able to use the hardware that we created to make the robot move.

In addition, we will go into more advanced topics such as making our own functions, and using feedback loops to have our robot "self-correct" its behavior.

## Making the Motor Spin

Here are the steps we will take to make our motor spin:

1. <mark style="color:red;">**Create**</mark> the motors (in code)
2. <mark style="color:red;">**Initialize**</mark> and / or configure the motors (if needed)
3. <mark style="color:red;">**Run**</mark> the motors

{% hint style="info" %}
Note that program comments in both C++ and Java are denoted by the double-slash `//` or live between `/*` and `*/`.  Program comments are not read by the robot, and are provided here to annotate any code shown to you.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
public class Robot extends TimedRobot {
  // Step 1 - Create motors here
  
  public Robot() {
    // Step 2 - Configure motors here if needed
    
  }
  
  @Override
  public void teleopInit() {

  }

  @Override
  public void teleopPeriodic() {
    // Step 3 - Run the motors
    
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="C++" %}

{% endtab %}
{% endtabs %}

### Creating the Motor

We will create a variable to represent the left XRP motor.  The left motor is recognized as device number 0.  See the WPILib documentation, [Getting to Know your XRP](https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html), for a full list of which device numbers correspond to which piece of XRP hardware.

To create hardware in our code, we use the word `new` combined with the **constructor.**  The **constructor** is a special function that creates a code representation of our hardware.

<table><thead><tr><th width="223">XRPMotor Constructor</th><th>Description</th></tr></thead><tbody><tr><td><code>XRPMotor(int deviceNum)</code></td><td>Creates an XRP motor in our code.  The input <code>deviceNum</code> must be an <code>int</code> value (whole number.<br><br>The left motor is device number 0; the right is 1.  Additional motors are device numbers 2 and 3.</td></tr></tbody></table>

1. The data type of the variable is `XRPMotor`
2. We will name the variable `m_leftMotor`
3. The value of the motor variable is - in a literal sense - "a motor plugged into port 0"

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
public class Robot extends TimedRobot {
  private final XRPMotor m_leftMotor = new XRPMotor(0);
  
}
```
{% endcode %}
{% endtab %}

{% tab title="C++" %}

{% endtab %}
{% endtabs %}

### Driving the Motor

To set the speed of the motor, we use the `set()` function.  For a full list of functions you can use with an `XRPMotor`, see the documentation here: [XRPMotor API (2026.2.2)](xrpmotor-api-2026.2.2.md)

<table><thead><tr><th width="182">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>set(double speed)</code></td><td>Sets the speed of the motor.  The input <code>speed</code> must be a decimal value (type <code>double</code>).<br><br>Speeds are between <code>-1.0</code> (full reverse) and <code>1.0</code> (full forward).</td></tr></tbody></table>

Feel free to experiment with different speed values.  Speed values run from `-1.0` to `1.0` , with negative speed values making the motor spin in the reverse direction.

{% tabs %}
{% tab title="Java" %}
```java
@Override
public void teleopPeriodic() {
    m_leftMotor.set(0.5);
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

{% hint style="info" %}
The general pattern for using functions from hardware is to write the variable name, followed by a dot (or period), followed by the function.

This is similar to telling a person to do something: you always start with their name, and then you tell them what to do.

Imagine you had a friend named Kyle that you wanted to ask to do some favors for you.  You would say things like:

* "Kyle, stand up"
* "Kyle, give me $5"
* "Kyle, stop that"

This could be represented in code as:

* `kyle.standUp()`
* `kyle.giveMoney(5)`
* `kyle.stop()`
{% endhint %}

### Simulate and Test Code

Connect to the XRP and [simulate your code](../running-code-on-an-xrp.md) to run it.

***

## Extension 1 - Creating the Right Motor

Follow a similar process and create a variable for the XRP motor on the right, and run it.  The right motor is plugged into port 1 on the XRP.

Expand the code below for a sample solution.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" expandable="true" %}
```java
// Expand code and scroll down to view sample solution
// Code is purposefully hidden













public class Robot extends TimedRobot {
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  
  public Robot() {
  
  }
  
  @Override
  public void teleopInit() {
  
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

***

## Extension 2 - Inverting the Right Motor

If you did Extension 1, you may have noticed that the right motor spins in the "wrong" direction when you apply speed values to it, and that running both motors "forwards" actually causes the robot to spin in place.

This is because when building the robot, the right motor is physically "flipped".  One way to make the motor behave more intuitively (i.e., telling the motor to go forward actually makes the robot go forward) is to **invert the motor.**

We can invert the right motor by using the dot operator to tell the motor to use the `setInverted` function, and setting the inversion to `true`.  Do this within either the Robot constructor `Robot()` or within `teleopInit`, since we want to invert the motor exactly once, either when the robot is "created" or at the start of the teleoperated mode.

<table><thead><tr><th width="295">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>set(double speed)</code></td><td>Sets the speed of the motor.  The input <code>speed</code> must be a decimal value (type <code>double</code>).<br><br>Speeds are between <code>-1.0</code> (full reverse) and <code>1.0</code> (full forward).</td></tr><tr><td><code>setInverted(boolean isInverted)</code></td><td>Sets the inversion state of the motor.  The input <code>isInverted</code> must be either <code>true</code> or <code>false</code>, with an inversion state of <code>true</code> meaning that the motor has flipped behavior.</td></tr></tbody></table>

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
public class Robot extends TimedRobot {
  private final XRPMotor m_leftMotor = new XRPMotor(0);
  private final XRPMotor m_rightMotor = new XRPMotor(1);
  
  public Robot() {
    m_rightMotor.setInverted(true);
  }
  
  @Override
  public void teleopInit() {

  }

  @Override
  public void teleopPeriodic() {
    m_leftMotor.set(0.75);
    m_rightMotor.set(0.75);
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="C++" %}

{% endtab %}
{% endtabs %}

## Extension 3

Redo some of the extensions from Level 0.

* Program the robot to drive in reverse, in a circle, or to turn in place (in either direction).
