---
layout: post
title: "Database Methods Modules"
date: 2015-06-18
categories: technical code ruby sql omaha-code-school
---
Two modules were created to deal with Databases in Ruby: Database Class Methods and Database Instance Methods. These modules are generic and can be used as the ORM for any database project in Ruby.  The modules require active\_support and assumes that Class Objects are initialized with an options Hash. It also assumes standard naming conventions are being followed between Class names and Table names.

## Database Class Methods Module
This module is created to be called on Classes. They deal with a table as a whole, not just a specific row.  

### all Method
The all method returns all rows of a table. It looks to self Class that the method is being called on, and returns the associated table name.

~~~ ruby
def all
  table = self.to_s.pluralize.underscore

  results = DATABASE.execute("SELECT * FROM #{table};")

  store_results = []

  results.each do |hash|
    store_results << self.new(hash)
  end

  return store_results

end
~~~

The method then uses the Database connection to call to SQL and return all rows in the given table. An empty Array is created, and the results are iterated over and pushed into the Array. Results, when pulled from SQL, return as an Array of Hashes. When the .new method is called, it takes the Hash of row information and creates an Object from it. This method returns an Array of Objects.

### find Method
The find method works identically to the all method, but it instead returns the row WHERE id is equal to a passed id.

### add Method
The add method takes a bit more work to run. It takes an Object that hasn't been included in the database yet and adds it to the database. It returns the Object with the id attached.

To accomplish this, a Hash is passed containing Object information. The keys and values from the Hash are stored into variables. Naming conventions are important here as well, as each attribute should match column names for the table.

~~~ruby
def add(options = {})

  columns = options.keys
  values = options.values

  columns_for_sql = columns.join(", ")
  individual_values_for_sql = []

  values.each do |value|
    if value.is_a?(String)
      individual_values_for_sql << "'#{value}'"
    else
      individual_values_for_sql << value
    end
  end

  values_for_sql = individual_values_for_sql.join(", ")

  table = self.to_s.pluralize.underscore

  DATABASE.execute("INSERT INTO #{table} (#{columns_for_sql}) VALUES (#{values_for_sql});")

  id = DATABASE.last_insert_row_id

  options["id"] = id

  self.new(options)
end
~~~

At this point, the columns are joined together into a comma-separated String. An empty Array is created, and each value from the passed Hash is iterated and pushed into the Array - with quotes for Strings and without quotes for all other Classes. That Array is then also joined into a comma-separated String.

From here, the appropriate data is included into the SQL query to add the new row. Returned from the query is the last\_insert\_row\_id, and that's stored into the Object's Hash. The Object is returned.

## Database Instance Methods
This class is used with specific Object in a Class. They should interact with a given row, rather than the entire table.

### get Method
This method returns a specific field value. It gets the table name, runs a query on the id attribute of the Object, and returns the value of the passed field.

~~~ruby
def get(field)
  table = self.to_s.pluralize.underscore

  result = DATABASE.execute("SELECT * FROM #{table} WHERE id = #{@id}").first

  result[field]
end
~~~

### delete Method
The delete method is fairly straightforward, deleting a row where the id matches the id attribute of the Object the method is called on.

~~~ruby
def delete
  table = self.class.to_s.pluralize.underscore

  DATABASE.execute("DELETE FROM #{table} WHERE id = #{@id};")
end
~~~

### save Method
Finally, the save method is generic to all classes, but involves a few steps.

First, the table is found using active\_support. The attributes of an Object are found using the .instance\_variables method.  An empty Hash is created to store each instance variable as a key and it's value as the value in the Hash. To do this, each instance variable must be sliced to remove the "@" sign from the beginning. The result is a Hash of the Object's attribute names and attribute values.

~~~ruby
def save
  table = self.class.to_s.pluralize.underscore

  instance_variables = self.instance_variables

  attribute_hash = {}

  instance_variables.each do |variable|
    attribute_hash["#{variable.slice(1..-1)}"] = self.send("#{variable.slice(1..-1)}")
  end

  individual_instance_variables = []

  attribute_hash.each do |key, value|
    if value.is_a?(String)
      individual_instance_variables << "#{key} = '#{value}'"
    else
      individual_instance_variables << "#{key} = #{value}"
    end
  end

  for_sql = individual_instance_variables.join(', ')

  DATABASE.execute("UPDATE #{table} SET #{for_sql} WHERE id = #{self.id}")

  return self
end
~~~

From there, the method then creates an empty Array and pushes each key-value pair from the attribute\_hash into it. If the value is a String, it adds quotes around the value before pushing it to the individual\_instance\_variables Array. Finally, for\_sql is created by joining the Array together to a comma-separated String. The String and table name are inserted into the SQL query to update the given table where the id of the Object equals id.  
