---
layout: post
title: "Dinner Club"
date: 2015-06-17
categories: technical code
---

# Dinner Club Explanation

## Assignment Task

The program was built to keep track of a dinner club's events. Specifically, it stores and displays club history, as well as members and total member costs during the history of the Dinner Club.

## Classes
Three classes were used to accomplish the Dinner Club assignment: CheckSplitter, ClubEvent, and DinnerClub.

## CheckSplitter Class
The CheckSplitter class was created to split the costs of a given meal. It requires three parameters to split the costs of the meal:
- Total cost of the meal as an integer
- Tip percentage as an integer or float
- Number of people as an integer

## ClubEvent Class
The ClubEvent cost was created to gather and store information about a specific event that the Dinner Club holds.

### ClubEvent Instance Variables
- total_cost - Integer of the cost of the meal given by user
- tip_percentage - Integer or float given by user
- people - Array of members who attended the event given by user
- date - String given by user
- location - String given by user
- who_paid - String given by user
- per_person - Float calculated from a method
- total_cost_single_pay - Float calculated from a method
- attendees_hash - Hash created from a method. Key is member and value is cost the member paid.
- event_log - Hash created from a method. Key is date, value is an array. Array contains location at index 0 and the attendees_hash at index 1.

### ClubEvent Methods
The *define_costs_for_event* method creates a new CheckSplitter object and stores the split as the per_person instance variable. It also stores the total_cost_single_pay instance variable as the cost of the meal plus tip, to be used if one person treats the group.

The *store_attendees_and_costs* method creates and populates the attendees_hash.

~~~ruby
def store_attendees_and_costs(who_paid, people, per_person)
  @attendees_hash = {}
  if who_paid.downcase == "everyone"
    populate_attendees_hash(people, per_person)
  else
    populate_attendees_hash(people, 0)
    attendees_hash[who_paid] = total_cost_single_pay
  end
end
~~~

It's uses another utility method, the *populate_attendees_hash*, which iterates over the people array and populates the attendees_hash with members and their costs.  In the *store_attendees_and_costs* method, an if-else statement is added to determine if everyone paid, or a specific person treated the group. If everyone paid, the attendees_hash value is populated with the per_person costs. If one person paid, the attendees_hash is still populated with all attendees, but their value is set to 0. Then the value at the key of who_paid is modified to be the total_cost_single_pay.

Finally, ClubEvent has the *event_history_update* method, which is used to create the event_log hash.

~~~ruby
def event_history_update(date, location, attendees_hash)
  @event_log = {date => [location, attendees_hash]}
end
~~~

All of these methods are called when a new ClubEvent object is created, so the information is automatically created and stored.

## DinnerClub Class
The DinnerClub Class was created to track and display dinner club events. It displays the history of club, as well as members and the total costs they've paid.

### DinnerClub Instance Variables:
- running_balance - Hash. Member name as the key and the total costs the member has paid as the value
- club_history - Hash. Key is the date of an event, value is an array. The array contains the location at index 0, and another hash at index 1. The hash at index 1 holds the attendees of a specific event and how much they paid.

### DinnerClub Methods:
The *new_event* method is used every time the Dinner Club has a new event. It creates a new ClubEvent object and requires the same parameters that a ClubEvent object takes (total_cost, tip_percentage, people, date, location, who_paid). The *new_event* method then runs three other methods: the *club_history_update* method, the *running_balance_update* method, and the *display_running_balance* method.

The *club_history_update* method takes two parameters: club_history and event_log. club_history is the hash created when the DinnerClub object is created, and event_log comes from the *new_event* method, which calls the event_log instance variable from the ClubEvent class.

~~~ruby
def club_history_update(club_history, event_log)
  club_history.merge!(event_log)
end
~~~

The *club_history_update* method destructively merges the club_history hash with the event_log hash.

The *running_balance_update* method takes two parameters: the running_balance hash and the club_history hash. It's used to update the member name and their total cost paid throughout the history of the club.

~~~ruby
def running_balance_update(running_balance, club_history)
  club_history.each do |date, array|
    array[1].each do |name, amount|
      running_balance[name] += amount
    end
  end
end
~~~

The *running_balance_update* method works by iterating over the club_history hash, and then accessing the array stored as the value in the hash. It then takes that array and looks at index 1, where the attendees_hash was stored in the *new_event* method. From there, it iterates over the hash at index 1 and adds the name of each member as the key to running_balance and adds the cost each member paid to the current value of running_balance. The default value of running_balance is set to 0, so if there is a new member, it adds on to 0.

The *display_running_balance* method only takes the running_balance as a parameter and displays the member names and total costs paid.

~~~ruby
def display_running_balance(running_balance)
  puts "\n"
  puts "Member Name".ljust(20) + "Member Balance".rjust(10)
  35.times {print "-"}
  puts "\n"
  running_balance.each do |name, balance|
    puts "#{name}".ljust(20) + "$#{sprintf("%0.2f", balance)}".rjust(10)
  end
  puts "\n"
end
~~~

The method puts a blank line, then puts the header information to display like a table. It left and right justifies the headers. The method then prints a dash 35 times to create the look of a dashed line. Then another line break is entered to follow the dashed line. Finally, the method iterates over running_balance to display member name left justified, then balance, formatted to display as currency, right justified.

The final method in DinnerClub is *display_history*, and uses the club_history as the parameter. It works exactly the same as *display_running_balance*, but instead iterates over the club_history hash.

~~~ruby
club_history.each do |date, array|
  puts "#{date}\t#{array[0]}\t\t" + "#{array[1]}".rjust(25)
end
~~~

This displays three columns of information: date, location, and attendees_hash, which displays members and the costs they paid at the event.  This method is only displayed if called, and is not automatically ran anywhere in the DinnerClub Class.
