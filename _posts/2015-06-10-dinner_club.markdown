---
layout: post
title: "Dinner Club"
date: 2015-06-10
categories: technical code ruby omaha-code-school
---

# Assignment Task
The program was built to keep track of a dinner club's events. Specifically, it stores and displays club history, as well as members and total member costs during the history of the Dinner Club.

# Classes
Three classes were used to accomplish the Dinner Club assignment: CheckSplitter, ClubEvent, and DinnerClub.

# CheckSplitter Class
The CheckSplitter class was created to split the costs of a given meal. It requires three parameters to split the costs of the meal:

- total_cost - Integer for the cost of the meal
- tip_percentage - Integer or Float for the percentage of tip
- people - Integer for the number of people attending the dinner

# ClubEvent Class
The ClubEvent cost was created to gather and store information about a specific event that the Dinner Club holds.

#### ClubEvent Instance Variables

- total_cost - Integer of the cost of the meal given by user
- tip_percentage - Integer or float given by user
- people - Array of members who attended the event given by user
- date - String given by user
- location - String given by user
- who_paid - String given by user
- per_person - Float calculated from a method
- total\_cost\_single_pay - Float calculated from a method
- attendees_hash - Hash created from a method. Key is member and value is cost the member paid.
- event\_log - Hash created from a method. Key is date, value is an Array. Array contains location at index 0 and the attendees_hash at index 1.

#### ClubEvent Methods
The "define\_costs\_for\_event" method creates a new CheckSplitter object and stores the split as the per\_person instance variable. It also stores the total\_cost\_single\_pay instance variable as the cost of the meal plus tip, to be used if one person treats the group.

The "store\_attendees\_and\_costs" method creates and populates the attendees\_hash.

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

This method uses another utility method, the "populate\_attendees\_hash", which iterates over the people Array and populates the attendees\_hash with members and their costs.  In the "store\_attendees\_and\_costs" method, an if-else statement is added to determine if everyone paid, or a specific person treated the group. If everyone paid, the attendees\_hash value is populated with the per_person costs. If one person paid, the attendees\_hash is still populated with all attendees, but their value is set to 0. Then the value at the key of who\_paid is modified to be the total\_cost\_single\_pay.

Finally, ClubEvent has the "event\_history\_update" method, which is used to create the event\_log Hash.

~~~ruby
def event_history_update(date, location, attendees_hash)
  @event_log = {date => [location, attendees_hash]}
end
~~~

All of these methods are called when a new ClubEvent object is created, so the information is automatically created and stored.

# DinnerClub Class
The DinnerClub Class was created to track and display dinner club events. It displays the history of club, as well as members and the total costs they've paid.

#### DinnerClub Instance Variables:
- running\_balance - Hash. Member name as the key and the total costs the member has paid as the value
- club\_history - Hash. Key is the date of an event, value is an Array. The Array contains the location at index 0, and another Hash at index 1. The Hash at index 1 holds the attendees of a specific event and how much they paid.

#### DinnerClub Methods:
The "new\_event" method is used every time the Dinner Club has a new event. It creates a new ClubEvent object and requires the same parameters that a ClubEvent object takes (total\_cost, tip\_percentage, people, date, location, who\_paid). The "new\_event" method then runs three other methods: the "club\_history\_update" method, the "running\_balance\_update" method, and the "display\_running\_balance" method.

The "club\_history\_update" method takes two parameters: club\_history and event\_log. club\_history is the Hash created when the DinnerClub object is created, and event\_log comes from the "new\_event" method, which calls the event\_log instance variable from the ClubEvent class.

~~~ruby
def club_history_update(club_history, event_log)
  club_history.merge!(event_log)
end
~~~

The "club\_history\_update" method destructively merges the club\_history Hash with the event\_log Hash.

The "running\_balance\_update" method takes two parameters: the running\_balance Hash and the club\_history Hash. It's used to update the member name and their total cost paid throughout the history of the club.

~~~ruby
def running_balance_update(running_balance, club_history)
  club_history.each do |date, array|
    array[1].each do |name, amount|
      running_balance[name] += amount
    end
  end
end
~~~

The "running\_balance\_update" method works by iterating over the club\_history Hash, and then accessing the Array stored as the value in the Hash. It then takes that Array and looks at index 1, where the attendees\_hash was stored in the "new\_event" method. From there, it iterates over the Hash at index 1 and adds the name of each member as the key to running\_balance and adds the cost each member paid to the current value of running\_balance. The default value of running\_balance is set to 0, so if there is a new member, it adds on to 0.

The "display\_running\_balance" method only takes the running\_balance as a parameter and displays the member names and total costs paid.

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

The method puts a blank line, then puts the header information to display like a table. It left and right justifies the headers. The method then prints a dash 35 times to create the look of a dashed line. Then another line break is entered to follow the dashed line. Finally, the method iterates over running\_balance to display member name left justified, then balance, formatted to display as currency, right justified.

The final method in DinnerClub is "display\_history", and uses the club\_history as the parameter. It works exactly the same as "display\_running\_balance", but instead iterates over the club\_history Hash.

~~~ruby
club_history.each do |date, array|
  puts "#{date}\t#{array[0]}\t\t" + "#{array[1]}".rjust(25)
end
~~~

This displays three columns of information: date, location, and attendees\_hash, which displays members and the costs they paid at the event.  This method is only displayed if called, and is not automatically ran anywhere in the DinnerClub Class.
