# About Functions

## What are Functions

A _function_ (sometimes called a method or procedure) is a block of code with a name.  When you use a function (referred to as _calling_ the function, or _invoking_ the function), all the code within that function is executed.  It is similar to the concept of functions from your mathematics classes.

| Mathematical Representation                                                                      | Sample Blocks                                                                                                          |
| ------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| <p><strong>Function Definition</strong><br><span class="math">f(x) = 2x+3</span></p>             | <p><strong>Function Definition</strong><br><img src="../../.gitbook/assets/block_function_example.png" alt=""></p>     |
| <p><strong>Calling the Function</strong><br><span class="math">f(5)=2\cdot 5 + 3 = 13</span></p> | <p><strong>Calling the Method</strong><br><img src="../../.gitbook/assets/block_function_call_example.png" alt=""></p> |

## Why use Functions

We can use function to bundle together code statements that are performing largely the same task.  This makes it easier to read your code, debug your code (when properly done), and organize your code into groups that make intuitive sense.

## How to Create Functions

Recall that to create variables, we needed to give the data type of the variable, and the variable name (and optionally, the value of the variable).  The general pattern was this

```
dataType variableName = value;
```

For functions, the pattern is similar.  We start with "the data type of the function", followed by the function name, a parameter list (which can be empty), and then the function body.  The general pattern is shown below.

{% hint style="info" %}
The "data type of the function" is known as the "return type", and the function body is the code that the function runs when it is called.
{% endhint %}

```cpp
// Function with no parameters
returnType funcName()
{
    functionBody
}

// Function with parameters
returnType funcName(parameterType parameterName, ...)
{
    functionBody
}
```

## Functions without Parameters

### Getting Started

We will create a program that plays the song "Mary Had a Little Lamb".  You can see the notes for it below.

<div align="left"><figure><img src="../../.gitbook/assets/mary_little_lamb_notes.png" alt=""><figcaption></figcaption></figure></div>

### Circuit

Our program will play the song through a buzzer when a button is pressed.  Below is a sample circuit diagram as well as a schematic.

{% hint style="warning" %}
We suggest using a resistor with your buzzer so that you do not accidentally blow it out (damage it) when using it.  A 220 ohm resistor should be good enough.  Stronger resistors will cause the buzzer to be softer.  You can also switch your power line to 3.3V instead of 5V to get a softer sound as well.
{% endhint %}

<figure><img src="../../.gitbook/assets/buzzer_button_circuit_diagram.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/buzzer_button_circuit_schematic.png" alt=""><figcaption></figcaption></figure>

### Program

Before we get into using functions, here is a snippet of what the program _could potentially_ look like if we did not use functions.

```cpp
#include <Arduino.h>    // only needed if programming on mBlock

// creating variables for the notes measured in Hz
int NOTE_C = 262;
int NOTE_D = 294;
int NOTE_E = 330;
int NOTE_G = 392;

// variables to keep track of beat and tempo
int DURATION = 250;
int BEAT = 300;
int REST = 800;

int buzzer = 13;
int button = 8;

void setup()
{
    pinMode(buzzer, OUTPUT);
    pinMode(button, INPUT);
}

void loop()
{
    if (digitalRead(button))
    {
        // mary had a little lamb
        tone(buzzer, NOTE_E4, DURATION);
        delay(300);
        tone(buzzer, NOTE_D4, DURATION);
        delay(300);
        tone(buzzer, NOTE_C4, DURATION);
        delay(300);
        tone(buzzer, NOTE_D4, DURATION);
        delay(300);
        tone(buzzer, NOTE_E4, DURATION);
        delay(300);
        tone(buzzer, NOTE_E4, DURATION);
        delay(300);
        tone(buzzer, NOTE_E4, DURATION);
        delay(800);
        
        // little lamb, little lamb
        tone(buzzer, NOTE_D4, DURATION);
        delay(300);
        tone(buzzer, NOTE_D4, DURATION);
        delay(300);
        tone(buzzer, NOTE_D4, DURATION);
        delay(800);
        tone(buzzer, NOTE_E4, DURATION);
        delay(300);
        tone(buzzer, NOTE_G4, DURATION);
        delay(300);
        tone(buzzer, NOTE_G4, DURATION);
        delay(800);
        
        // more code below for the rest of the song...
    }
}      
```

