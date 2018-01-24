---
layout: post
title:      "Using JQuery to create a front-end for Simple Tidy"
date:       2018-01-24 16:51:04 +0000
permalink:  using_jquery_to_create_a_front-end_for_simple_tidy
---


I've pretty much accepted that after graduation I'll be deleting all these blog posts and re-writing everything with the goal of sounding more professional and technical. 

So, with that in mind...

JQuery and AJAX are "Great!" in that I can render content on the page without a refresh.  That's the kind of responsiveness users expect nowadays. Refreshes are just plain unacceptable.

That being said, when I decided to tinker more with the layout and presentation of the site AFTER implementing my front-end code, I found that one simple change could break an entire system.  This is because when working in the ERB files it was difficult to continue visualizing the DOM and how its elements were hooked into my JQuery code.  This lead me to two realizations...

1. I would likely soon be introduced to a better front-end solution (already I'm seeing where this problem is less existent in React) 

and
 
2.  I needed to hunker down and learn test writing.


Let's discuss tests really quickly.


In the Learn.co slack community it's not uncommon to see an occassional post asking "is writing tests going to be required for the project?" or "are there going to be more lessons on writing tests?"  From what I gather, we all find it to be a little intimidating, and since it's not really in the requirements for the projects or covered much in the curriculum, it's probably not implemented much.

That being said, while it seems time-consuming to both learn how to create tests and then implement them, you'll likely save that time tenfold as your project gets bigger and more complex and you're able to automatically catch bugs.  It's just not efficient to be manually testing your app with every little change that you make.

When setting up tests with Capybara and Rspec, do yourself a favor and look into the [Guard Gem](https://github.com/guard/guard).  It's a really really simple command line tool that, while it's executed, will run through your tests ALMOST real-time as you're coding.

Unfortunately I didn't muster up the courage to approach test writing until after I'd actually finished the bulk of the requirements for this assignment, but I'm planning on continuing to build on my app so I'm writinging more and more tests for it as we speak.

Moving on to the rest of the app development...

Ajax was pretty slick in allowing me to pass information back and forth from the server without refreshing the page.  However, I did run into an issue where I was deleting flex items, and needed the container to "refresh" so all the items would realign nicely.  I ended up using the Ajax shortcut [.load](http://api.jquery.com/load/) to essentially "reload" the flexbox container holding the items.  It's a bit clunky and slow, so I'm hoping to figure out a better alternative down the road.

```$("#list-container").load(location.href+" #list-container>*","");```

Overall, like all the projects, implementation was less intimidating once I dove in and got my hands dirty, probably because working on my ideas as opposed to arbitrary lab examples is a lot more interesting.  I'm looking forward to rebuilding this using React after graduation!

On to the next challenge...

[SimpleTidyV2 @ Github](https://github.com/meebenitez/SimpleTidyV2/)
















