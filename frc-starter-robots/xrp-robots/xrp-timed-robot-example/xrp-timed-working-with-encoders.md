# XRP Timed: Working with Encoders

## Quadrature Encoders

Encoders are devices that measure how many times something has rotated.  It does this by measuring how many pulses (or ticks) have gone by as the motor is turning.  For example, the XRP encoders measure one rotation every 585 ticks; so once it has measured 585 ticks, then you know the motor shaft (and wheel) has made one full rotation.

We use can this to measure the distance a wheel has travelled for example.  For a slightly more in-depth discussion on quadrature encoders, see the link below.

{% embed url="https://kb.vex.com/hc/en-us/articles/360039512851-Using-the-V5-3-Wire-Optical-Shaft-Encoder" %}

## Programming Encoders

### Starter Code

This page assumes you've created the [XRP Timed Robot Drive Program](xrp-timed-robot-drive-program.md).  You can also use the starter code found in the [CyberLions Starter Code Repository](https://github.com/CyberLions-8521/wpilib-starter-v2024.3.2), and use the XRP Timed starter code.

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

