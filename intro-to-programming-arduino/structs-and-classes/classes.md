# Classes

## Class as an Extension of Structs

If you are coming from C or C++, a class can be thought of a struct that also has methods.  For example, if you had a struct which represented an RGB LED, instead of making a method that sets the color the LED as external to the struct and passing the RGB LED as a parameter, you could instead make the method "part of the struct" and use the parameter from the RGB LED itself.

{% tabs %}
{% tab title="Struct Pseudocode" %}
```cpp
struct RGBLed {
    // code not shown
};

// Note that the RGBLed is given to the method as parameter
void setColor(RGBLed led, double r, double g, double b) {
    // code not shown
}

RGBLed rgbLED = /* value not shown */;

void loop() {
    setColor(rgbLED, 255, 0, 0);    // the LED is red
}
```
{% endtab %}

{% tab title="Class Pseudocode" %}
```cpp
class RGBLed {
    // other code not shown
    
    // setColor is part of the class
    void setColor(double r, double g, double b) {
        // code not shown
    }
};

RGBLed rgbLED = /* value not shown */;

void loop() {
    // We do not need to give the RGBLed to the method as parameter
    // since we are setting the color of this particular RGBLed
    rgbLED.setColor(255, 0, 0);    // the LED is red
}
```
{% endtab %}
{% endtabs %}

## What is a Class

A class is a collection (bundle, or group) of code that defines a data type in much the same way that `int`, `double`, or `String`, define integer, decimal, and textual data types respectively.  A variable whose data type is a class data type is called an _object_ of the class, or _an instance of_ the class.  For example, if we had a class named `RGBLed`  that we used to represent RGB LEDs in our program code, then - in the same way that `int x` defines a variable named `x` of type `int` - the code `RGBLed myLed` defines a variable named `myLed` that is an _object_ of the `RGBLed` class (i.e., `myLed` is an instance of the `RGBLed` class.

The added advantage of a class is that a class data type can have multiple pieces of data within it, and can also have associated methods that can be used to access or modify the values of the data within the class.

## Making a Class - RGBLed

### Attributes and Access

We will create a class to represent RGB LEDs in our program code.  An RGB LED has three (3) programmable components that we care about: a pin number for the red leg, a pin number for the green leg, and a pin number for the blue leg.  We will create our class to bundle together these three pieces of data as `int` variables.  These are called the _class attributes_ (sometimes called _fields_, or _instance variables_).

C++ (as well as other programming languages such as Java) also allows you to control whether or not the user (or other programmers) have access to the attributes of the class.  Since we do not want (or need) the user to directly interface with the pin numbers of the RGB LED, we will make these attributes private.

```cpp
class RGBLed {
  private:
    int r;
    int g;
    int b;
};
```
