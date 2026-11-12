# Driving with Buttons

## Starter Code

We will use the starter code found here: [https://github.com/CyberLions-8521/xrp-variables-functions-intro0](https://github.com/CyberLions-8521/xrp-variables-functions-intro0)

If you have not already, then clone the starter code repository to your computer by following the steps in [Getting Starter Code](../../getting-started/getting-started/getting-starter-code.md).

## Overview

This tutorial will have you using the Z, X, C, and V keys on the keyboard to have the XRP do different things based on which button is being pressed.

## Part 1: Drive or Stop

We will write code so that whenever we are pressing the Z key on our keyboard, the robot will drive forward.  The robot stops when we are not pressing the Z key.

<table><thead><tr><th width="153">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>driveForward()</code></td><td>The robot drives forward.</td></tr><tr><td><code>stop()</code></td><td>The robot stops driving.</td></tr><tr><td><code>getZButton()</code></td><td>Check whether the Z key is being pressed.  The value is <code>true</code> if it is being pressed, and <code>false</code> when it is not being pressed.</td></tr></tbody></table>

1. We will write our code within the `Robot.java` file.
2. The logic of the code is:
   1. If the Z key is being pressed (its value is `true`), then the XRP will drive forward.
   2. Otherwise (or else) it will stop.
3. Copy the code below, and simulate the code to test it on the XRP (see the instructions in [Driving the XRP](driving-the-xrp.md) or in [Running Code on an XRP)](../running-code-on-an-xrp.md).
4. <pre class="language-java" data-title="Robot.java" data-line-numbers><code class="lang-java">@Override
   public void teleopInit() {

   }

   @Override
   public void teleopPeriodic() {
       if (getZButton())
       {
           driveForward();
       }
       else
       {
           stop();
       }
   }
   </code></pre>

## Part 2: More Buttons

What if we wanted to incorporate more buttons?  We can extend our logic using an `if-else if` chain.

The code below does the following:

* If the Z key is being pressed, then the robot drives forward.
* Otherwise, if the X key is being pressed, then the robot drives in reverse.
* Otherwise (if no key is being pressed), then the robot stops.

{% code title="Robot.java" lineNumbers="true" %}
```java
@Override
public void teleopInit() {

}

@Override
public void teleopPeriodic() {
    if (getZButton())
    {
        driveForward();
    }
    else if (getXButton())
    {
        driveReverse();
    }
    else
    {
        stop();
    }
}
```
{% endcode %}

## Extension

**Challenge** - Adding to the `if-else if` chain, make the robot do different things when each key is pressed (Z, X, C, V).

<table><thead><tr><th width="187">Function</th><th>Description</th></tr></thead><tbody><tr><td><code>driveForward()</code></td><td>The robot drives forward.</td></tr><tr><td><code>driveReverse()</code></td><td>The robot drives backwards.</td></tr><tr><td><code>drive(fwd, turn)</code></td><td>The robot drives forward with the effort specified by <code>fwd</code> and turns with the effort specified by <code>turn</code>.  The range of allowed values for <code>fwd</code> and <code>turn</code> are between <code>-1.0</code> and <code>1.0</code>.</td></tr><tr><td><code>stop()</code></td><td>The robot stops driving.</td></tr><tr><td><code>getZButton()</code></td><td>Check whether the Z key is being pressed.  The value is <code>true</code> if it is being pressed, and <code>false</code> when it is not being pressed.</td></tr><tr><td><code>getXButton()</code></td><td>Check whether the X key is being pressed.  The value is <code>true</code> if it is being pressed, and <code>false</code> when it is not being pressed.</td></tr><tr><td><code>getCButton()</code></td><td>Check whether the C key is being pressed.  The value is <code>true</code> if it is being pressed, and <code>false</code> when it is not being pressed.</td></tr><tr><td><code>getVButton()</code></td><td>Check whether the V key is being pressed.  The value is <code>true</code> if it is being pressed, and <code>false</code> when it is not being pressed.</td></tr></tbody></table>
