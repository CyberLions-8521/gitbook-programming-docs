# XRP Command Robot Drive Program

## Tank Drive

### Introduction

This page will walk you through programming the XRP as a Command Robot using the Tank Drive operator control scheme.  This means that the left joystick of a gamepad will control the left wheel on the robot, and the right joystick of the gamepad will control the right wheel on the robot.

Note that using Arcade Drive as a control scheme takes only slight modification to the code.  Sample code for Arcade Drive is included at the bottom of this page.

### Code Setup

You should have already created and set up a WPILib project for your XRP robot.  If you have not already done so, follow the instructions in [Simple XRP Program Project Setup](../xrp-project-setup.md), and choose Command Robot Skeleton (Advanced) as your project template.

To understand the structure of a command robot, be sure to read [Structure of a Command Robot](../../../../frc-programming-theory/command-robots/structure-of-a-command-robot.md).  To better organize our code to align with the structure of a command robot, make a folder for your subsystems and for your commands within your project (either within `java/frc/robot` or within `src/main/cpp`).  Name the folders "subsystems" and "commands".

You can make new folders by right clicking where you want the folders to go within the File Explorer on the left sidebar of VSCode, and selecting "New Folder".

<figure><img src="../../../../.gitbook/assets/wpilib-create-new-folder.PNG" alt=""><figcaption><p>Creating a new folder within a WPILib Project</p></figcaption></figure>

We are going to represent our drivebase (or drivetrain) as a subsystem in our program.  Right click within your subsystems folder, and select "Create a new class/command".  Select "Subsystem" from the dropdown list that appears, and name it `Drivetrain` (case-sensitive).

We will preemptively make a tank drive command for our program as well.  Create a new class/command in your commands folder, and select "Command" from the dropdown list.  Name it `TankDriveCommand`.

<figure><img src="../../../../.gitbook/assets/wpilib-create-new-class-command.PNG" alt=""><figcaption></figcaption></figure>

### The Drivetrain Class

We will begin by creating our `Drivetrain` class.  All classes have attributes/fields (i.e., what the class _has_) as well as methods/behaviors (i.e., what the class _can do_).  In this case, our `Drivetrain` should _have_ two motors - one each for the left and right motors - and the behavior or action that it is _able to do_ is that it can drive using Tank Drive

{% @lucid/lucid-component url="https://lucid.app/lucidchart/d993916f-f833-429c-bead-09efd4c0ba77/edit?invitationId=inv_8a541e58-44ba-47f9-9c33-add839c0605b&viewport_loc=2371,772,947,516,HWEp-vi-RSFO" %}

Import the `XRPMotor` class into your program.

{% tabs %}
{% tab title="Java" %}
```java
import edu.wpi.first.wpilibj.xrp.XRPMotor;
```
{% endtab %}

{% tab title="C++ (Header)" %}
```cpp
#include <frc/xrp/XRPMotor.h>
```
{% endtab %}
{% endtabs %}

