# Extension 2 - Teleoperated Drive for Distance

## Drive for Distance in Teleoperated Mode

We can have the "drive for distance" behavior within our teleoperated code as well.  We will use the controller button presses to mimic the autonomous behavior.

* `getXButtonPressed()` will get the initial button press, and mimics `autonomousInit()`
* `getXButton()` will mimic `autonomousPeriodic()`  and will run the "drive for distance" code as long as the X-button is pressed

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
@Override
public void teleopPeriodic() {
  m_leftEncoder.reset();
  m_rightEncoder.reset();
  
  m_leftEncoder.setDistancePerPulse(convFactor);
  m_rightEncoder.setDistancePerPulse(convFactor);
}

@Override
public void teleopPeriodic() { 
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
  } else {  // tank drive by default if button is not being pressed
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
