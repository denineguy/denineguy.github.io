---
layout: post
title: "Object Oriented Programming - Ruby Classes and Objects"
date: 2014-06-16 15:37:03 -0400
comments: true
categories: 
languages: ruby
tags: object oriented programming, classes and objects
---

Ruby is an object oriented programming language modeled around objects rather than "actions" and data rather than logic. Ruby allows you to define a class that provides a blueprint for the construction of similar objects.
<!-- more -->
## What is a Class
A class is a way of organizing and providing a blueprint for producing objects with similar attributes and methods.  It should have a single responsibility and its purpose is to define the behavior of an object.  Ruby has several built in classes such as the String Class, Array Class, Integer Class, Hash Class and many more.  Ruby also allows one to define and create your own classes.

## Creating Classes 
A class is defined by the keyword class, the class name, a constant, beginning with a capital letter and followed by the word end. Below is an example of the syntax:

```ruby 
class Person
end
```

##Class Objects
An object is thought of as an instance of the class. in Ruby you can create an object or instance of the class by using the new method, as follow:
```ruby
girl = Person.new("Lisa")
boy = Person.new("Gene")
```

## Variable Scope
Ruby offers 4 types of variables

* Local Variables: Local variable are variables that are defined in a method and not available outside of the method.  Local variables begin with a lower case letter or \_.`local` or `_local`

* Instance Variables: Instance variables are available across methods for any particular instance or object. That means that instance variables change from object to object. Instance variables are preceded by the at sign (@) followed by the variable name. `@variable`

* Class Variables: Class variables are available across different objects. A class variable belongs to the class and is a characteristic of a class. They are preceded by the sign @@ and are followed by the variable name. `@@class`

* Global Variables: Class variables are not available across classes. If you want to have a single variable, which is available across classes, you need to define a global variable. The global variables are always preceded by the dollar sign ($). `$global`

## New vs Initialize method
Initialize is a method of the instance that is used to boot up the object once it has been created.  New is a method of the class and is used to create the instance.  When "new" is called it creates an instance of the class and triggers the initalize method, which tells the object what to do as soon as it is created.   So if I call Person.new it will trigger the initialize method and take in the person name and job and the person will automatically say Hello my name is Danielle.

```ruby
class Person
  def initialize(name)
    @name = name 
    @job = job
  end

  def speak
    puts "Hello my name is #{name}"
  end
end

girl = Person.new("Danielle, secretary")
```

## Instance vs Class Methods

Instance methods on the other hand are methods of the instance or object. The speak method above is an example of an instance method. In my example the Instance method defines what a person does when they speak.  Class methods are declared the same way as instance methods, except that they are prefixed by self, or the class name, followed by a period. These methods are executed at the Class level and may be called without an object instance. They cannot access instance variables but do have access to class variables. Below is an example of class methods

```ruby
class Person
  def Person.find_by_name
    #do something
  end
end
```
This is can also be written using self which is the preferred method for Ruby.

```ruby
class Person
  def self.find_by_age
    #do something 
  end
end
```

## Attribute Reader Write and Accessor Methods
An object's instance variables are its attributes, which distinguish one object from other objects of the same class. It is important to be able to write to and read these attributes; doing so requires methods called attribute accessors.  We can use as attr_reader to access a variable and and attr_writer to change it.

```ruby
class Person
  def initialize
    @name = name
    @job = job
  end

  def name=(value) #setter method, which sets the value
    @name = value  
  end

  def name  # getter method
    @name  
  end

  def job=(value) #setter method, which sets the value
    @job = value  
  end
```

This can also be written more concisely with attr_reader and attr_writer methods which automatically creates the setter and getter methods respectively.

```ruby
class Person
  attr_reader :name
  attr_writer :name, :job

  def initialize
    @name = name
    @job = job
  end

end
```

When there is a variable that is both an attr_reader and attr_writer it can be written even more succinctly with an attr_accessor method as follows.

```ruby
class Person
  attr_accessor :name
  attr_writer  :job

  def initialize
    @name = name
    @job = job
  end

end
```

##Inheritance
Inheritance is the process by which one class takes on the attributes and methods of another class, the parent class.  Inheritance creates a relationship such that if one object cannot respond to a message it delegates that message to another object.  It should be noted that Ruby does not support multiple inheritance and so a class in Ruby can have only one base or parent class. The The syntax for inheritance is the Derived class, the new class which inherits from the base or parent class. 

```ruby
class DerivedClass < BaseClass

end
```