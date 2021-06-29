---
layout: post
title:  "Regular expressions in Ruby"
date:   2021-06-28 19:00:00 +0200
categories: [ruby, regex]
---

Regular expressions are needed to work with strings.

They are used when you need to find a match with some pattern in a string, and do something if this match is found, for example, replace part of the string with other characters.

For working with regular expressions, the Ruby standard library has a class in its arsenal `Regexp`, as well as methods defined by other classes.

Let's start by searching for strings in a way we already know.

Here we check the string for the presence of a word `Nick`, the result, respectively, returns us `true`:

```ruby
'Neil and Nick'.include?('Nick')
=> true
```

And in this case, the word `Alex` in our line is missing - we get the result `false`

```ruby
'Neil and Nick'.include?('Alex')
=> false
```

Now, using the same example, we will use a regular expression, namely, we will use a method `match` whose argument is the desired value enclosed by slashes, i.e. `/Neil/...` The method returns the found value to us in the form of a class object `MatchData`, and more specifically the name `Neil` in encoded form:

```ruby
'Neil and Nick'.match(/Neil/)
=> #<MatchData "\x83\xAE\xE8\xAO">
```

In the case of using a word that the string does not contain, we get nil.

We can also use a more concise method `=~` that does a similar job:

```ruby
'Neil and Nick' =~ /Alex/
=> nil
```

Substitute the correct word and we see that the method returned to us `9`. This number means the position (character number) in the string where the match was first encountered (the required condition). Let's check if the result is correct:

```ruby
'Neil and Nick' =~ /Nick/
=> true
```

We can set more complex constructions, for example, we need to find all the numbers, or skip only those data that match the regular expression (often you will have to use this approach when checking email).

Consider the following example:

```ruby
'cat' =~ /c.t/
=> 0
```

A period in a regular expression means that any single character is possible in its place, be it `ONE` number, `ONE` letter, or any other character.

Let us now apply the anchor in our regular expression ( anchor ) `^`- signifying the beginning of the line. When using this anchor, we explicitly indicate that the character should come first. In the case of a different character in the first place after the anchor (the beginning of the line), the method will return us `nil`:

``` ruby
'cat' =~ /^c.t/
=> 0

'1cat' =~ /^c.t/
=> nil
```

And now we will introduce a restriction on the last letter, that is, the letter `t` must be the last in the line, otherwise the line will not pass our 'funnel'. To do this, we will use an anchor `$`- denoting the end of the line:

```ruby
'cat' =~ /^c.t$/
=> 0

'cute' =~ /^c.t$/
=> nil
```

In the last example, the letter is tnot the last character, hence the result nil.

It's time to get acquainted with the quantifiers ( Quantifier ) from the English word Quantify - which means to determine the amount. Let's take an example already familiar to us, and substitute an asterisk symbol *(asterisk) instead of a dot. Using this quantifier, we can enter any number of characters from zero to infinity in place of the asterisk:

```ruby
'catttttttttttttttt' =~ /^c.t*$/
=> 0

'ca' =~ /^c.t*$/
=> 0
```

Let's validate the email, that is, create a simplified regular expression that checks the correctness of the entered email:

```ruby
/^[a-z0-9]+@[a-z0-9]+\.[a-z]+/ =~ 'google@gmail.com'
=> 0

/^[a-z0-9]+@[a-z0-9]+\.[a-z]+/ =~ 'google@gmail.%%'
=> nil
```

Let's take a closer look at the example:

* `//` - as we remember, we put our regular expression between the slash characters

* `^` - this anchor means the beginning of a line

* `[a-z0-9]`- character class, here we indicated that any numeric character is allowed, as well as any lowercase (small, not uppercase) letter of the English alphabet. Accordingly, if we wanted to skip capital letters, then we would write `[A-Z]`. Or, to make it even more beautiful and more compact at the end of our regular expression we can add a modifier ( modifier ) `i`, which turns a blind eye on a case letters (skip both uppercase and lowercase). The expression will take the form:`/^[a-z0-9]+@[a-z0-9]+\.[a-z]+/i`

* `+` - a quantifier that skips a result from one to infinity, that is, in our case `[a-z0-9]+`, any number of letters of the English alphabet and numbers from 1 to infinity are allowed

* `@` - check that the string has the given character in the strictly specified place, and this character must be no more and no less than one

* `\.` - we remember that .in a regular expression it means that in place of a given dot we can use any character in the singular, but we want to specify a specific dot character that is written in the email, for this we need to escape our dot with a backslash

`[a-z]+` - and lastly, we check the spelling of the top-level domain name (that is, for example, `ru` or `com`)

We can write our regular expression a little differently using the following syntax:

```ruby
`/^[\w\d]+@[\w\d]+\.[\w]+/ =~ 'google2012@gmail.com'`
=> 0
```

Where

`\w`- similar to an expression `[a-zA-Z]`, that is, any letter of the English alphabet, independent of the case, but in addition to this, the presence of an underscore is also allowed `_`, which in general is what we need to check email since this character in an email address is acceptable.

`\d`- similar to an expression `[0-9]`, that is, it checks a string for the presence of any numbers

We learned how to search and filter information using regular expressions, and now let's take a step further and learn how to transform the filtered information. We can apply a method `gsub`that takes the regular expression itself as parameters, as well as a string that will replace the found value. Let's consider an example:

```ruby
'I have an old car'.gsub('an old', 'a new')
=> "I have a new car"

'I have no money'.gsub(/no/, 'a heap of')
=> "I have a heap of money"

'I have a new car'.gsub(/a/, '1')
=> "I h1ve 1 new c1r"
```