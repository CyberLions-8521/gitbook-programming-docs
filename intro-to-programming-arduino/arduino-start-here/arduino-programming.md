# Arduino Programming

## Arduino Program Structure

We will learn how to program by using Arduino and other electrical components.  In an Arduino program, there are three key places our code goes:

* **At the top of the program -** We import other libraries and declare variables here.
* **Inside of setup() -** In Arduino, `setup` is a special function that occurs exactly one time at the start of your program.  Usually, we put code that sets up our hardware here (e.g., setting up pins/ports, setting up initial configurations, etc.).
* **Inside of loop() -** In Arduino, `loop` is a special function that repeats forever.  You put the main code you want to run repeatedly within `loop`.

{% tabs %}
{% tab title="TinkerCAD Block Code" %}
The blocks can be found in the "Control" tab.

<div align="left"><figure><img src="../../.gitbook/assets/arduino_block_basic_structure.png" alt=""><figcaption><p>Basic Structure of Arduino Code in Blocks</p></figcaption></figure></div>
{% endtab %}

{% tab title="Arduino C++" %}
```arduino
// library imports and variables go here

void setup()
{
  // initial setup code goes here
}

void loop()
{
  // main code goes here
}
```
{% endtab %}
{% endtabs %}

## Arduino Programming Basics

To get started with programming our Arduino, we need to learn about three, built-in, Arduino functions.  These functions allow us to write code that interacts with the Arduino.

**pinMode(pin\_number, mode)**

Sets up the Arduino pin with the appropriate mode (either `INPUT` or `OUTPUT` ).  We typically put this within `setup` .  Pins set as output can send, or give off signals (i.e., voltage), and pins set as input can receive signals (e.g., from buttons).

**digitalWrite(pin\_number, signal)**

Tells the pin at `pin_number`  to give an output signal.  The signal can either be `HIGH` (on) or `LOW` (off).  The pin must have a pin mode of `OUTPUT` .

**delay(time\_ms)**

The `delay` function will pause the Arduino.  No additional code will run for the duration of the wait time and the code will get "stuck" at that line of code.  The wait time is given in milliseconds.

{% hint style="warning" %}
Using a `delay` will completely pause the Arduino code at that particular line of code.  No inputs will be read until _after_ the delay has passed.  This means that even if you are pressing a button, the button presses will not be registered by the code until the code finishes the delay and loops back to when it checks your button press.
{% endhint %}

{% tabs %}
{% tab title="TinkerCAD Blocks" %}
**pinMode(pin\_number, mode)**

{% hint style="info" %}
Note that when using TinkerCAD blocks, the block environment will use the `pinMode` function for you; there is no block for it, and it cannot be seen when programming in blocks
{% endhint %}

**digitalWrite(pin\_number, signal)**

<div align="left"><figure><img src="../../.gitbook/assets/arduino_block_digital_write.png" alt="" width="272"><figcaption><p>Block Version of Digital Write</p></figcaption></figure></div>

**delay(time\_ms)**

<div align="left"><figure><img src="../../.gitbook/assets/arduino_block_delay.png" alt="" width="272"><figcaption><p>Block Version of Delay</p></figcaption></figure></div>
{% endtab %}

{% tab title="Arduino C++" %}
**pinMode(pin\_number, mode)**

```arduino
void setup()
{
    pinMode(13, OUTPUT);    // sets up pin 13 as an output pin
}
```

**digitalWrite(pin\_number, signal)**

```arduino
void setup()
{
    pinMode(13, OUTPUT);
}

void loop()
{
    digitalWrite(13, HIGH);    // Sends a signal of HIGH through pin 13
}
```

**delay(time\_ms)**

```arduino
void loop()
{
    delay(1000);    // Wait for 1000 ms (or 1 second)
}
```
{% endtab %}
{% endtabs %}
