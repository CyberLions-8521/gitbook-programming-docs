# XRPMotor API (2026.2.2)

> **WPILib API 2026.2.2** · Package `edu.wpi.first.wpilibj.xrp`

```
java.lang.Object
  └─ edu.wpi.first.wpilibj.xrp.XRPMotor
```

**All Implemented Interfaces:** `MotorController`

***

```java
public class XRPMotor extends Object implements MotorController
```

**XRPMotor.**

A SimDevice based motor controller representing the motors on an XRP robot.

***

## Constructor Summary

| Constructor               | Description             |
| ------------------------- | ----------------------- |
| `XRPMotor(int deviceNum)` | Constructs an XRPMotor. |

***

## Method Summary

| Modifier and Type | Method                            | Description                                                                           |
| ----------------- | --------------------------------- | ------------------------------------------------------------------------------------- |
| `void`            | `disable()`                       | Disable the motor controller.                                                         |
| `double`          | `get()`                           | Common interface for getting the current set speed of a motor controller.             |
| `boolean`         | `getInverted()`                   | Common interface for returning if a motor controller is in the inverted state or not. |
| `void`            | `set(double speed)`               | Common interface for setting the speed of a motor controller.                         |
| `void`            | `setInverted(boolean isInverted)` | Common interface for inverting direction of a motor controller.                       |
| `void`            | `stopMotor()`                     | Stops motor movement.                                                                 |

### Methods inherited from class `java.lang.Object`

`clone`, `equals`, `finalize`, `getClass`, `hashCode`, `notify`, `notifyAll`, `toString`, `wait`, `wait`, `wait`

### Methods inherited from interface `edu.wpi.first.wpilibj.motorcontrol.MotorController`

`setVoltage`, `setVoltage`

***

## Constructor Details

### XRPMotor

```java
public XRPMotor(int deviceNum)
```

Constructs an XRPMotor.

| Parameter   | Description        |
| ----------- | ------------------ |
| `deviceNum` | The motor channel. |

***

## Method Details

### set

```java
public void set(double speed)
```

_Description copied from interface: `MotorController`_

Common interface for setting the speed of a motor controller.

|                  |                                                                   |
| ---------------- | ----------------------------------------------------------------- |
| **Specified by** | `set` in interface `MotorController`                              |
| **Parameter**    | `speed` — The speed to set. Value should be between -1.0 and 1.0. |

***

### get

```java
public double get()
```

_Description copied from interface: `MotorController`_

Common interface for getting the current set speed of a motor controller.

|                  |                                                       |
| ---------------- | ----------------------------------------------------- |
| **Specified by** | `get` in interface `MotorController`                  |
| **Returns**      | The current set speed. Value is between -1.0 and 1.0. |

***

### setInverted

```java
public void setInverted(boolean isInverted)
```

_Description copied from interface: `MotorController`_

Common interface for inverting direction of a motor controller.

|                  |                                                            |
| ---------------- | ---------------------------------------------------------- |
| **Specified by** | `setInverted` in interface `MotorController`               |
| **Parameter**    | `isInverted` — The state of inversion; `true` is inverted. |

***

### getInverted

```java
public boolean getInverted()
```

_Description copied from interface: `MotorController`_

Common interface for returning if a motor controller is in the inverted state or not.

|                  |                                                                |
| ---------------- | -------------------------------------------------------------- |
| **Specified by** | `getInverted` in interface `MotorController`                   |
| **Returns**      | `isInverted` — The state of the inversion; `true` is inverted. |

***

### disable

```java
public void disable()
```

_Description copied from interface: `MotorController`_

Disable the motor controller.

|                  |                                          |
| ---------------- | ---------------------------------------- |
| **Specified by** | `disable` in interface `MotorController` |

***

### stopMotor

```java
public void stopMotor()
```

_Description copied from interface: `MotorController`_

Stops motor movement. Motor can be moved again by calling `set` without having to re-enable the motor.

|                  |                                            |
| ---------------- | ------------------------------------------ |
| **Specified by** | `stopMotor` in interface `MotorController` |
