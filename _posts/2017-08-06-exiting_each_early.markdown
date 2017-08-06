---
layout: post
title:  "Using .take to exit .each early"
date:   2017-08-06 03:22:38 -0400
---


For the Oxford Comma procedural ruby lab I wrote some pretty cumbersome code.  If you don't remember that lab, this was the task:

> Write a method oxfordcomma that takes an argument array of string elements and converts it into a string using the Oxford comma. For example, the array ["fiddleheads","okra","kohlrabi"] should get converted to the string "fiddleheads, okra, and kohlrabi". 

It bugged me quite a bit because in my head I knew a simpler solution than what I'd eventually submitted, but I couldn't quite make it work given my constraints (... and by constraints I mean I was trying to code on my Macbook air in the middle of an indoor playground surrounded by a dozen screaming toddlers, one of them mine.)

I wanted to use the .each iterator and add a comma to all names in the array BUT the last name, and I thought that might require a while loop with a counter.  Unfortunately, I couldn't quite get it to work in my test cases (at one point I was looping the each, and then eaching the loop).

So in the limited time I had to complete the lab, this is the chunky solution I ended up with:

```
def oxford_comma(names_array)
  if names_array.length == 1
    string = names_array.join
  elsif names_array.length == 2
    string = names_array.join(" and ")
  else
    no_comma = names_array.length - 1
    new_names_array = []
    names_array.each do |name|
      new_names_array << name + ","
    end
    new_names_array[no_comma] = names_array[no_comma]
    names_array[no_comma]
    new_names_array.insert(no_comma, "and")
    string = new_names_array.join(" ")
  end
  return string
end
```

I basically created a new array with all the names + a comma, then replaced the last name in the new array with the comma-less name from the first array.  
```
 no_comma = names_array.length - 1
 new_names_array = []
 names_array.each do |name|
    new_names_array << name + ","
 end
 new_names_array[no_comma] = names_array[no_comma]
```

I know.  Overly complex.  But it worked.

So after getting my kid to bed, I took another stab at trying to accomplish my original plan.  I googled "use while in .each" and found the .take method.

My solution got even simpler.

When used with an iterator, .take will allow you to choose how many times you want to loop through the collection you're iterating on.  

For example...  .take(4) takes only the first 4 entries in the collection. Awesome!

So here was my second attempt at Oxford_Comma:

```
def oxford_comma(names_array)
  if names_array.length == 1
    string = names_array.join
  elsif names_array.length == 2
    string = names_array.join(" and ")
  else
    no_comma = names_array.length - 1
  	names_array.take(no_comma).each_with_index { |person, index| names_array[index] = person + "," }
    names_array.insert(no_comma, "and")
    string = names_array.join(" ")
  end
end
```

I used .take to stop my .each method of adding commas at the second to last name.  Then I inserted "and".  And then finally I converted the array to a string with the .join method.  It's much simpler and I trimmed 6 lines of code and one unneccessary array.  I'm sure I can simplify this even more, but this was a good start.

To learn more about .take I recommend this page over at RubyCuts --> [Take Method](http://www.rubycuts.com/developer-resources/ruby-enumerable-module/take-method/)

And to learn more about using the .take method with the .each method, check out this Stack Overflow post -> [Escaping the .each iterator early in Ruby](https://stackoverflow.com/questions/1568288/escaping-the-each-iteration-early-in-ruby)
