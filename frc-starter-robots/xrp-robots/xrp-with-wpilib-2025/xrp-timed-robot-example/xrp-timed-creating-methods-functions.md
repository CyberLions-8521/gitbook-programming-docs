# XRP Timed: Creating Methods (Functions)

## Methods

For a brief introduction or review of methods, see the beginning sections of [About Methods](../../../../intro-to-programming-arduino/functions/about-functions/).

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

## XRP Code so Far

If you have been following along with the XRP Timed Robot tutorials, you might have code that looks like this.  Notice that it is a _lot_ of code, and that it is very difficult to figure out what your program is doing overall.

{% tabs %}
{% tab title="Java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  
  /** Creating the encoder objects **/
  Encoder m_leftEncoder = new Encoder(4, 5);  // channels A, B on ports 4 and 5
  Encoder m_rightEncoder = new Encoder(6, 7); // channels A, B on ports 6 and 7
  double wheelDiameter = 2.3622;  // inches
  double trackwidth = 6.1;        // inches
  double pulsePerRev = 585;       // pulses per revolution of the encoder
  
  // Distance traveled in inches of one revolution of the wheel
  double circumference = Math.PI * wheelDiameter;
  
  // Distance traveled in inches of one pulse of the encoder
  double distPerPulse = circumference / pulsePerRev;
  
  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    m_leftEncoder.reset();
    m_rightEncoder.reset();
    
    m_leftEncoder.setDistancePerPulse(distPerPulse);
    m_rightEncoder.setDistancePerPulse(distPerPulse);
  }
  
  @Override
  public void teleopPeriodic() {
    // Driving with the joysticks
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
    
    // Driving until a certain distance (5 inches in this example)
    if (m_controller.getXButton()) {
      // Reset encoders
      m_leftEncoder.reset();
      m_rightEncoder.reset();
      
      while ((Math.abs(m_leftEncoder.getDistance()) + Math.abs(m_rightEncoder.getDistance())) / 2.0 < 5) {
        // drive forward
        m_leftMotor.set(0.5);
        m_rightMotor.set(0.5);
      }
    }
    
    // Making a left, 90-degree turn
    if (m_controller.getYButton()) {
      // Reset encoders
      m_leftEncoder.reset();
      m_rightEncoder.reset();
    
      // Distance the wheel travels to make the robot turn in place for 90 degrees
      // This is 1/4th of the circle created by the wheels as the robot turns in place
      double quarterTurn = 0.25 * Math.PI * trackwidth;
      while ((Math.abs(m_leftEncoder.getDistance()) + Math.abs(m_rightEncoder.getDistance())) / 2.0 < quarterTurn) {
        m_leftMotor.set(-0.5);
        m_rightMotor.set(0.5);
      }
    }
    
    // Making a right, 90-degree turn
    if (m_controller.getBButton()) {
      // Reset encoders
      m_leftEncoder.reset();
      m_rightEncoder.reset();
    
      // Distance the wheel travels to make the robot turn in place for 90 degrees
      // This is 1/4th of the circle created by the wheels as the robot turns in place
      double quarterTurn = 0.25 * Math.PI * trackwidth;
      while ((Math.abs(m_leftEncoder.getDistance()) + Math.abs(m_rightEncoder.getDistance())) / 2.0 < quarterTurn) {
        m_leftMotor.set(0.5);
        m_rightMotor.set(-0.5);
      }
    }
  }
}
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

## Refactoring with Methods

We will create all our methods underneath of (but not inside of) the `teleopPeriodic`  method.

{% hint style="info" %}
The methods created below are simply common examples that increase or extend the functionality of our robot.  In practice, you are free to make any kinds of methods that you want; you can make more methods than are shown below, less methods, or other methods (such as methods that output data onto `SmartDashboard` or something else for debugging purposes).
{% endhint %}

### Drive Method

We will make a method named `tankDrive`, that accepts two decimal numbers as parameter (input) to represent the speeds of our motors, and sets our motors to that speed.  The return type will be `void` because the method will do something (i.e., set the speed of the motor), but no data is being returned to us by the method.