Already, we can see that the program is going to get very, very long, and we have not even finished the first verse (the song actually has multiple verses: "everywhere that Mary went...", "it followed her to school one day...", "it made the children laugh and play...").  It is also difficult to tell what the program is doing when the button is pressed.  We know that it is playing some tones, but nothing beyond that.

Here is what the program could look like if we start grouping parts of the verses into functions.

<pre class="language-cpp"><code class="lang-cpp">#include &#x3C;Arduino.h>    // only needed if programming on mBlock

// creating variables for the notes measured in Hz
int NOTE_C = 262;
int NOTE_D = 294;
int NOTE_E = 330;
int NOTE_G = 392;

// variables to keep track of beat and tempo
int DURATION = 250;
int BEAT = 300;
int REST = 800;

int buzzer = 13;
int button = 8;

void setup()
{
  pinMode(buzzer, OUTPUT);
  pinMode(button, INPUT);
}

void loop()
{
  if (digitalRead(button))
  {
    maryHad();
    littleLamb();
    maryHad();
    whiteAsSnow();
  }
}

/* mary had a little lamb */
void maryHad()
{
  tone(buzzer, NOTE_E4, DURATION);
  delay(300);
  tone(buzzer, NOTE_D4, DURATION);
  delay(300);
  tone(buzzer, NOTE_C4, DURATION);
  delay(300);
  tone(buzzer, NOTE_D4, DURATION);
  delay(300);
  tone(buzzer, NOTE_E4, DURATION);
  delay(300);
  tone(buzzer, NOTE_E4, DURATION);
  delay(300);
  tone(buzzer, NOTE_E4, DURATION);
  delay(800);
}

/* little lamb, little lamb */
<strong>void littleLamb()
</strong>{
  tone(buzzer, NOTE_D4, DURATION);
  delay(300);
  tone(buzzer, NOTE_D4, DURATION);
  delay(300);
  tone(buzzer, NOTE_D4, DURATION);
  delay(800);
  tone(buzzer, NOTE_E4, DURATION);
  delay(300);
  tone(buzzer, NOTE_G4, DURATION);
  delay(300);
  tone(buzzer, NOTE_G4, DURATION);
  delay(800);
}

/* whose fleece was white as snow */
void whiteAsSnow()
{
  tone(buzzer, NOTE_E4, DURATION);
  delay(300);
  tone(buzzer, NOTE_D4, DURATION);
  delay(300);
  tone(buzzer, NOTE_D4, DURATION);
  delay(300);
  tone(buzzer, NOTE_E4, DURATION);
  delay(300);
  tone(buzzer, NOTE_D4, DURATION);
  delay(300);
  tone(buzzer, NOTE_C4, DURATION);
  delay(800);
}
</code></pre>

As an intermediate level example, consider finding the average of a list of numbers.  A classic solution has you looping through the list, adding up all the values within the list, and then dividing your total by the length (or size) of the list.  Without a function , you need to duplicate the same code multiple times for each list you would like to find the average of.  With a function, you can simply call the function that computes the average of the list that was given to it.

The examples below are written in pseudocode, and can represent any programming language.

{% tabs %}
{% tab title="Without Methods" %}
```scala
SET listA to [1, 3, 5, 7]
SET listB to [2, 4, 6, 8]

// Finding average for listA
SET total TO 0
FOR EACH num IN listA
{
    ADD num TO total
}
SET avg1 TO (total / LENGTH(listA))

// Doing the same exact thing for listB
SET total TO 0
FOR EACH num IN listB
{
    ADD num TO total
}
SET avg2 TO (total / LENGTH(listA))
```
{% endtab %}

{% tab title="With Methods" %}
```scala
// Defining the method
PROCEDURE calcAverage(inputList)
{
    SET total TO 0
    FOR EACH num IN inputList
    {
        ADD num TO total
    }
    SET avg TO (total / LENGTH(inputList))
    RETURN avg
}

// Using the method
SET listA TO [1, 3, 5, 7]
SET listB TO [2, 4, 6, 8]

SET avg1 TO calcAverage(listA)
SET avg2 TO calcAverage(listB)
```
{% endtab %}
{% endtabs %}
