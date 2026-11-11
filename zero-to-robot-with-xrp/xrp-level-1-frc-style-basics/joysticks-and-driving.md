# Joysticks and Driving

## Starter Code

We will use the starter code found here: [https://github.com/CyberLions-8521/xrp-making-hardware](https://github.com/CyberLions-8521/xrp-making-hardware)

If you have not already, then clone the starter code repository to your computer by following the steps in [Getting Starter Code](../../getting-started/getting-started/getting-starter-code.md).

## Overview

This page will walk you through programming the XRP and driving it using a controller.

* <mark style="color:red;">**Pushing up**</mark> on a joystick makes the robot <mark style="color:red;">**drive forward**</mark>
* <mark style="color:red;">**Pulling back**</mark> on a joystick makes the robot <mark style="color:red;">**drive in reverse**</mark>

## Code Setup

We will continue based off of the code from the [Make the Motors Spin](make-the-motors-spin.md) project.

{% hint style="info" %}
To make it easier to see the new lines of code that you should be typing, the entire code file is not necessarily included in each code block.

Any "missing" code can be assumed to be unchanged.
{% endhint %}

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

## Driving with a Controller

Here are the steps we will take to drive our robot with a controller:

1. <mark style="color:red;">**Create**</mark> the controller
2. <mark style="color:red;">**Get the joystick value**</mark> of the controller
3. <mark style="color:red;">**Use the joystick value**</mark> to set the speed of the motor

### Creating a Controller

For these tutorials, we have created a custom `KeyboardController` class.

1. We create the controller hardware and name it `m_controller`.
2. We will "plug it in" to joystick port 0.

<table><thead><tr><th width="272">Constructor</th><th>Description</th></tr></thead><tbody><tr><td><code>KeyboardController(int port)</code></td><td>The constructor.  Creates a keyboard controller that uses WASD controls (and ZXCV as buttons).<br><br>The input <code>port</code> must be a whole number between 0 and 5.<br><br>Only a single keyboard controller can be created.</td></tr></tbody></table>

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
public class Robot extends TimedRobot {
  private final KeyboardController m_controller = new KeyboardController(0);

}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Getting the Joystick Value

We use the dot operator to tell the controller to use the `getWSAxis()` function in order to get the value of the joystick that uses the W and S keys.

* If the joystick is pushed fully up (W), then the value is `-1.0`&#x20;
* If the joystick is pushed fully down (S), then the value is `1.0`
* When we let go of the buttons, then the "joystick" goes back to the "center" and the value is `0.0`&#x20;

Similar to the [Driving with Joysticks](../xrp-level-0-new-to-programming/driving-with-joysticks.md) tutorial, we will store the joystick value in a variable of type `double` within the `teleopPeriodic` function, so that the robot code is constantly grabbing the joystick value.

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" %}
```java
public class Robot extends TimedRobot {
  
  private double forwardSpeed;

  @Override
  public void teleopPeriodic() {
    forwardSpeed = -m_controller.getWSAxis();
    m_leftMotor.set(0.75);
    m_rightMotor.set(0.75);
  }
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
public class Robot extends TimedRobot {
  
  private double forwardSpeed;

  @Override
  public void teleopPeriodic() {
    forwardSpeed = -m_controller.getWSAxis();
    m_leftMotor.set(forwardSpeed);
    m_rightMotor.set(forwardSpeed);
  }
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Simulating Robot Code

Simulate the code to test it on the XRP (see the instructions in [Driving the XRP](../xrp-level-0-new-to-programming/driving-the-xrp.md) or in [Running Code on an XRP)](../running-code-on-an-xrp.md).

***

## Extension

Modify the code so that the robot drives using an Arcade Drive style.

* The "forward effort" is obtained using the WS Axis
* The "turning effort" is obtained using the AD Axis
* Create variables to store the values of the joysticks
* Don't forget to negate the axis values (use a negative sign) due to [joystick math weirdness](https://docs.wpilib.org/en/stable/docs/software/basic-programming/coordinate-system.html#joystick-and-controller-coordinate-system)
* The total speed given to the left motor is mathematically `forward - turn`
* The total speed given to the right motor is mathematically `forward + turn`

<table><thead><tr><th width="130">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>getWSAxis()</code></td><td>Gets the value of the joystick represented by the W and S keys.  The joystick achieves values between <code>-1.0</code> and <code>1.0</code>.  Pushing "up" on the joystick (W) results in a negative value.  The joystick resets its value back to <code>0.0</code> when you let go of the buttons.</td></tr><tr><td><code>getADAxis()</code></td><td>Gets the value of the joystick represented by the A and D keys.  The joystick achieves values between <code>-1.0</code> and <code>1.0</code>.  Pushing "left" on the joystick (A) results in a negative value.  The joystick resets its value back to <code>0.0</code> when you let go of the buttons.</td></tr><tr><td><code>getERAxis()</code></td><td>Gets the value of the joystick represented by the E and R keys.  The joystick achieves values between <code>-1.0</code> and <code>1.0</code>.  Pressing R results in a positive value.  This joystick retains its value, and does not automatically reset to <code>0.0</code> when you let go of the buttons.</td></tr></tbody></table>

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
