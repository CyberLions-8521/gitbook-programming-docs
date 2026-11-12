# Extension 1 - Add the Right Encoder

## Add the Right Encoder

So far, this project has shown you how to configure and use the left encoder.  Add a right encoder to your program.

* Configure it by setting the distance per pulse within `Robot()`.
* Reset its distance measurement within `autonomousInit()`.
* Use it alongside the left encoder to measure the distance traveled to travel for distance in `autonomousPeriodic()`.

You can either use your code, or copy and complete the code below.

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

import frc.robot.util.KeyboardController;

import edu.wpi.first.wpilibj.Encoder;
import edu.wpi.first.wpilibj.xrp.*;

public class Robot extends TimedRobot {
  private final KeyboardController m_controller = new KeyboardController(0);
  private final XRPMotor m_leftMotor = new XRPMotor(0);
  private final XRPMotor m_rightMotor = new XRPMotor(1);
  
  private final Encoder m_leftEncoder = new Encoder(4, 5);
  // TODO: Create the right encoder on ports 6 and 7
  
  private final double wheelDiameter = 2.3622;  // inches
  private final double wheelCircumference = wheelDiameter * Math.PI;  // C = pi * D
  private final double countsPerWheelRev = 585; // counts per one revolution of the wheel
  private final double convFactor = wheelCircumference / countsPerWheelRev;
  
  private double fwd;
  private double turn;

  public Robot() {
    m_leftEncoder.setDistancePerPulse(convFactor);
    m_rightMotor.setInverted(true);
  }  

  @Override
  public void autonomousInit() {
    m_leftEncoder.reset();
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
    
  }

  @Override
  public void teleopPeriodic() {
    fwd = -m_controller.getWSAxis();
    turn = -m_controller.getADAxis();
    
    m_leftMotor.set(fwd - turn);
    m_rightMotor.set(fwd + turn);
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
