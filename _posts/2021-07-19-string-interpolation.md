---
layout: post
title:  "String interpolation in Ruby"
date:   2021-07-19 17:30:00 +0200
categories: [Ruby]
---

`String Interpolation` it is all about combining strings together, but not by using the `+` operator, and this is 
how it looks:

```ruby
name = "Fil"
puts "Hello, #{name}!"
```

Using this syntax everything between the opening `#{ and closing }` bits is evaluated as Ruby code, and the result of this evaluation will be embedded into the string surrounding it.

In other words, when Ruby finds `#{name}` in this string, then it will evaluate the piece of Ruby code `name`. It finds that this is a variable, so it returns the value of the variable, which is the string `"Ada"`. So it embeds it into the surrounding string ` "Hello, #{name}!"`, by replacing `#{name}`.

As you know, there is a default way to concatenate strings using the `+` operator in most languages, for example:

```ruby
name = "Nick"
puts "Hello, " + name + "!"
```

But, if we consider these methods in more detail, then we can also finally explain the difference between strings created with single and double quotes:

String interpolation only works with double quotes.

That means that:

```ruby
puts "Interpolation works in double quoted strings: #{1 + 2}."
puts 'And it does not work in single quoted strings: #{1 + 2}.'
```

will print out:

```ruby
Interpolation works in double quoted strings: 3.
And it does not work in single quoted strings: #{1 + 2}.
```

So, why do people prefer string interpolation?

First of all, again, it’s slightly fewer letters to type. In our example, that’s just 5 characters, no big deal. However, consider a longer string, which is constructed using three, four, or more variables. Now this extra space quickly adds up, and things wouldn’t fit nicely on a single line anymore.

Also, many people find that the syntax reads a bit better. There’s a little bit less clutter, making it a little bit easier to see what’s going on.

One other, albeit pretty negligible reason is, that string interpolation actually uses less resources:

 * The code "Hello, #{name}!" creates one single new string object, and then embeds the existing string "Ada" into it.

 * The code `"Hello, " + name + "!"` on the other hand creates 3 new string objects: first it creates the string `"!"`, and passes it to the method `+` on the existing string `"Nick"`. The operator `+` returns a new string, which now is `"Nick!"`. Now this string is passed to the method `+` on `"Hello, "`, which again, creates a new string, `"Hello, Nick!"`.

So, string concatenation creates 2 more string objects even in our simple example. These intermediate objects are immediately discarded, because they’re not used any more. We’re only interested in the final result `"Hello, Fil!"`.
