# XRP Timed: Making a Class

## Classes

For a brief introduction or review of classes, see the beginning portions of the [Classes page within the Structs and Classes section](../../../intro-to-programming-arduino/structs-and-classes/classes.md).

## Why Make Classes - Subsystems

In the previous tutorial about making methods, we bundled together related code into methods to better organize our code and make it easier to maintain.  Our robot code is already getting fairly long, and we have only programmed a drivebase (some wheels and encoders).  You can imagine how much _longer_ our code is going to get once we add additional subsystems on the robot.  A robot subsystem is a collection of robot hardware that typically operates together as a unit to perform a task.  Examples of various subsystems include:

* Drivebases - Includes motors, encoders, gyroscopes, etc. in order to move the robot.
* Vertical Arms - A rod that pivots on one end to swing up and down.  Similar to the way your arm swings around your shoulder joint.
* Turrets - A mechanism that swivels in place.  Think of your head turning back and forth.
* Flywheels - Most anything that spins.  These are usually rollers or compliance wheels that are used to intake or shoot game pieces.
* Vertical Elevator - A mechanism that can act as a lift.

By bundling related objects (such as motors, encoders, etc.) together with their related functions (e.g., drive, lift, shoot) into separate classes, we can further organize our code and make it cleaner and easier to maintain.  Each class should be in its own separate file.

## Creating the Drivebase Class

We will make a class that represents our drivebase.  To better organize our code, we will make a folder for our subsystems (either within `java/frc/robot` or within `src/main/cpp`).  Name the folder "subsystems"

You can make new folders by right clicking where you want the folders to go within the File Explorer on the left sidebar of VSCode, and selecting "New Folder".

<figure><img src="../../../.gitbook/assets/wpilib-create-new-folder.PNG" alt=""><figcaption><p>Creating a new folder within a WPILib Project</p></figcaption></figure>

We are going to represent our drivebase (or drivetrain) as a class in our program.  Right click within your subsystems folder, and select "Create a new class/command".  Select "Empty Class" from the dropdown list that appears, and name it `Drivebase` (case-sensitive).

<figure><img src="../../../.gitbook/assets/wpilib-create-new-class-command.PNG" alt=""><figcaption><p>Creating a new class within a WPILib Project</p></figcaption></figure>

## Setting up the Code

We have already done most of the work for the class.  We are going to copy over all the objects and methods that are directly related to the drivebase.  From the `Robot` file, move over all imports, objects, and methods related to the `XRPMotor` , and `Encoder` objects.  Leave behind the teleoperated `init` and `periodic` methods, as well as the `XboxController`  and `XRPServo`.  See the code below for how your `Drivebase` file should look like.

{% hint style="info" %}
The imports and other lines of code should get deleted from the `Robot.java` file so that you can start cleaning it up.
{% endhint %}

While we are moving over the code we are going to add the `private` access specifier to all our objects and variables, and `final` to all of our variables.

* The `private` access specifier will make it so that the only way to access our motors and encoders will be through the `public` methods that we create (as in, other programmers will only have access to things that we explicitly give them access to)
* Making our variables `final` will ensure that their values will never change while the program is running.
  * This makes sense physically, since the motors and encoders should never change, and neither should the wheel diameter, trackwidth, etc.

{% tabs %}
{% tab title="Java" %}
{% code title="Drivebase.java" lineNumbers="true" %}
```java
package frc.robot.subsystems;

import edu.wpi.first.wpilibj.xrp.XRPMotor;
import edu.wpi.first.wpilibj.Encoder;

public class Drivebase {
  private final XRPMotor m_leftMotor = new XRPMotor(0);
  private final XRPMotor m_rightMotor = new XRPMotor(1);
  private final Encoder m_leftEncoder = new Encoder(4, 5);
  private final Encoder m_rightEncoder = new Encoder(6, 7);
  private final double wheelDiameter = 2.3622;  // inches
  private final double trackwidth = 6.1;        // inches
  private final double countsPerWheelRev = 585;
  private final double wheelCircumference = Math.PI * wheelDiameter;  // inches
  private final double convFactor = wheelCircumference / countsPerWheelRev;  // inches per pulse
  
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
}
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

## Class Constructor

We will initialize, reset, set conversion factors, etc., _within_ the class, so that anything that the `Drivebase` object would need created or initialized is done so within its own class.  This is usually done within the class constructor, and what we will do as well.

{% hint style="warning" %}
You might think that we could take advantage of the fact that the reset encoders, and set distance per pulse methods of the `Drivebase` class are public, and continue to use them within `telopInit`.  We would then also create a public "invert" method that inverts one of the drivebase motors.  However, this is going to lead to a lot of code cluttering within the `teleopInit` method, especially after we start adding more and more subsystems and need to initialize all of them.
{% endhint %}

The class constructor is the special method that has no return type, shares the same name as the class, and creates, and returns an object of the class back to us so that we can assign it to variables of that class type.  The constructor should be the first method defined after the variables (and before all the other methods).

{% hint style="info" %}
In the example below, one encoder has been initialized outside the constructor, while the other has been initialized _inside_ of the constructor.  This has been done purely for example to show you the many different ways you _could_ program a constructor, based on your preferences and needs.
{% endhint %}

{% tabs %}
{% tab title="Java" %}
Initialization outside of the constructor example.

{% code title="Drivebase.java" %}
```java
package frc.robot.subsystems;