Within the class, create two `XRPMotor` objects, one for each of the left and right motors.  Within the `Drivetrain` constructor, initialize the left motor to port 0, the right motor to port 1, and set the inverted status of the right motor to `true` (as stated in the [WPILib XRP documentation](https://docs.wpilib.org/en/stable/docs/xrp-robot/getting-to-know-xrp.html)) using the `setInverted()` method of the `XRPMotor` class.

{% hint style="info" %}
Source code for the `XRPMotor` class can be found here ([Java](https://github.com/wpilibsuite/allwpilib/blob/main/xrpVendordep/src/main/java/edu/wpi/first/wpilibj/xrp/XRPMotor.java) / [C++](https://github.com/wpilibsuite/allwpilib/blob/main/xrpVendordep/src/main/native/cpp/xrp/XRPMotor.cpp)).
{% endhint %}

{% tabs %}
{% tab title="Java" %}
```java
// Drivetrain.java
package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.xrp.XRPMotor;  // import XRPMotor class

public class Drivetrain extends SubsystemBase {
  // Attributes
  private XRPMotor m_leftMotor;  // create left motor
  private XRPMotor m_rightMotor; // create right motor
  
  // Class Constructor
  public Drivetrain() {
    m_leftMotor = new XRPMotor(0);  // initialize left motor to port 0
    m_rightMotor = new XRPMotor(1); // initialize right motor to port 1
    m_rightMotor.setInverted(true); // invert the right motor
  }

  @Override
  public void periodic() {
    // This method will be called once per scheduler run
  }
}
```
{% endtab %}

{% tab title="C++ (Header)" %}
```cpp
// You only need to include the one you use
#include <frc/PS4Controller.h>
#include <frc/XboxController.h>
```
{% endtab %}
{% endtabs %}

We want our `Drivetrain` to be able to drive using Tank Drive.  We will make a method (function/procedure) that handles this for us.  The method will take two doubles as parameters: one for the left motor speed and one for the right motor speed, and set the speed values of the motors accordingly.

{% tabs %}
{% tab title="Java" %}
```java
// Drivetrain.java
package frc.robot.subsystems;

import edu.wpi.first.wpilibj2.command.SubsystemBase;
import edu.wpi.first.wpilibj.xrp.XRPMotor;

public class Drivetrain extends SubsystemBase {
  // Attributes
  private XRPMotor m_leftMotor;
  private XRPMotor m_rightMotor;
  
  // Class Constructor
  public Drivetrain() {
    m_leftMotor = new XRPMotor(0);
    m_rightMotor = new XRPMotor(1);
    m_rightMotor.setInverted(true);
  }
  
  /******** NEW CODE HERE ********/
  // Tank Drive method
  public void tankDrive(double leftSpeed, double rightSpeed) {
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
  }
  /*******************************/

  @Override
  public void periodic() {
    // This method will be called once per scheduler run
  }
}
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

### Tank Drive Command

We will now create the Tank Drive Command.  All commands require a subsystem.  In this case, the Tank Drive Command will require the `Drivetrain` subsystem.  Import the `Drivetrain` class into your `TankDriveCommand` file.

{% tabs %}
{% tab title="Java" %}
```java
import frc.robot.subsystems.Drivetrain;
```
{% endtab %}

{% tab title="C++ (Header)" %}
```cpp
#include <frc2/command/button/CommandXboxController.h>
```
{% endtab %}
{% endtabs %}

We want the class to have a `Drivetrain` object as an attribute, and we want to modify the constructor to take a `Drivetrain` object as parameter.  We must also tell the command which subsystem it is responsible for by using the built-in `addRequirements()` method (which `TankDriveCommand` inherits from the `Command` class).  We call the `addRequirements()` method in the class constructor so that the command knows which subsystem it is responsible for as soon as the command is created.

{% tabs %}
{% tab title="Java" %}
```java
// TankDriveCommand.java
package frc.robot.commands;

import frc.robot.subsystems.Drivetrain;  // import Drivetrain class

import edu.wpi.first.wpilibj2.command.Command;

public class TankDriveCommand extends Command {
  private final Drivetrain m_drivetrain;  // create Drivetrain attribute
  
  /** Modify constructor to take a Drivetrain as parameter
  * Note: I am using the "i" for input naming convention here
  **/
  public TankDriveCommand(Drivetrain iDrivetrain) {
    m_drivetrain = iDrivetrain;    // set this drivetrain to input drivetrain
    addRequirements(m_drivetrain); // add the drivetrain as the subsystem req.
  }

  /** Other methods not shown **/
}
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

In addition to a Drivetrain, the `TankDriveCommand` class also needs to have a controller object as an attribute (PS4 Controller, X Box Controller, or something else).  Import the appropriate class or header file, and add a controller to your class as shown below.  Note that we are modifying our constructor to take the controller as parameter as well.

{% hint style="info" %}
Alternatively, you can also give the class two functions as attributes.  You could even not make a `TankDriveCommand` class at all, and just set the default command directly within `RobotContainer` using lambda functions.  For a more in-depth explanation, see [WPILib Command Based Tank-Arcade Drive](../../../../frc-programming-theory/command-robots/wpilib-command-based-tank-arcade-drive.md).
{% endhint %}

{% tabs %}
{% tab title="Java" %}
```java
// TankDriveCommand.java
package frc.robot.commands;

import frc.robot.subsystems.Drivetrain;

// Import the CommandXboxController
import edu.wpi.first.wpilibj2.command.button.CommandXboxController;
import edu.wpi.first.wpilibj2.command.Command;

public class TankDriveCommand extends Command {
  private final Drivetrain m_drivetrain;
  private final CommandXboxController m_controller;  // create controller
  
  // Modify constructor to take controller as parameter
  public TankDriveCommand(Drivetrain iDrivetrain,
                          CommandXboxController iController) {
    m_drivetrain = iDrivetrain;
    m_controller = iController;  // set this controller to input controller
    addRequirements(iDrivetrain);
  }

  /** Other methods not shown **/
}
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

We can now finish our command by defining what the command does in its `execute()` method.  For this command, all we want it to do is to get the joystick values from the controller, and pass them as the speeds to the Drivetrain object's `tankDrive()` method.  Remember to negate the joystick y-values.

{% tabs %}
{% tab title="Java" %}
```java
// TankDriveCommand.java
package frc.robot.commands;

import frc.robot.subsystems.Drivetrain;

import edu.wpi.first.wpilibj2.command.button.CommandXboxController;
import edu.wpi.first.wpilibj2.command.Command;

public class TankDriveCommand extends Command {
  private final Drivetrain m_drivetrain;
  private final CommandXboxController m_controller;
  
  public TankDriveCommand(Drivetrain iDrivetrain,
                          CommandXboxController iController) {
    m_drivetrain = iDrivetrain;
    m_controller = iController;
    addRequirements(iDrivetrain);
  }
  
  /** NEW CODE HERE
  * Modify the execute() method
  **/
  @Override
  public void execute() {
    double leftSpeed = -m_controller.getLeftY();
    double rightSpeed = -m_controller.getRightY();
    m_drivetrain.tankDrive(leftSpeed, rightSpeed)
  }
  
  /** Other methods not shown **/
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Documentation for the `CommandXboxController` class can be found here ([Java](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/button/CommandXboxController.html) / [C++](https://github.wpilib.org/allwpilib/docs/release/cpp/classfrc2_1_1_command_xbox_controller.html))
{% endhint %}

### Robot Container

We can now finish our program by putting it all together in the `RobotContainer` file.  Import both the `Drivetrain` class and the `TankDriveCommand` class into your `RobotContainer`.  Also import a gamepad controller class.  After importing, give the `RobotContainer` both a `Drivetrain` and a controller object as attributes.

{% tabs %}
{% tab title="Java" %}
```java
// RobotContainer.java
package frc.robot;

// Import CommandXboxController
import edu.wpi.first.wpilibj2.command.button.CommandXboxController;
import edu.wpi.first.wpilibj2.command.Command;
import edu.wpi.first.wpilibj2.command.Commands;

import frc.robot.commands.TankDriveCommand;  // Import TankDriveCommand
import frc.robot.subsystems.Drivetrain;      // Import Drivetrain

public class RobotContainer {
  // Create Attributes
  private final Drivetrain m_drivetrain = new Drivetrain();
  private final CommandXboxController m_controller = new CommandXboxController(0);

  // Constructor
  public RobotContainer() {
    configureBindings();
  }

  private void configureBindings() {}

  public Command getAutonomousCommand() {
    return Commands.print("No autonomous command configured");
  }
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}
{% endtabs %}

Within the `configureBindings()` method, we will set the default command of the `Drivetrain` subsystem to be our `TankDriveCommand`.  Congratulations!  You can now simulate your code to check that it is working properly.

{% tabs %}
{% tab title="Java" %}
```java
// RobotContainer.java
private void configureBindings() {
    m_drivetrain.setDefaultCommand(
        new TankDriveCommand(m_drivetrain, m_controller));
}
```
{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}

{% hint style="info" %}
See [How to Simulate Code](../../../../getting-started/wpilib-vscode-how-to/wpilib-simulate-code.md).
{% endhint %}

***

## Arcade Drive

To program your robot using Arcade Drive, simply modify or add the code in the following files as shown below.

{% tabs %}
{% tab title="Java" %}
```java
// Note: Code for 3 separate files is shown below

/** Drivetrain.java
* Either modify tankDrive to become arcadeDrive, or
* create a new function for Arcade Drive
**/
public void arcadeDrive(double fwd, double turn) {
    double leftSpeed = fwd - turn;
    double rightSpeed = fwd + turn;
    m_leftMotor.set(leftSpeed);
    m_rightMotor.set(rightSpeed);
}

/** ArcadeDriveCommand.java
* Either modify TankDriveCommand or create a new Command file
**/
public void execute() {
    double fwd = -m_controller.getLeftY();
    double turn = -m_controller.getLeftX();
    m_drivetrain.arcadeDrive(fwd, turn);
}

/** RobotContainer.java **/
private void configureBindings() {
    m_drivetrain.setDefaultCommand(
        new ArcadeDriveCommand(m_drivetrain, m_controller));
}
```
{% endtab %}

{% tab title="C++ (Header)" %}

{% endtab %}

{% tab title="C++ (Source)" %}

{% endtab %}
{% endtabs %}
