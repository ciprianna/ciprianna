---
layout: post
title: "CheckSplitter"
date: 2015-06-01
categories: technical code ruby omaha-code-school
---
# Assignment Task
The purpose of CheckSplitter is to split checks amongst individuals who have eaten a meal together, with a tip included.  There is only one Class used for this assignment: CheckSplitter.

# Arguments
CheckSplitter takes an options Hash as the argument, allowing for inputs to be entered out of order and default values to be set.  Within the options Hash, three pieces of information are used to run the program:

- total\_cost - Integer for the total cost of the meal
- tip\_percentage - Float for the percentage of tip the group is leaving; default value set to 0.15
- people - Integer for the number of people eating the meal

# Methods
The _tip\_percent_ method takes the options Hash, args, and reads the value of the tip\_percentage key. It converts it to a Float and stores it as a variable.

~~~ruby
def tip_percent(args)
  tip_percent_temp = args[:tip_percentage].to_f
  if tip_percent_temp >= 1
    @tip_percentage = (tip_percent_temp / 100.0)
  else
    @tip_percentage = tip_percent_temp
  end
end
~~~

The method then evaluates if the variable for tip\_percentage is greater than or equal to 1, and converts it to its corresponding decimal value if it is. The value, either converted or as was entered, is stored into an attribute variable called tip\_percentage, which is always a Float.

The _neg\_tip_ method evaluates the tip\_percentage variable, and sets it back to the default value if it entered in as a negative number by the user.

~~~ruby
def neg_tip(args)
  tip_percent_temp = args[:tip_percentage].to_f
  if tip_percent_temp < 0
    @tip_percentage = 0.15
  end
end
~~~

This allows the user to set a tip of 0, if they wanted, but it will not allow them to create a negative tip amount.

The _people\_count_ method works the same way as the _neg\_tip_ method, by evaluating if the number of people entered by the user was less than 1. If so, the method sets the number of people equal to 1.

The next method used is _total\_cost\_with\_tip_. It simply runs the calculations needed to determine the final cost of the meal with the tip included.

~~~ruby
def total_cost_with_tip
  total_cost + (total_cost * tip_percentage)
end
~~~

Finally, the _final\_per\_person_ method is used to determine the cost that each person should pay to split the check evenly.

~~~ruby
def final_per_person
  total_cost_with_tip / people
end
~~~

This is done by dividing the _total\_cost\_with\_tip_ return by the people variable.

# Testing CheckSplitter
To test this program, I used Minitest to ensure that the correct calculations were being made. This was accomplished with a series of "assert\_equal" tests.

For example, one test was to ensure that the _tip\_percentage_ method actually converted Integers to Floats, as expected.

~~~ruby
def test_tip_percentage
  new_check = CheckSplitter.new(tip_percentage: 15)

  assert_equal(0.15, new_check.tip_percent(new_check.args))
end
~~~

The test creates a new CheckSplitter object, using default values in the options Hash for total_cost and people variables. It then sets the value of tip\_percentage to 15. The test asserts that the expected result should be 0.15, and the way to test if that's true is by running the _tip\_percent_ method on the new_check Object created.  The test was successful.

These assertions were used to test all the methods used for CheckSplitter which calculated or changed a value.