{% tabs %}
{% tab title="Java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  /** Code not shown - Making all our motors, encoders, etc. **/
  
  @Override
  public void teleopInit() {
    /** code not shown **/
  }
  
  @Override
  public void teleopPeriodic() {
    /** Updated Driving code **/
    double leftStick = -m_controller.getLeftY();
    double rightStick = -m_controller.getRightY();
    tankDrive(leftStick, rightStick);  // calling the method
  }
  
  /** New Code - Defining the Tank Drive method **/
  public void tankDrive(double leftSpeed, double rightSpeed) {
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

### Reset Encoders Method

We notice in our code that we are resetting our encoders multiple times in multiple places.  We can bundle the code into a method that resets both (or all encoders if we later write code that uses more than two) encoders for us.

This method will take in no inputs, and will also be `void` , since it will also not be returning any data to us.

{% tabs %}
{% tab title="Java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  /** Code not shown - Making all our motors, encoders, etc. **/
  
  /** Constants not shown - track width, wheel diameter, etc. **/
  
  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    
    /** Updated Encoder reset - other code not shown **/
    resetEncoders();
  }
  
  @Override
  public void teleopPeriodic() {
    /** code not shown **/
  }
  
  /** Previously created methods not shown **/
  
  /** New Code - Defining the resetEncoders method **/
  public void resetEncoders() {
    m_leftEncoder.reset();
    m_rightEncoder.reset();
  }
}
```
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
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  /** Other code not shown - Making all our motors, encoders, etc. **/
  
  /** Constants shown for use in the method below **/
  double wheelDiameter = 2.3622;  // inches
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
    
    /** Updated set distance per pulse **/
    setEncoderDistPerPulse(distPerPulse);
  }
  
  @Override
  public void teleopPeriodic() {
    /** code not shown **/
  }
  
  /** Previously created methods not shown **/
  
  /** New Code - Set Distance Per Pulse Method **/
  public void setEncoderDistPerPulse(double distPerPulseRatio) {
    m_leftEncoder.setDistancePerPulse(distPerPulseRatio);
    m_rightEncoder.setDistancePerPulse(distPerPulseRatio);
  }
}
```
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
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  /** Other code not shown - Making all our motors, encoders, etc. **/
  
  /** Constants not shown - track width, wheel diameter, etc. **/
  
  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    resetEncoders();
    setEncoderDistPerPulse(distPerPulse);
  }
  
  @Override
  public void teleopPeriodic() {
    /** code not shown **/
  }
  
  /** Previously created methods not shown **/
  
  public double averageEncoderDist() {
    double leftEncDist = Math.abs(m_leftEncoder.getDistance());
    double rightEncDist = Math.abs(m_rightEncoder.getDistance());
    return (leftEncDist + rightEncDist) / 2.0;
  }
}
```
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
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  /** Other code not shown - Making all our motors, encoders, etc. **/
  
  /** Constants not shown - track width, wheel diameter, etc. **/
  
  @Override
  public void teleopInit() {
    /** code not shown **/
  }
  
  @Override
  public void teleopPeriodic() {
    /** other code not shown **/
    
    /** Updated Drive until distance bound to X button **/
    if (m_controller.getXButton()) {
      driveUntilDist(5);  // inches
    }
  }
  
  /** Previously created methods not shown **/
  
  public void driveUntilDist(double distInInches) {
    resetEncoders();  // This calls the method we created earlier
    while (averageEncoderDist() < distInInches) {
      tankDrive(0.5, 0.5);  // Hardcoding to 50% speed just because.
                            // You can change this to any speed, or use
                            // a parameter to control the speed
    }
  }
}
```
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
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;   // XRPMotor
import edu.wpi.first.wpilibj.XboxController; // XboxController
import edu.wpi.first.wpilibj.Encoder;        // Encoder

public class Robot extends TimedRobot {
  /** Other code not shown - Making all our motors, encoders, etc. **/
  
  /** Constants shown for use in the method below **/
  double wheelDiameter = 2.3622;  // inches
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

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

## Putting it All Together

By now, your code might look something like this.  Notice it is much easier to understand what is going on in `teleopPeriodic`  when each button is pressed on the controller, and that we do not necessarily need to understand the details of how each method does its job.

{% tabs %}
{% tab title="Java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.xrp.XRPMotor;
import edu.wpi.first.wpilibj.XboxController;
import edu.wpi.first.wpilibj.Encoder;

public class Robot extends TimedRobot {
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  Encoder m_leftEncoder = new Encoder(4, 5);
  Encoder m_rightEncoder = new Encoder(6, 7);
  double wheelDiameter = 2.3622;
  double trackwidth = 6.1;
  double pulsePerRev = 585;
  double circumference = Math.PI * wheelDiameter;
  double distPerPulse = circumference / pulsePerRev;
  
  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    resetEncoders();
    setEncoderDistPerPulse(distPerPulse);
  }
  
  @Override
  public void teleopPeriodic() {
    double leftStick = -m_controller.getLeftY();
    double rightStick = -m_controller.getRightY();
    tankDrive(leftStick, rightStick);
    
    if (m_controller.getXButton()) {
      driveUntilDist(5);
    }
    
    if (m_controller.getYButton()) {
      leftTurn(90);
    }
    
    if (m_controller.getBButton()) {
      rightTurn(90);
    }
  }
  
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
  
  public void leftTurn(double degrees) {
    resetEncoders();
    double turnDist = (degrees / 360.0) * Math.PI * trackwidth;
    while (averageEncoderDist() < turnDist) {
      tankDrive(-0.5, 0.5);
    }
  }
  
  public void rightTurn(double degrees) {
    resetEncoders();
    double turnDist = (degrees / 360.0) * Math.PI * trackwidth;
    while (averageEncoderDist() < turnDist) {
      tankDrive(0.5, -0.5);
    }
  }
}
```
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
