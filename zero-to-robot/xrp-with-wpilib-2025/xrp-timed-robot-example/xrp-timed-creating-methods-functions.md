# XRP Timed: Creating Methods (Functions)

## XRP Code so Far

If you have been following along with the XRP Timed Robot tutorials, you might have code that looks like this.  Notice that it is very difficult to figure out what your program is doing overall in any of the robot modes (autonomous, teleoperated, and during each button press).

The robot behavior is as follows:

* Drives 5 inches during autonomous mode
* During teleoperated mode...
  * Does tank drive
  * X-button drives 5 inches
  * Left bumper button turns the robot 90 degrees left
  * Right bumper button turns the robot 90 degrees right
  * A button waves the servo

{% tabs %}
{% tab title="Java" %}
{% code expandable="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.Encoder;
import edu.wpi.first.wpilibj.xrp.XRPMotor;
import edu.wpi.first.wpilibj.xrp.XRPServo;
import edu.wpi.first.wpilibj.XboxController;

public class Robot extends TimedRobot {
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  XRPServo m_servo = new XRPServo(4);  
  Encoder m_leftEncoder = new Encoder(4, 5);
  Encoder m_rightEncoder = new Encoder(6, 7);
  
  double wheelDiameter = 2.3622;  // inches
  double wheelCircumference = wheelDiameter * Math.PI;  // C = pi * D
  double countsPerWheelRev = 585; // counts per one revolution of the wheel
  double convFactor = wheelCircumference / countsPerWheelRev;
  
  double trackWidth = 6.1;  // inches

  @Override
  public void autonomousInit() {
    m_leftEncoder.reset();
    m_leftEncoder.setDistancePerPulse(convFactor);
    m_rightEncoder.reset();
    m_rightEncoder.setDistancePerPulse(convFactor);
  }
  
  public void autonomousPeriodic() {
    double avgMeasuredDist = (m_leftEncoder.getDistance() + m_rightEncoder.getDistance()) / 2.0;
    if (avgMeasuredDist < 5) { // drive for 5 inches; you can change this
      m_leftMotor.set(0.5);
      m_rightMotor.set(0.5);
    } else {
      m_leftMotor.set(0.0);
      m_rightMotor.set(0.0);
    }
  }

  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    m_servo.setPosition(0.5);
    m_leftEncoder.reset();
    m_leftEncoder.setDistancePerPulse(convFactor);
    m_rightEncoder.reset();
    m_rightEncoder.setDistancePerPulse(convFactor);
  }

  @Override
  public void teleopPeriodic() {
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    
    if (m_controller.getXButtonPressed()) {  // initial button press - init
      m_leftEncoder.reset();
      m_rightEncoder.reset();
    } else if (m_controller.getXButton()) {  // button is held - periodic
        double avgDist = (m_leftEncoder.getDistance() + m_rightEncoder.getDistance()) / 2.0;
        if (avgDist < 5) {  // 5 inches
          m_leftMotor.set(0.5);
          m_rightMotor.set(0.5);
        } else {
          m_leftMotor.set(0.0);
          m_rightMotor.set(0.0);
        }
    } else if (m_controller.getLeftBumperButtonPressed()) {
      m_leftEncoder.reset();
      m_rightEncoder.reset();
    } else if (m_controller.getLeftBumperButton()) {
      double absLeft = Math.abs(m_leftEncoder.getDistance());
      double absRight = Math.abs(m_rightEncoder.getDistance());
      double avgAbsDist = (absLeft + absRight) / 2.0;
      double quarterTurn = 0.25 * Math.PI * trackWidth;
      if (avgAbsDist < quarterTurn) {
        m_leftMotor.set(-0.5);
        m_rightMotor.set(0.5);
      } else {
        m_leftMotor.set(0.0);
        m_rightMotor.set(0.0);
      }
    } else if (m_controller.getRightBumperButtonPressed()) {
      m_leftEncoder.reset();
      m_rightEncoder.reset();
    } else if (m_controller.getRightBumperButton()) {
      double absLeft = Math.abs(m_leftEncoder.getDistance());
      double absRight = Math.abs(m_rightEncoder.getDistance());
      double avgAbsDist = (absLeft + absRight) / 2.0;
      double quarterTurn = 0.25 * Math.PI * trackWidth;
      if (avgAbsDist < quarterTurn) {
        m_leftMotor.set(0.5);
        m_rightMotor.set(-0.5);
      } else {
        m_leftMotor.set(0.0);
        m_rightMotor.set(0.0);
      }
    } else {  // tank drive by default if button is not being pressed
      m_leftMotor.set(leftSpeed);
      m_rightMotor.set(rightSpeed);
    }
    
    if (m_controller.getAButton()) {
      m_servo.setPosition(1.0);
    } else {
      m_servo.setPosition(0.5);
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

***

## Methods

For a brief introduction or review of methods, see the beginning sections of [About Methods](../../../intro-to-programming-arduino/functions/about-functions/).

### Defining Methods in Java/C++

Defining methods in both Java and C++ follow similar patterns.  Methods have four (4) main components: the return type, the method name, the parameter list, and the method body.  Specifically in Java, you also include the access specifier (public or private).

{% tabs %}
{% tab title="Java" %}
```java
// General Pattern
public returnType methodName(paramType paramName) {
    // method body
}

// Example - A method that will reset both my encoders for me
public void resetEncoders() {
    m_leftEncoder.reset();
    m_rightEncoder.reset();
}
```
{% endtab %}

{% tab title="C++ (Header)" %}
```cpp
// General Pattern
returnType methodName(paramType paramName);

// Example - A method that will reset both my encoders for me
void resetEncoders();
```
{% endtab %}

{% tab title="C++ (Source)" %}
```cpp
// General Pattern
returnType methodName(paramType paramName) {
    // method body
}

// Example - A method that will reset both my encoders for me
void resetEncoders() {
    m_leftEncoder.reset();
    m_rightEncoder.reset();
}
```
{% endtab %}
{% endtabs %}

## Refactoring with Methods

We will create all our methods underneath of (but not inside of) the `teleopPeriodic`  method.  In essence, we are just copying the code out of the robot operating modes, and putting them in their own blocks of code.  This will allow us to reuse our code by calling the appropriate method, instead of copying and pasting the code all over again.

After we've moved all the relevant code into functions, we can replace those lines of code in our original program with function calls instead.

{% hint style="info" %}
The methods created below are simply common examples that increase or extend the functionality of our robot.  In practice, you are free to make any kinds of methods that you want; you can make more methods than are shown below, less methods, or other methods (such as methods that output data onto `SmartDashboard` or something else for debugging purposes).
{% endhint %}

### Drive Method

We will make a method named `tankDrive`, that accepts two decimal numbers as parameter (input) to represent the speeds of our motors, and sets our motors to that speed.  The return type will be `void` because the method will do something (i.e., set the speed of the motor), but no data is being returned to us by the method.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

public class Robot extends TimedRobot {
  /** Other code not shown **/
  
  @Override
  public void teleopPeriodic() {
    // code not shown
  }
    
  /** New Code - Defining the Tank Drive method **/
  public void tankDrive(double leftSpeed, double rightSpeed) {
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

### Reset Encoders Method

We notice in our code that we are resetting our encoders multiple times in multiple places.  We can bundle the code into a method that resets both (or all encoders if we later write code that uses more than two) encoders for us.

This method will take in no inputs, and will also be `void` , since it will also not be returning any data to us.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

public class Robot extends TimedRobot {
  /** Other code not shown **/
  @Override
  public void teleopPeriodic() {
    // code not shown
  }
  
  /** Previously created methods not shown **/
  /** New Code - Defining the resetEncoders method **/
  public void resetEncoders() {
    m_leftEncoder.reset();
    m_rightEncoder.reset();
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

### Set Encoder Distance Per Pulse Method

For our particular program, since we know that both encoders have the same distance per pulse, we can create a method that takes in a single parameter (the distance per pulse), and sets the distance per pulse of both encoders to that input value.

{% hint style="info" %}
Remember that programs are simply a tool that you use to get the job done; there is no "one, absolute way" to program a robot in FRC.  If you wanted more control over your encoders, you can make a method that takes in two, separate parameters (one for the left distance per pulse, and one for the right) - similar to our tank drive method - and set the distance per pulse values of each encoder separately.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

public class Robot extends TimedRobot {
  /** Other code not shown **/  
  @Override
  public void teleopPeriodic() {
    // code not shown
  }

  /** Previously created methods not shown **/  
  /** New Code - Set Distance Per Pulse Method **/
  public void setEncoderDistPerPulse(double distPerPulseRatio) {
    m_leftEncoder.setDistancePerPulse(distPerPulseRatio);
    m_rightEncoder.setDistancePerPulse(distPerPulseRatio);
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

### Average Encoder Distance Method

We notice that we are computing the average distance the encoders have travelled frequently throughout our code.  We can write a method that returns this value to us as a `double` .

{% hint style="info" %}
Since we only care about the distance travelled by the encoder, and not necessarily the direction the encoder was going, we take the absolute value of the encoder distances when computing the average.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

public class Robot extends TimedRobot { 
  /** Other code not shown **/  
  @Override
  public void teleopPeriodic() {
    // code not shown
  }
  
  /** Previously created methods not shown **/
  public double averageEncoderDist() {
    double leftEncDist = Math.abs(m_leftEncoder.getDistance());
    double rightEncDist = Math.abs(m_rightEncoder.getDistance());
    return (leftEncDist + rightEncDist) / 2.0;
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}
```java
import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  /** Other code not shown - Making all our motors, encoders, etc. **/
  double trackwidth = 6.1;        // inches
  double pulsePerRev = 585;       // pulses per revolution of the encoder
  
  // Distance traveled in inches of one revolution of the wheel
  double circumference = Math.PI * wheelDiameter;
  
  // Distance traveled in inches of one pulse of the encoder
  double distPerPulse = circumference / pulsePerRev;
  
  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    resetEncoders();
    setDistancePerPulse(distPerPulse);
  }
  
  @Override
  public void teleopPeriodic() {
    double leftStick = -m_controller.getLeftY();
    double rightStick = -m_controller.getRightY();
    tankDrive(leftStick, rightStick);  // calling the method
  }
  
  public void tankDrive(double leftSpeed, double rightSpeed) {
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
  }
  
  public void resetEncoders() {
    m_leftEncoder.reset();
    m_rightEncoder.reset();
  }
  
  /** New Code - Set Distance Per Pulse Method **/
  public void setDistancePerPulse(double distPerPulseRatio) {
    m_leftEncoder.setDistancePerPulse(distPerPulseRatio);
    m_rightEncoder.setDistancePerPulse(distPerPulseRatio);
  }
}
```
{% endtab %}
{% endtabs %}

### Drive Until Distance Method

Using the methods we created previously, we can make another method that causes our robot to drive until a certain distance is read by the encoders.  This method will take in a single parameter - the distance (in inches) we wish to travel - and return nothing (making it `void`).

{% hint style="info" %}
You can also have your method take in a separate parameter for the speed so that you can control how fast the motors will spin while driving to the appropriate distance.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

public class Robot extends TimedRobot { 
  /** Other code not shown **/  
  @Override
  public void teleopPeriodic() {
    // code not shown
  }
  
  /** Previously created methods not shown **/
  public void driveUntilDist(double distInInches, double speed) {
    if (averageEncoderDist() < distInInches) {
      tankDrive(speed, speed);
    } else {
      tankDrive(0.0, 0.0);
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

### Turn Methods

We can combine the methods we made previously to make the robot make a left, or right turn.  In this example, we have decided to have the angle of the turn be given to the method as a parameter.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

public class Robot extends TimedRobot {
  /** Other code not shown **/  
  @Override
  public void teleopPeriodic() {
    // code not shown
  }
  
  /** Previously created methods not shown **/
  public void leftTurn(double degrees) {
    double turnDist = (degrees / 360.0) * Math.PI * trackwidth;
    if (averageEncoderDist() < turnDist) {
      tankDrive(-0.5, 0.5);  // alternatively: speed can be given as parameter
    } else {
      tankDrive(0.0, 0.0);
    }
  }
  
  public void rightTurn(double degrees) {
    double turnDist = (degrees / 360.0) * Math.PI * trackwidth;
    if (averageEncoderDist() < turnDist) {
      tankDrive(0.5, -0.5);  // alternatively: speed can be given as parameter
    } else {
      tankDrive(0.0, 0.0);
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

## Putting it All Together

We add back (or uncommented) the `Robot()` function, which is the `Robot` constructor, and behaves as a sort of `RobotInit`.  We put the code that can (or should) happen at the _very_, _very_ beginning of the code, when the robot initially turns on.

We do the following

1. Create variables - all motors, encoders, and data (wheel diameter, conversion factors, etc.)
2. `Robot()` - Initialize hardware
   * Reset encoders using `resetEncoders()`
   * Set encoder distance per pulse using `setEncoderDistPerPulse(#)`
   * invert right motor
   * set servo initial position to the midpoint (0.5, or 90 degrees)
3. `autonomousInit()` - Reset the encoders using `resetEncoders()`
4. `autonomousPeriodic()` - Run the "drive for distance" behavior using `driveUntilDist(#, #)`
5. `teleopInit()` - Nothing needs to be done here.  All setup code has already occurred in `Robot()`.  Optionally, you can reset the encoders again, just for good measure; use `resetEncoders()`
6. `teleopPeriodic()` - Run driving for distance, turning, servos, or anything else with the controller.  Tank drive by default for the drivebase.

{% hint style="info" %}
In the code below, you can see the usage of the `||` operator, which is the logical OR operator.  We are taking advantage of this to clean up our drivebase code, since we want to reset the encoders whenever the X-button OR left bumper button OR right bumper button is initially pressed.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;
import edu.wpi.first.wpilibj.xrp.XRPServo;
import edu.wpi.first.wpilibj.XboxController;
import edu.wpi.first.wpilibj.Encoder;

public class Robot extends TimedRobot {
  // 1. Create all variables
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  XRPServo m_servo = new XRPServo(4);
  Encoder m_leftEncoder = new Encoder(4, 5);
  Encoder m_rightEncoder = new Encoder(6, 7);
  double wheelDiameter = 2.3622;  // inches
  double trackwidth = 6.1;        // inches
  double countsPerWheelRev = 585;
  double wheelCircumference = Math.PI * wheelDiameter;  // inches
  double convFactor = wheelCircumference / countsPerWheelRev;  // inches per pulse
  
  // 2. Initialize hardware
  public Robot() {
    m_rightMotor.setInverted(true);
    resetEncoders();
    setEncoderDistPerPulse(convFactor);
    m_servo.setPosition(0.5);
  }
  
  // 3. Autonomous routine initialization code
  @Override
  public void autonomousInit() {
    resetEncoders();
  }
  
  // 4. Autonomous routine - drive for 5 inches
  @Override
  public void autonomousPeriodic() {
    driveUntilDist(5, 0.5);  // 5 inches at 50% speed
  }
  
  // 5. Optionally reset encoders for teleop mode
  @Override
  public void teleopInit() {
    resetEncoders();
  }
  
  // 6. Main robot loop during teleoperated mode
  @Override
  public void teleopPeriodic() {    
    // Servo code
    if (m_controller.getAButton()) {
      m_servo.setPosition(1.0);
    } else {
      m_servo.setPosition(0.0);
    }
    
    // Drivebase code
    // Initial press of any of the 3 buttons
    if (m_controller.getXButtonPressed() || m_controller.getLeftBumperButtonPressed() || m_controller.getRightBumperButtonPressed()) {
      resetEncoders();
    } else if (m_controller.getXButton()) {  // holding x-button
      driveUntilDist(5, 0.5);
    } else if (m_controller.getLeftBumperButton()) {  // holding left bumper
      turnLeft(90);
    } else if (m_controller.getRightBumperButton()) { // holding right bumper
      turnRight(90);
    } else {
      tankDrive(-m_controller.getLeftY(), -m_controller.getRightY());
    }
  }  // teleopPeriodic ends here
  
  // Here's all the functions we made to clean up the code from above
  public void tankDrive(double leftSpeed, double rightSpeed) {
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
  }
  
  public void resetEncoders() {
    m_leftEncoder.reset();
    m_rightEncoder.reset();
  }
  
  public void setEncoderDistPerPulse(double distPerPulseRatio) {
    m_leftEncoder.setDistancePerPulse(distPerPulseRatio);
    m_rightEncoder.setDistancePerPulse(distPerPulseRatio);
  }
  
  public double averageEncoderDist() {
    double leftEncDist = Math.abs(m_leftEncoder.getDistance());
    double rightEncDist = Math.abs(m_rightEncoder.getDistance());
    return (leftEncDist + rightEncDist) / 2.0;
  }
  
  public void driveUntilDist(double distInInches, double speed) {
    if (averageEncoderDist() < distInInches) {
      tankDrive(speed, speed);
    } else {
      tankDrive(0.0, 0.0);
    }
  }
  
  public void leftTurn(double degrees) {
    double turnDist = (degrees / 360.0) * Math.PI * trackwidth;
    if (averageEncoderDist() < turnDist) {
      tankDrive(-0.5, 0.5);  // alternatively: speed can be given as parameter
    } else {
      tankDrive(0.0, 0.0);
    }
  }
  
  public void rightTurn(double degrees) {
    double turnDist = (degrees / 360.0) * Math.PI * trackwidth;
    if (averageEncoderDist() < turnDist) {
      tankDrive(0.5, -0.5);  // alternatively: speed can be given as parameter
    } else {
      tankDrive(0.0, 0.0);
    }
  }
}  // Robot class definition ends here
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  /** Other code not shown - Making all our motors, encoders, etc. **/
  
  /** Constants shown for use in the method below **/
  double trackwidth = 6.1;        // inches
  double pulsePerRev = 585;       // pulses per revolution of the encoder
  
  // Distance traveled in inches of one revolution of the wheel
  double circumference = Math.PI * wheelDiameter;
  
  // Distance traveled in inches of one pulse of the encoder
  double distPerPulse = circumference / pulsePerRev;
  
  @Override
  public void teleopInit() {
    /** code not shown **/
  }
  
  @Override
  public void teleopPeriodic() {
    /** other code not shown **/
    
    /** Updated left turn bound to Y button **/
    if (m_controller.getYButton()) {
      leftTurn(90);  // degrees
    }
    
    /** Updated right turn bound to B button **/
    if (m_controller.getBButton()) {
      rightTurn(90);  // degrees
    }
  }
  
  /** Previously created methods not shown **/
  
  public void leftTurn(double degrees) {
    resetEncoders();
    double turnDist = (degrees / 360.0) * Math.PI * trackwidth;
    while (averageEncoderDist() < turnDist) {
      tankDrive(-0.5, 0.5);  // alternatively: speed can be given as parameter
    }
  }
  
  public void rightTurn(double degrees) {
    resetEncoders();
    double turnDist = (degrees / 360.0) * Math.PI * trackwidth;
    while (averageEncoderDist() < turnDist) {
      tankDrive(0.5, -0.5);  // alternatively: speed can be given as parameter
    }
  }
}
```
{% endtab %}
{% endtabs %}
