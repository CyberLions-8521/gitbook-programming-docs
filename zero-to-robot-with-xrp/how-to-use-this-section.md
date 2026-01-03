# How to Use this Section

## The XRP Robots

At the time of writing this documentation, the XRP robots use the same WPILib libraries (i.e., the same libraries as used on "real" FRC robots), and so learning how to program the XRP is almost the same as learning how to program the FRC robot controller.

Additionally, FRC teams can get an XRP for free from Digikey (see [Digikey FRC Resources](https://www.digikey.com/en/resources/first) page).

* Making motors spin
* Using a controller to drive the robot
* Using encoders and programming an autonomous routine
* Organizing code with functions and classes

## Other Robots

As the programming style is basically the same, you _could_ potentially switch out the XRP specific hardware for motors from the Romi robot, or SparkMax motor controllers plugged into PWM ports (CIM, NEO, etc.), and your code will still be able to run said other hardware.  As in, you can still follow along with the XRP tutorials, and just use other hardware as needed.

{% hint style="info" %}
These early sections are geared mainly more towards initial introductions to how to get motors moving in FRC, and focus on the Romi / XRP, or PWM.  Usage of CAN devices will force you to use vendor specific libraries.  Running the motors is largely the same (with some sort of `set` method), but using other sensors, such as encoders, is much different than what is presented for the XRP.
{% endhint %}

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="image">Cover image</th></tr></thead><tbody><tr><td><p>Romi Robots</p><ul><li><a href="https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj/romi/package-summary.html">WPILib Romi Classes</a> - <code>RomiMotor</code>, <code>RomiGyro</code> </li><li><a href="https://docs.wpilib.org/en/stable/docs/romi-robot/getting-to-know-romi.html">Port numbers for Romi hardware</a></li><li><a href="https://docs.wpilib.org/en/stable/docs/romi-robot/hardware-support.html">Other hardware and sensors the Romi robot is compatible with from WPILib</a></li></ul></td><td><a href="https://docs.wpilib.org/en/stable/_images/assembly-pi.webp">https://docs.wpilib.org/en/stable/_images/assembly-pi.webp</a></td></tr><tr><td><p>SparkMax motor controllers using PWM</p><ul><li><a href="https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj/motorcontrol/PWMSparkMax.html">PWMSparkMax API</a> from WPILib</li></ul></td><td><a href="https://docs.wpilib.org/en/stable/_images/spark-max-motor-controller.png">https://docs.wpilib.org/en/stable/_images/spark-max-motor-controller.png</a></td></tr><tr><td><p>CTRE Falcon or Kraken motors</p><ul><li><a href="https://api.ctr-electronics.com/phoenix6/release/java/com/ctre/phoenix6/hardware/TalonFX.html">TalonFX API</a> from Phoenix 6</li><li><a href="https://v6.docs.ctr-electronics.com/en/stable/docs/api-reference/examples/quickstart.html">Quickstart example</a></li></ul></td><td><a href="https://docs.wpilib.org/en/stable/_images/talonfx.webp">https://docs.wpilib.org/en/stable/_images/talonfx.webp</a></td></tr></tbody></table>
