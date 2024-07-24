# XRP Project Setup

### Creating a New Project

See [WPILib: Create New Projects](../../getting-started/wpilib-vscode-how-to/wpilib-create-new-projects.md).

### Downloading XRP Vendor Library

We need to download an additional library specific to the XRP into our project so that our program has access to XRP specific things.  The XRP library needs to be downloaded into your WPILib project as an External 3rd Party Library.  See instructions for that in [WPILib: Download 3rd Party Vendor Libraries](../../getting-started/wpilib-vscode-how-to/wpilib-download-3rd-party-vendor-libraries.md).

### Simulating XRP Code

In order to use WPILib's built-in code simulator on the XRP, we need to add the following lines of code to the file named `build.gradle` after about line 80 where it says "Simulation configuration (e.g., environment variables)".

```gradle
//Sets the XRP Client Host
wpi.sim.envVar("HALSIMXRP_HOST", "192.168.42.1")
wpi.sim.addXRPClient().defaultEnabled = true
```
