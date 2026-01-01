# XRP Project Setup

## CyberLions Starter Code

You can clone a repository of starter code with this link here.  Once cloned, navigate to the `xrp-timed-template`  folder.

[https://github.com/CyberLions-8521/wpilib-starter-v2025.3.2](https://github.com/CyberLions-8521/wpilib-starter-v2025.3.2)

***

## Manually Making an XRP Project

Skip this whole section if you are using the starter code.

### Creating a New Project

See [WPILib: Create New Projects](../../../getting-started/wpilib-vscode-how-to/wpilib-create-new-projects.md).  To best facilitate learning, it is recommended to use the Timed Skeleton (Advanced) project template.  You can use the XRP template, but this template comes pre-built with the entire XRP drivebase already programmed.

{% hint style="info" %}
The XRP vendor library and code simulation are already included if you use the XRP specific template from the WPILib.  You only need to follow the steps below if using the Timed Skeleton (Advanced) template.
{% endhint %}

### Downloading XRP Vendor Library

We need to download an additional library specific to the XRP into our project so that our program has access to XRP specific things.  The XRP library needs to be downloaded into your WPILib project as an External 3rd Party Library.  See instructions for that in [WPILib: Download 3rd Party Vendor Libraries](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html).

### Simulating XRP Code

In order to use WPILib's built-in code simulator on the XRP, we need to add the following lines of code to the file named `build.gradle` after about line 80 where it says `Simulation configuration (e.g., environment variables)`.

```gradle
//Sets the XRP Client Host
wpi.sim.envVar("HALSIMXRP_HOST", "192.168.42.1")
wpi.sim.addXRPClient().defaultEnabled = true
```
