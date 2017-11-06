---
layout: post
title:      "SimpleChoreTracker: my first Sinatra App"
date:       2017-11-06 10:50:30 -0500
permalink:  my_first_sinatra_app_-_simplechoretracker
---


"Don't go too crazy with your Sinatra app," they said.  "Keep it simple.  You'll want to put that time in to your Rails project later."

So I chose something I thought would be simple.  Two models.  Only one has_many relationship.  Super easy.  But as usual, every design decision was a bit of a rabbit hole.  Just like with my CLI Gem, though, I didn't mind the adventure as I seem to learn more in one project built-from-scratch than a dozen labs.

Even though this wasn't suppose to be a "big deal" I still wanted to build an app I would use, and, if you saw my apartment right now, you'd know why I chose to do a chore app.  SimpleChoreTracker is intended to be a relatively quick prototype I can start testing right away.  

First off, I want to give major props to Corneal.  That little "gem" is absolutely slick.  I'm pretty sure it saved me several hours of work and allowed me to start working on the meat and potatoes right away.

From there, the first rabbit hole was creating a concept of time in the app.  Chores needed to be "past due" a certain time after creation or completion.  If I were using rails I could simply + 1.day, + 1.week, +1month, etc.  But using pure ruby, I ended up working in seconds.  It's an imperfect system I found, as it doesn't take into account odd numbered months and those two times a year when we change our clocks (funny enough, I was unwittingly testing this app the night before daylight saving and kicking myself over adding 24 hours in seconds to a time variable, and it only showing that I'd added 23... eventually I figured it out).  I conceded that there'd be be much simpler ways to achieve my goal in Rails and accepted the imperfections.

After that the next rabbit hole was the layout.  Admitedly my CSS skills had hibernated, so there was a lot of copy and pasting from W3Schools.  Still, it was a sizeable chunk of the work as I needed to make an unknown number of chores spill over into X columns when iterated through.  I ended up using a handy method called each_slice.

```
<% @chores.each_slice(x) do |array_of_chores|%>
  <div class="row">
    <div class="x_columns">
      <% array_of_x_chores.each do |chore|%>
           <%= chore.name %>
      <% end %>
    </div>
  </div>
<% end %>
```

In the above, 'x' is the limit of items I want to list in a column.  So I created my own helper method to flow my data into 2 columns if the @chore array count was greater than 8, and only one column if less than 8.

```
IN MY HELPER METHODS
def slice_count(list_count)
     if list_count.even? && list_count > 8
       slice = list_count / 2
     elsif list_count.odd? && list_count > 8
       list_count = (list_count + 1) / 2
     else
       list_count = 4
     end
end
	 
IN MY CONTROLLER
@chores_slice_count = slice_count(@chores.count)

IN THE ERB
<% @chores.each_slice(@chores_slice_count) do |array_of_chores|%>

etc.


```

And that was that.  The prototype is still bare bones, but I can start testing it immediately.  I'll throw it into a regular pattern of iteration (test, change, test change) until it's something truly useful to me, and then I'll pass it to friends to test.  Eventually, I'll have my prototype at a state where I rebuild it in rails and start to finish it as a full fledged product.  Exciting :)
