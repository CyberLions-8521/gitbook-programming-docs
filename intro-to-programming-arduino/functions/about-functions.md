# Buzzer Circuit - Methods

## What are Methods

A _method_ (sometimes called a function) is a block of code with a name.  When you use a method (referred to as _calling_ the method, or _invoking_ the method), all the code within that method is executed.  It is similar to the concept of functions from your mathematics classes.

| Mathematical Representation                                                                          | Pseudocode Representation                                                                                                    |
| ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| <p><strong>Function Definition</strong><br><span class="math">f(x) = 2x+3</span></p>                 | <p><strong>Function Definition</strong><br>PROCEDURE f(x)<br>{<br>  MULTIPLY x BY 2<br>  ADD 3<br>  RETURN (result)<br>}</p> |
| <p><strong>Calling the Function</strong><br><span class="math">y = f(5)=2\cdot 5 + 3 = 13</span></p> | <p><strong>Calling the Method</strong><br><code>SET y TO f(5)</code></p>                                                     |

## Why use Methods

We can use methods to bundle together code statements that are performing largely the same task.  This makes it easier to read your code, debug your code (when properly done), and organize your code into groups that make intuitive sense.

As an intermediate level example, consider finding the average of a list of numbers.  A classic solution has you looping through the list, adding up all the values within the list, and then dividing your total by the length (or size) of the list.  Without a method, you need to duplicate the same code multiple times for each list you would like to find the average of.  With a method, you can simply call the method that computes the average of the list that was given to it.

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
