# Extension 2 - Teleoperated Drive for Distance

## Drive for Distance in Teleoperated Mode

We can have the "drive for distance" behavior within our teleoperated code as well.  We will use the controller button presses to mimic the autonomous behavior.

* `getXButtonPressed()` will get the initial button press, and mimics `autonomousInit()`
* `getXButton()` will mimic `autonomousPeriodic()`  and will run the "drive for distance" code as long as the X key is pressed

Complete the code below by filling out the sections marked `TODO`.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" lineNumbers="true" %}
```java
// Other code not shown
private double fwd;
private double turn;
private double avgDist;

public Robot() {
  m_leftEncoder.setDistancePerPulse(convFactor);
  m_rightEncoder.setDistancePerPulse(convFactor);
  m_rightMotor.setInverted(true);
}

@Override
public void teleopPeriodic() {
  m_leftEncoder.reset();
  m_rightEncoder.reset();
}

@Override
public void teleopPeriodic() { 
  if (m_controller.getXButtonPressed()) {  // initial button press - init
    // Startup code - what should the encoders and robot do
    // when you initially press the button?
    // TODO: Fill out this section of code
    
    
  } else if (m_controller.getXButton()) {  // button is held - periodic
      avgDist = (m_leftEncoder.getDistance() + m_rightEncoder.getDistance()) / 2;
      // What should the robot do when the button is being held?
      // TODO: Fill out this section of code
      
          
  } else {
    // The robot defaults to the driving behavior otherwise
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
