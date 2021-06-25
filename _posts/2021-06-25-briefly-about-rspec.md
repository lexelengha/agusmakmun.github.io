---
layout: post
title:  "Briefly about RSpec"
date:   2021-06-25 20:00:00 +0200
categories: []
---

`RSpec` - is a unit testing module for the Ruby programming language. `RSpec` is different from traditional `xUnit` frameworks like `JUnit` because `RSpec` is a behavior driven development tool. This means that tests written in `RSpec` focus on the "behavior" of the application under test. `RSpec` focuses not on how an application works, but on how it behaves, in other words, what the application actually does. This tutorial will show you how to use `RSpec` to test your code when building Ruby applications.

Since `RSpec` is a BDD testing tool, the goal is to focus on what the application is doing and whether it conforms to the specification. In behavior-driven development, the specification is often described in terms of "user story". `RSpec` is designed to make it clear if the target code is working correctly, in other words, following the specification.

Let's test the functionality of the HelloWorld class. This is of course a very simple class that only contains one say_hello () method, but here is the basic RSpec code:

```ruby
describe HelloWorld do 
   context "When testing the HelloWorld class" do 
      
      it "The say_hello method should return 'Hello World'" do 
         hw = HelloWorld.new 
         message = hw.say_hello 
         expect(message).to eq "Hello World!" 
      end
      
   end 
end
```
The word description is an `RSpec` keyword. It is used to define a "group of examples". You can think of an Example Group as a collection of tests. The `description` keyword can take a class name and / or a string argument. You also need to pass a block argument for the description, which will contain the individual tests or, as `RSpec` knows, "Examples". A block is simply a Ruby block denoted by the Ruby `do` / `end` keywords.

The contextual keyword is similar to the description. It can also take a class name and / or a string argument. You should also use a context block. The idea behind the context is that it includes tests of a certain type.

```ruby
context "When passing bad parameters to the foobar() method"
context "When passing valid parameters to the foobar() method"
context "When testing corner cases with the foobar() method"
```

The `context` keyword is optional, but it helps add more detail about the examples it contains.

Word is another `RSpec` keyword that is used to define "example". An example is basically a test or test case. Again, like `description` and `context`, it accepts both a class name and string arguments and must be used with a block argument denoted by `do` / `end`. In this case, it is customary to pass only the string and the block argument. The string argument often uses the word "should" and is intended to describe what specific behavior should occur within an `it` block. In other words, it describes the expected result for an example.

```ruby
it "The say_hello method should return 'Hello World'" do
```
The line makes it clear what should happen when we call the `hello` command on an instance of the `HelloWorld` class. This is part of the `RSpec` philosophy, An example is not just a test, it is also a specification (specification). In other words, the Example documents and tests the expected behavior of your `Ruby` code.

```ruby
expect(message).to eql "Hello World!"
```
The idea with expected statements is that they read like normal English. You can say this out loud as `Expect the variable message to be equal to the string "Hello World" `. The idea is that it is descriptive and also easy to read, even for non-technical stakeholders such as project managers.


