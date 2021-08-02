---
layout: post
title:  "Mocking with RSpec: Doubles and Expectations"
date:   2021-08-02 17:30:00 +0200
categories: [Rspec, mocks]
---

Mocking is a technique in test-driven development (TDD) that involves using fake dependent objects or methods in order 
to write a test. There are a couple of reasons why you may decide to use mock objects:

  * As a replacement for objects that don’t exist yet.
  * When you are working with objects which return non-deterministic values or depend on an external resource, e.g. a
  method that returns an RSS feed from a server.
  * To avoid setting up a complex scheme of data or dependency objects in order to write a test.
  * To avoid invoking code which would degrade the performance of the test, while at the same time being unrelated to
  the test you are writing.

The first reason is particularly prevalent among those who practice `behavior-driven development (BDD)`. The dependent 
object needs to exist, but it’s not what you’re currently working on. You can take advantage of the complete freedom of 
defining its interface to make it as nice and clear as possible, which is usually equivalent to being the simplest 
thing to write in the ongoing test.

By mocking objects in advance, you can allow yourself to focus on the thing that you’re working on at the moment. Let’s 
say that you are working on a new part of the system, and you realize that the code you’re currently describing and 
implementing will require two new collaborating objects. Using mocks, you can define their interfaces as you write a 
spec for the code you’re currently working on.

That way, you maintain a clean environment by having all your tests pass, before moving on to implement the 
collaborating objects. Without mocks, you’d be required to immediately jump to writing the implementation for the 
collaborating objects, before having your tests pass. This can be distracting and may lead to poor code design 
decisions. Mocking helps us by reducing the number of things we need to keep in our head at a given moment.

Mocking with RSpec is done with the `rspec-mocks` gem. If you have `rspec` as a dependency in your `Gemfile`, you 
already have `rspec-mocks` available.

# Doubles

A test double is a simplified object which takes the place of another object in a test. Creating a double with `RSpec` 
is easy:

```ruby
feed = double

# Optionally, you may give your double an identifier, which may come handy when debugging and inspecting objects:
feed = double("feed")
```
# Method Stubs

A new double resembles a plain Ruby `Object` — it’s not very useful on its own. It is usually the first step before 
defining some fake methods on it. This is called method stubbing, and with `RSpec` 3 it is done using the `allow()` and 
`receive()` methods:

```ruby
allow(feed).to receive(:fetch).and_return("imagine I'm a JSON string")

feed.fetch
=> "imagine I'm a JSON string"
```

The value provided to `and_return()` defines the return value of the stubbed method. The use of `and_return()` is 
optional, and if you don’t have it, the stubbed method will be set to return `nil`.

You can also apply `allow` to a real object (which is not a double). When testing `Rails` applications, for example, it 
is common to mock a method which works with the database and make it return a predefined double whenever the focus of 
the code is not whether the database operations work or not (and the test can run faster too).

```ruby
comment = double("comment")
expect(Comment).to receive(:find).and_return(comment)
```

# Message Expectations

Expecting messages — that is, defining expectations on test doubles that certain methods will be invoked after some 
code that follows runs — is a common pattern when working with doubles.

For example, let’s say that we’re working on a background job class whose job is to send an email via a mailer class. 
In BDD mocking style, we would write a spec for it by setting message expectations.

First, we know that our class needs to work with email records, available through a Rails model called Email. So, our 
first step is to define that our job class begins its work by finding the right email record based on an ID it receives 
as an argument.

```ruby
describe EmailVerificationJob do
  describe "#perform" do

    it "finds the email by id" do
      expect(Email).to receive(:find).with(12)

      EmailVerificationJob.new.perform(12)
    end
  end
end
```

In the code above, we set an expectation that if we run `EmailVerificationJob.perform(12)`, its implementation will 
call `Email.find(12)`. We can make that pass easily with the following implementation:

```ruby
class EmailVerificationJob
  def perform(email_id)
    email = Email.find(email_id)
  end
end
```

Next, we want to say that a mailer class should take the email record returned by `Email#find` and use it to send an 
actual email.

In order to glue the two actions together, we need to mock `Email#find` to return a double which we control. Then, we 
can use that double to set an expectation that a mailer class is using it correctly to send an email.

```ruby
describe EmailVerificationJob do
  describe "#perform" do

    it "finds the email by id" { ... }

    it "sends the verification email" do
      email = double
      allow(Email).to receive(:find) { email }

      expect(UserMailer).to receive(:send_verification_email).with(email)

      EmailVerificationJob.new.perform(12)
    end
  end
end
```

Note how we can compose `receive` with `with` to make sure that the `send_verification_email` method is being passed the right parameter. RSpec supports many ways to write argument matchers.

The final implementation of our job class should be simple, as follows:

```ruby
class EmailVerificationJob
  include Sidekiq::Worker

  def perform(email_id)
    email = Email.find(email_id)
    UserMailer.send_verification_email(email)
  end
end
```

However, when we run our spec, our first example fails:

```
$ bundle exec rspec
...
EmailVerificationJob
  #perform
    finds the email by id (FAILED - 1)
    sends the verification email

Failures:

  1) EmailVerificationJob#perform finds the email by id
     Failure/Error: EmailVerificationJob.new.perform(12)
     NoMethodError:
       undefined method `send_verification_email' for UserMailer:Class
     # ./lib/email_verification_job.rb:8:in `perform'
     # ./spec/lib/email_verification_job_spec.rb:12:in `block (3 levels)'

Finished in 0.30142 seconds (files took 4.91 seconds to load)
2 examples, 1 failure
```

`UserMailer#send_verification_email` doesn’t exist yet, but we know this and it’s not an issue we are concerned about 
right now. So, we can make the example pass by mocking the `send_verification_email` method by `allow`-ing it to be invoked:

```ruby
describe "#perform" do
  it "finds the email by id" do
    allow(UserMailer).to receive(:send_verification_email)
    expect(Email).to receive(:find).with(12)

    EmailVerificationJob.new.perform(12)
  end
end
```

This time all of our examples pass.

# A Word of Caution

Note that in the example above we’ve reached a green spec (and possibly, the whole test suite), while at the same time 
our implementation contains code calling an undefined method. Are we just fooling ourselves with all this mocking?

This is one of the reasons why there are people who prefer to never mock. We won’t go into that argument in this 
tutorial. With great power (of expressiveness and flexibility of code we can write and interfaces we can design by 
mocking, in this case) comes great responsibility.

It is important to bear in mind that a mocking approach should always be used within a development cycle that begins 
with a high-level integration or acceptance test. In Ruby applications, this frequently means writing a Cucumber 
scenario which describes and checks for the key points of the business logic, before diving into the implementation 
code. There, in the lower layers of the code, mocking can sometimes be a good way to drive your code.





