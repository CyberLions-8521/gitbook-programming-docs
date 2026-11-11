# Extension 1 - Add the Right Encoder

## Add the Right Encoder

So far, this project has shown you how to configure and use the left encoder.  Complete your code in the sections shown below to add the right encoder, and measure using the average encoder distance for more accurate results.

The right encoder's channels A and B are on ports 6 and 7 respectively.

{% hint style="info" %}
You do not need to recreate the variables for the conversion factor for the right encoder, since the values are the same.  Simply reuse the same conversion factor variable when setting the distance per pulse.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.Encoder;
import edu.wpi.first.wpilibj.xrp.XRPMotor;
import edu.wpi.first.wpilibj.XboxController;

public class Robot extends TimedRobot {
  XboxController m_controller = new XboxController(0);
  XRPMotor m_leftMotor = new XRPMotor(0);
  XRPMotor m_rightMotor = new XRPMotor(1);
  
  Encoder m_leftEncoder = new Encoder(4, 5);
  // TODO: Create the right encoder on ports 6 and 7
  
  double wheelDiameter = 2.3622;  // inches
  double wheelCircumference = wheelDiameter * Math.PI;  // C = pi * D
  double countsPerWheelRev = 585; // counts per one revolution of the wheel
  double convFactor = wheelCircumference / countsPerWheelRev;

  @Override
  public void autonomousInit() {
    m_leftEncoder.reset();
    m_leftEncoder.setDistancePerPulse(convFactor);
    
    // TODO: reset and set the distance per pulse for the right encoder
  }
  
  public void autonomousPeriodic() {
    // TODO: Average the encoder readings, and store it into the variable below
    double avgMeasuredDist = 0.0; // replace this value
    
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
  }

  @Override
  public void teleopPeriodic() {
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
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