import edu.wpi.first.wpilibj.xrp.XRPMotor;
import edu.wpi.first.wpilibj.Encoder;

public class Drivebase {
  private final XRPMotor m_leftMotor = new XRPMotor(0);
  private final XRPMotor m_rightMotor = new XRPMotor(1);
  private final Encoder m_leftEncoder = new Encoder(4, 5);
  private final Encoder m_rightEncoder = new Encoder(6, 7);
  private final double wheelDiameter = 2.3622;  // inches
  private final double trackwidth = 6.1;        // inches
  private final double countsPerWheelRev = 585;
  private final double wheelCircumference = Math.PI * wheelDiameter;  // inches
  private final double convFactor = wheelCircumference / countsPerWheelRev;  // inches per pulse
  
  // Constructor
  public Drivebase() {
    m_rightMotor.setInverted(true);
    resetEncoders();  // method from this class
    setEncoderDistPerPulse(convFactor);  // method and constant from this class
  }
  
  // Other methods below - not shown
}
```
{% endcode %}

Initialization inside of constructor example.
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

{% code title="Drivebase.java" lineNumbers="true" %}
```java
package frc.robot.subsystems;

import edu.wpi.first.wpilibj.xrp.XRPMotor;
import edu.wpi.first.wpilibj.Encoder;

public class Drivebase {  
  private final XRPMotor m_leftMotor;
  private final XRPMotor m_rightMotor;
  private final Encoder m_leftEncoder;
  private final Encoder m_rightEncoder;
  private final double wheelDiameter;
  private final double trackwidth;
  private final double countsPerWheelRev;
  private final double wheelCircumference;
  private final double convFactor;
  
  // Constructor
  public Drivebase() {  // Note: the constructor can also take inputs if needed
    m_leftMotor = new XRPMotor(0);
    m_rightMotor = new XRPMotor(1);
    m_leftEncoder = new Encoder(4, 5);
    m_rightEncoder = new Encoder(6, 7);
    wheelDiameter = 2.3622;  // inches
    trackwidth = 6.1;        // inches
    countsPerWheelRev = 585;
    wheelCircumference = Math.PI * wheelDiameter;  // inches
    convFactor = wheelCircumference / countsPerWheelRev;  // inches per pulse
  
    m_rightMotor.setInverted(true);
    resetEncoders();  // method from this class
    setEncoderDistPerPulse(convFactor);  // method and constant from this class
  }
  
  // Other methods below - not shown
}
```
{% endcode %}

## Updating the Robot File

Now that you have set up the `Drivebase` class, it is now time to update our `Robot` file to use the `Drivebase` class that we have created.  We begin by importing our custom-made class into the file.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" %}
```java
import frc.robot.subsystems.Drivebase;
```
{% endcode %}
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

Instead of creating variables for each of our motors and encoders, we can create a single `Drivebase` variable (which contains all of the motors and encoders within it).  We can then use the dot operator to call the appropriate methods from the `Drivebase` variable we have created.

A summary of changes:

1. Removed `XRPMotor` and `Encoder` imports
2. Added `Drivebase` import
3. Removed `XRPMotor` , `Encoder`, and numeric variables
4. Added `Drivebase` variable
5. `Robot()` constructor only initializes the servo
6. All other code is mostly the same.  We use the methods from the `Drivebase` by using the dot operator.
7. All created methods have been removed from `Robot.java` since they are now inside of `Drivebase.java`.

{% tabs %}
{% tab title="Java" %}
{% code title="Robot.java" %}
```java
package frc.robot;

import edu.wpi.first.wpilibj.TimedRobot;
import edu.wpi.first.wpilibj.XboxController;
import edu.wpi.first.wpilibj.xrp.XRPServo;

import frc.robot.subsystems.Drivebase;  // new import for our Drivebase

public class Robot extends TimedRobot {
  private final XboxController m_controller = new XboxController(0);
  private final XRPServo m_servo = new XRPServo(4);

  // Using our constructor to create a Drivebase and initialize it
  private final Drivebase m_drivebase = new Drivebase();
  
  public Robot() {
    m_servo.setPosition(0.5);
  }
  
  @Override
  public void autonomousInit() {
    m_drivebase.resetEncoders();
  }
  
  @Override
  public void autonomousPeriodic() {
    m_drivebase.driveUntilDist(5, 0.5);  // 5 inches at 50% speed
  }
  
  @Override
  public void teleopInit() {
    m_drivebase.resetEncoders();
  }
  
  @Override
  public void teleopPeriodic() {    
    // Servo code
    if (m_controller.getAButton()) {
      m_servo.setPosition(1.0);
    } else {
      m_servo.setPosition(0.5);
    }
    
    // Drivebase code
    // Initial press of any of the 3 buttons
    if (m_controller.getXButtonPressed() || m_controller.getLeftBumperButtonPressed() || m_controller.getRightBumperButtonPressed()) {
      m_drivebase.resetEncoders();
    } else if (m_controller.getXButton()) {  // holding x-button
      m_drivebase.driveUntilDist(5, 0.5);
    } else if (m_controller.getLeftBumperButton()) {  // holding left bumper
      m_drivebase.turnLeft(90);
    } else if (m_controller.getRightBumperButton()) { // holding right bumper
      m_drivebase.turnRight(90);
    } else {
      m_drivebase.tankDrive(-m_controller.getLeftY(), -m_controller.getRightY());
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
