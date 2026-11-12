# Driving with Joysticks

## Starter Code

We will use the starter code found here: [https://github.com/CyberLions-8521/xrp-variables-functions-intro0](https://github.com/CyberLions-8521/xrp-variables-functions-intro0)

If you have not already, then clone the starter code repository to your computer by following the steps in [Getting Starter Code](../../getting-started/getting-started/getting-starter-code.md).

## Overview

This tutorial will have you using the joysticks to drive the XRP.  We will create variables to hold the joystick values, and use the joystick values to drive the robot.

## Driving Forwards and Backwards

We will use the "up and down" joystick (the W and S keys on the keyboard) to drive the robot forwards and backwards at different speeds.

<table><thead><tr><th width="170">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>drive(fwd, turn)</code></td><td>The robot drives forward with the effort specified by <code>fwd</code> and turns with the effort specified by <code>turn</code>.  The range of allowed values for <code>fwd</code> and <code>turn</code> are between <code>-1.0</code> and <code>1.0</code>.</td></tr><tr><td><code>getWSAxis()</code></td><td>Gets the value of the joystick represented by the W and S keys.  The joystick achieves values between <code>-1.0</code> and <code>1.0</code>.  Pushing "up" on the joystick (W) results in a negative value.  The joystick resets its value back to <code>0.0</code> when you let go of the buttons.</td></tr></tbody></table>

1. We will write our code within the `Robot.java` file.
2. We begin by creating a variable named `joystickValue` that is able to hold decimal values.  Make sure you create this variable above `teleopInit`.

{% hint style="info" %}
Technically, you can name the variable anything you want.  We (the author) simply decided to use `joystickValue`  as the name for our variable.
{% endhint %}

3. <pre class="language-java" data-title="Robot.java" data-line-numbers><code class="lang-java">double joystickValue;

   @Override
   public void teleopInit() {

   }

   @Override
   public void teleopPeriodic() {

   }
   </code></pre>
4. The logic of the code is:
   1. Store the value of the joystick into our variable.
   2. Use our variable to drive the robot.
   3. Since `teleopPeriodic` is constantly looping and repeating forever, it will constantly update the value of our `joystickValue` variable to the current value of the joystick (i.e., however far up or down we are pushing the joystick).
5. Complete the code as shown in Step 6, and simulate the code to test it on the XRP (see the instructions in [Driving the XRP](driving-the-xrp.md) or in [Running Code on an XRP)](../running-code-on-an-xrp.md).
6. <pre class="language-java" data-title="Robot.java" data-line-numbers><code class="lang-java">double joystickValue;

   @Override
   public void teleopInit() {

   }

   @Override
   public void teleopPeriodic() {
       joystickValue = getWSAxis();
       drive(joystickValue, 0.0);
   }
   </code></pre>

## Joystick Weirdness

If you ran the code from above, then you'll have noticed that pushing "up" on the joystick (the W key) causes the robot to drive **backwards.**

This is because, due to how joysticks are represented mathematically, pushing up on a joystick results in a negative value.

Fix the behavior of the robot by negating the joystick value before it gets used.

{% code title="Robot.java" lineNumbers="true" %}
```java
double joystickValue;

@Override
public void teleopInit() {

}

@Override
public void teleopPeriodic() {
    joystickValue = -getWSAxis();
    drive(joystickValue, 0.0);
}
```
{% endcode %}

## Extension

**Challenge** - Use the "left-right" joystick (A and D keys) for the turning effort for the robot.  Pushing left on the joystick (A) should cause the robot to turn counterclockwise.  You will need a second variable to hold the value of this second joystick.

<table><thead><tr><th width="187">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>drive(fwd, turn)</code></td><td>The robot drives forward with the effort specified by <code>fwd</code> and turns with the effort specified by <code>turn</code>.  The range of allowed values for <code>fwd</code> and <code>turn</code> are between <code>-1.0</code> and <code>1.0</code>.</td></tr><tr><td><code>getWSAxis()</code></td><td>Gets the value of the joystick represented by the W and S keys.  The joystick achieves values between <code>-1.0</code> and <code>1.0</code>.  Pushing "up" on the joystick (W) results in a negative value.  The joystick resets its value back to <code>0.0</code> when you let go of the buttons.</td></tr><tr><td><code>getADAxis()</code></td><td>Gets the value of the joystick represented by the A and D keys.  The joystick achieves values between <code>-1.0</code> and <code>1.0</code>.  Pushing "left" on the joystick (A) results in a negative value.  The joystick resets its value back to <code>0.0</code> when you let go of the buttons.</td></tr><tr><td><code>getERAxis()</code></td><td>Gets the value of the joystick represented by the E and R keys.  The joystick achieves values between <code>-1.0</code> and <code>1.0</code>.  Pressing R results in a positive value.  This joystick retains its value, and does not automatically reset to <code>0.0</code> when you let go of the buttons.</td></tr></tbody></table>

