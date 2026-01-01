# XRP Timed: Working with Encoders

## Quadrature Encoders

Encoders are devices that measure how many times something has rotated.  It does this by measuring how many pulses (or ticks) have gone by as the motor is turning.  For example, the XRP encoders measure one rotation every 585 ticks; so once it has measured 585 ticks, then you know the motor shaft (and wheel) has made one full rotation.

We use can this to measure the distance a wheel has travelled for example.  For a slightly more in-depth discussion on quadrature encoders, see the link below.

{% embed url="https://kb.vex.com/hc/en-us/articles/360039512851-Using-the-V5-3-Wire-Optical-Shaft-Encoder" %}

## Programming Encoders

### Starter Code

This page assumes you've created the [XRP Timed Robot Drive Program](using-a-controller-tank-drive.md).  You can also use the starter code found in the [CyberLions Starter Code Repository](https://github.com/CyberLions-8521/wpilib-starter-v2024.3.2), and use the XRP Timed starter code.

### Encoder Class

The XRP robot uses the `Encoder` class from the WPILib.  The full API can be found below.  For this program, we will use the following constructors and methods from the Encoder class.

{% embed url="https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj/Encoder.html" %}

| Constructor/Method                              | Description                                                                                                                                              |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Encoder(int channelA, int channelB)`           | Constructs a motor.  You need to tell it what pins/ID numbers the sensor channels are on (channel A and channel B).                                      |
| `double getDistance()`                          | Returns the current distance the encoder has "traveled"/measured.  The units of the distance can be changed by using the `setDistancePerPulse()` method. |
| `void setDistancePerPulse(double distPerPulse)` | Sets the distance traveled per one pulse or tick of the encoder.                                                                                         |
| `void reset()`                                  | Reset the encoder back to zero.                                                                                                                          |

Import the Encoder class into your program.

{% tabs %}
{% tab title="Java" %}
```java
import edu.wpi.first.wpilibj.Encoder;
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

### Setting up the Encoder

We will be using the specifications of the XRP robot according to the WPILib documentation found below.  We notice the following:

* The left encoder is plugged into ports 4 and 5 for channels A and B
* The right encoder is plugged into ports 6 and 7 for channels A and B
* The XRP wheels have a diameter of 60 mm (2.3622 inches)
* The encoders count 585 pulses (ticks, counts) per single revolution
* The robot has a trackwidth (distance between wheels) of 155 mm (6.1 inches)

{% embed url="https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html" %}

We begin by creating our `Encoder` objects in code.  We will also do the following:

* Preemptively make variables to keep track of all the data relevant for encoders
* Reset the encoders in the `teleopInit()` method so that they start at 0 as soon as the teleoperated mode starts.

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
  double pulsePerRev = 585;
  
  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    m_leftEncoder.reset();
    m_rightEncoder.reset();
  }
  /** additional code not shown **/
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

### Calculating Distance Per Pulse

{% hint style="info" %}
Note that this section can be skipped if you do not care for the math.  Just update your code so that it matches the example (as in, set the distance per pulse of each encoder).  Continuing reading on below if you want to understand the mathematics behind the code.
{% endhint %}

We want to set the distance per pulse of the encoder so that whenever we read the distance from the encoder, we get a meaningful answer back (because getting a reading of 687 ticks is hard to make meaning out of, but getting a reading of 8.715 inches traveled _is_ meaningful).  By setting the distance per pulse of each encoder, the encoder will convert pulses to distance for us, so that we do not have to.

To find the distance per pulse, we first find the distance travelled when the wheel turns once.  This is given by the circumference of the wheel, since when the wheel turns once, it has travelled a length exactly equal to its circumference.  We then divide this by the number of pulses per revolution, to get a final value in units of distance per pulse.

The mathematical equation to find the distance per pulse is given below.  Notice that the units of revolutions cancels out, and you obtain distance (from the circumference which is in units of length) per pulses, which is what we wanted.

$$
\text{distance per pulse} = \dfrac{\text{circumference}}{1\text{ revolution}}\cdot\dfrac{1\text{ revolution}}{\text{number of pulses}}
$$

Update your code so that you have set the distance per pulse for the encoders.

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
  double pulsePerRev = 585;
  
  /** New code here **/
  double circumference = Math.PI * wheelDiameter;
  double distPerPulse = circumference / pulsePerRev;
  
  @Override
  public void teleopInit() {
    m_rightMotor.setInverted(true);
    m_leftEncoder.reset();
    m_rightEncoder.reset();
    
    /** New code here **/
    m_leftEncoder.setDistancePerPulse(distPerPulse);
    m_rightEncoder.setDistancePerPulse(distPerPulse);
  }
  /** additional code not shown **/
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

### Driving for a Distance

We complete our project by having our robot drive straight for a certain distance.  We'll have the robot execute this maneuver whenever we press the X button on our X Box Controller (or the square button on a PlayStation controller).  We add this code to `teleopPeriodic()`.

Note that when measuring the distance travelled, we use the average of the distance between the two encoders for more accurate readings.  (You can technically measure from only the left or the right encoder since the robot is only driving straight in this case.)

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
  double pulsePerRev = 585;
  
  /** New code here **/
  double circumference = Math.PI * wheelDiameter;
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
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
    
    /** When I press the X button, then...
    * as long as the average distance is less than 5 (inches in this case)
    * we drive the robot forward. You can change the distance to whatever
    * you want and change the units to whatever you want 
    * based on setDistancePerPulse.
    */
    if (m_controller.getXButton()) {
      while ((m_leftEncoder.getDistance() + m_rightEncoder.getDistance()) / 2.0 < 5) {
        // drive forward
        m_leftMotor.set(0.5);
        m_rightMotor.set(0.5);
      }
    } 
  }
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

## Extension 1 - Repeatable Drive Distance

If you followed the code sample above, you'll notice that the robot only drives for distance one time, no matter how many times you press the X button.

Modify your code so that the robot drives for distance _every_ time you press the X button.

## Extension 2 - Turning

One way to turn the robot at a right angle (left or right turn) is to make the motors run until the robot has traced out one-fourth of a circle (a quarter-turn).  Modify your code so that when you press certain buttons, the robot makes (near) perfect left and right turns.  The image below shows _one way_ to think about the problem (how to turn in place).  There is more than one way to solve the problem, and more than one way to turn (e.g., turning in place, or turning on a pivot).

<figure><img src="../../../../.gitbook/assets/xrp_quarter_turn.PNG" alt=""><figcaption><p>Circle traced out when turning in place</p></figcaption></figure>

## Extension 3 - Square

Combine all of the above to make the robot trace out a square shape.
