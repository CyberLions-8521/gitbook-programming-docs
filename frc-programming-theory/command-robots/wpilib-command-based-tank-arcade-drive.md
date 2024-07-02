# WPILib Command Based Tank-Arcade Drive



{% tabs %}
{% tab title="Java" %}
```java
// TankDriveCommand.java
package frc.robot.commands;

import frc.robot.subsystems.Drivetrain;

import edu.wpi.first.wpilibj2.command.Command;
import java.util.function.Supplier;  // a Java data type for functions

public class TankDriveCommand extends Command {
  private final Drivetrain m_drivetrain;
  private final Supplier<Double> m_leftSpeedSupplier;
  private final Supplier<Double> m_rightSpeedSupplier
  
  public TankDriveCommand(Drivetrain iDrivetrain, 
                          Supplier<Double> iLeftSpeedSupplier,
                          Supplier<Double> iRightSpeedSupplier) {
    m_drivetrain = iDrivetrain;
    m_leftSpeedSupplier = iLeftSpeedSupplier;
    m_rightSpeedSupplier = iRightSpeedSupplier;
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
