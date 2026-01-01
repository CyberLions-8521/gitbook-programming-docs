# Variables - Make the Motors Spin

## Introduction

Our goal with this first project is to simply make the motors spin, so that we can test the basic functionality of our XRP.

## Code Setup

You should have already created and set up a WPILib project for your XRP robot.  If you have not already done so, follow the instructions in [XRP Project Setup](../xrp-project-setup.md), and choose Timed Skeleton (Advanced) as your project template.

{% hint style="info" %}
To understand the structure of a timed robot, you can read the [Timed Robots](../../../frc-programming-theory/timed-robots.md) page.
{% endhint %}

In the left-hand file explorer sidebar of VSCode, open up the file named `Robot.java` in the `java/frc/robot` folder located within the `src/main` folder.

For this project, we will focus mainly on the Teleoperated mode, and so all other modes can be ignored, or commented out.

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

## Importing Classes

{% hint style="info" %}
If you are using the CyberLions provided template, then the classes are already imported for you, so that you can focus more on making the motors spin.  Importing classes will be covered on a different page.
{% endhint %}

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
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;

public class Robot extends TimedRobot {
  // Step 1 - Create motors here
  
  
  @Override
  public void teleopInit() {
    // Step 2 - Initialize and configure motors (if needed)
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

### Variables

Recall that in programming, a variable is a named container that can hold a value.  Anytime we want to use or reference the value in the container, we can just use the name.

To create a variable in Java / C++, you must provide the following:

1. <mark style="color:red;">**Data type**</mark> of the variable
2. <mark style="color:red;">**Name**</mark> of the variable
3. <mark style="color:red;">**(Optional) The value**</mark> of the variable

{% tabs %}
{% tab title="Java" %}
{% code lineNumbers="true" %}
```java
int x;        // An integer variable named x
double y;     // Use doubles to store decimal numbers
boolean z;    // true / false

int a = 5;    // Variables can be assigned a value right away
y = 2.7;      // You can assign values anytime after a variable is created
a = 15;       // Variables can update their values at any time
z = true;
```
{% endcode %}
{% endtab %}

{% tab title="C++" %}
{% code lineNumbers="true" %}
```cpp
int x;        // An integer variable named x
double y;     // Use doubles to store decimal numbers
bool z;       // true / false

int a = 5;    // Variables can be assigned a value right away
y = 2.7;      // You can assign values anytime after a variable is created
a = 15;       // Variables can update their values at any time
z = true;
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Creating the Motors

We will create a variable to represent the left XRP motor, which is plugged into port (pin, channel) 0 (see [WPILib: Getting to know your XRP](https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html)).

1. The data type is `XRPMotor`
2. We will name the variable `m_leftMotor`
3. The value of the motor variable is - in a literal sense - "a motor plugged into port 0"

{% hint style="info" %}
Note that for our motor, the value is given as `XRPMotor(0)` .  Simple data types such as numbers, text, and Booleans can just be assigned the data value directly (e.g., `5`, `2.8`, `"hello"`, `true`).  Complicated data types (classes) create values using what's called the "class constructor".  More on this later.\
\
In the meantime, just know that the format for constructing XRP motors in code is `XRPMotor(#)`, where the `#` represents the port that the motor is plugged into (Java also needs you to use the word `new`).
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;

public class Robot extends TimedRobot {
  // Step 1 - Create motors here
  // Creating an XRP motor that is plugged into port 0
  XRPMotor m_leftMotor = new XRPMotor(0);
  
  
  @Override
  public void teleopInit() {
    // Step 2 - Initialize and configure motors (if needed)
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

### TeleopInit

The `teleopInit` function is a section of code that will run exactly one time and runs when the robot is switched to teleoperated mode.  Since we are not really doing anything fancy with our motor, we can leave this function blank.

### TeleopPeriodic

The code in the `teleopPeriodic` function runs continuously while the robot is in teleoperated mode (and only after `teleopInit` has finished running).

In this section of code, we want to run the motor at some speed (say, 50% of its maximum speed).  To interface with our motors and sensors in code, we use what's called the "dot operator".  Using the dot operator will allow us to "tell" the motor to do things in code.  In this case, the thing we want to "tell the motor to do" is to "set the speed to 50%".

{% hint style="info" %}
Feel free to experiment with different speed values.  Speed values run from `-1.0` to `1.0` , with negative speed values making the motor spin in the reverse direction.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;

public class Robot extends TimedRobot {
  // Step 1 - Create motors here
  // Creating an XRP motor that is plugged into port 0
  XRPMotor m_leftMotor = new XRPMotor(0);
  
  
  @Override
  public void teleopInit() {
    // Step 2 - Initialize and configure motors (if needed)
    // No motor initialization needed; leaving this blank
  }

  @Override
  public void teleopPeriodic() {
    m_leftMotor.set(0.5);  // setting the motor to run at 50% speed
  }
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Simulate and Test Code

Connect to the XRP and [simulate your code](../../../getting-started/wpilib-vscode-how-to/wpilib-simulate-code.md) to run it.

{% hint style="info" %}
See [How to Simulate Code](../../../getting-started/wpilib-vscode-how-to/wpilib-simulate-code.md).
{% endhint %}

***

## Extension 1 - Creating the Right Motor

Follow a similar process and create a variable for the XRP motor on the right, and run it.  The right motor is plugged into port 1 on the XRP.

Expand the code below for a sample solution.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" expandable="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;

public class Robot extends TimedRobot {
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  
  
  @Override
  public void teleopInit() {}

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

We can invert the right motor by using the dot operator to tell the motor to use the `setInverted` function, and setting the inversion to `true`.  Do this within `teleopInit`, since we want to invert the motor exactly once, at the start of the teleoperated mode.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" expandable="true" %}
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
