---
layout: post
title:  "Rapid Iterative Development: the steps to completing my first CLI gem"
date:   2017-09-29 01:46:57 -0400
---

# 
Having worked for seven years in software development in a 3D world building capacity, programming is actually a brand new skill for me.  In fact, other than a solid understanding of an operating system and familiarity with source control, there's very little **obvious** technical overlap between web development and my previous job as a game level designer.

That being said, I found my extensive experience utilizing rapid iterative development methodology to be immensely helpful when tackling 'Priorities', my first ever "from-scratch, designed-by-me" programming project.  

‘Priorities’ is a gem I built to help me narrow down my search for a new, affordable city to buy a home in.  It creates a list of “ideal” cities based on my chosen criteria (budget, politics, safety, education, etc.) in the order I find most important.

I wasn’t sure how the gem was going to work at first, and if it was even doable with my limited knowledge of programming. So I built an early prototype of just the barebones features the quickest and easiest way I could think of… with hashes and long form if/else statements.

**With rapid iterative development, you build with the tools and knowledge you feel most comfortable with to quickly test out your concept.  You want to be able to identify and bail on a bad idea sooner rather than later.**

I first spent half a day researching sites that I could scrape from, because if I couldn't find enough content, my gem was going to fail.  When I found a successful scrape, I set aside the parsing code for later use.

And then I spent a day designing/building the concept of narrowing down a list based on the user's choices.  I only built out 2 priorities to test out this early prototype.

After a couple days of "tinkering," my direction had solidified.  I felt more confident in my idea, and so I started to rebuild the architecture of my gem using objects.  I'm glad that I didn't start out working with objects right away because my confidence level was still a bit shaky around them and I think I would have abandoned ship and chosen a simpler project, not knowing what I was capable of.  **Seeing a working early prototype of my idea was motivating enough to help push me through later challenges.**

This point of commitment (when I switched everything over to a more "object oriented" structure), is where I truly leapt into the point of no-return.  I'd budgeted myself 1 week to do this project, so, at day 4, if I was going to come face-to-face with a regret, I'd have to compromise or make cuts without changing direction.  This is why it's so critical to work out your fundamental ideas early so you're not in a position to make the detrimental decision to keep something that should be cut simply because of the time invested in it.

I treated each portion of my program as a "feature."  I would build something to the point of "just working," build an interacting feature, and then find that the previous feature would likely need an iteration because of the new interaction.  I did ZERO polish until the last 2 days of development.

Also, throughout this project, I tested, and tested often.  This is a key tenant in the church of rapid iterative development.  If I broke this rule and just "built" without testing for a long period of time, I ALWAYS paid for it in having to cut or rewrite what I'd wrote.  There was one day when I was trying to take advantage of my kid's last day of daycare, so I parked myself at a coffee shop after dropping him off and feverishly worked to build as much as I could.  I made the mistake of building for four hours without **thoroughly** testing, and toward the end of the day I realized I had 30 lines of useless code that I needed to replace.  It was a dark moment.  Despite the many years experience that I have in rapid iterative development, I still make this mistake.  **Never iterate without testing.**  You'd be surprised how many game developers still don't understand this.

I wrapped up this project at around day eight.  How did I know when it was done?  The simple answer is... it's not done.  But I know it's a decent working prototype because when I test it I find it useful, and I can imagine it being modified into a successful web app.  But at this point, it's a proof-of-concept that should be implemented with a data-sourcing method more efficient than web scraping.  

**Here are some of the problems I ran into during development and how I solved them...
**

--"The power switch"--
I was originally storing an array of "cities" that fit the user's choices.  Each time a priority was picked, I would have to compare that array against all the existing City objects to grab the array cities' attributes.  It was insanely slow.

I solved this by creating an "on/off" switch attribute, where a city that still fits the user's chosen criteria is turned "on," but when it no longer fits it's turned off.  I was pretty proud of this solution since it was very "Object Oriented." :)


```
home_price.gsub(/[$,mM]/, '').to_i < user_budget ? city.avg_home_price = home_price : turn_city_off(city)
```

--"Dynamic lists"--
More than once I found myself in need of a dynamic "option" list that would delete an option once a user chose it.  

I ended up writing the options in either a hash or an array, and then iterating with index through them to generate the list.  When an option was removed, the list would appear to have been dynamically updated the next time I used it.


```
def pick_priority(state)
    if PRIORITIES.count > 1
    PRIORITIES.each_with_index { |priority, index| puts "#{index + 1}. #{priority}" }
    input = gets.strip.to_i
    input = input - 1
    priority = PRIORITIES[input]
    run_priority_check(priority)
    PRIORITIES.delete(priority)
    results_check(state)
    else
      puts "Looks like we've run out of priorities to choose from before we could get our list down to 5.  I guess that means you have lots of cities to look into.  Hurray for options!  Here's your list of cities.  Happy house hunting!"
      display_results_short(City.create_display_hash)
    end
  end
```


--"Handling cases when a nil value was scraped, or a page returns a 404 error."--

When I'd written enough "priority" methods to heavily test the web scraping, I started running into errors.  

To handle 404 pages, I used a rescue statement:

```
begin
        doc = Nokogiri::HTML(open(index_url))
        percent = doc.css("section#education-info").css("ul").css("li")[1].text.sub("Bachelor's degree or higher: ", "")
      end
    rescue OpenURI::HTTPError => e
      if e.message == '404 Not Found' || e.message == "404 NOT FOUND"
        puts "-->"
      else
        raise e
      end
    end
```

I ended up printing a secret message to myself ( "--->" ) to alert me to errors when testing the gem without weirding out the user.  Since I was attempting to generate urls based on city names, there were inevitably going to be situations where a page would not exist.   However, if I ran a test and saw numerous "--->" printed, I'd know something was up and needed further investigation.

As for scenarios when a 'nil' value was returned?

I simply checked to see if the value existed:

```
if median_income
   median_income.gsub(/[$,mM]/, '').to_i < 55775 ? city.median_income = median_income : turn_city_off(city)
end
```

**A Quick Post-mortem
**

Overall, I went over my time-budget on this project, but I'm thrilled to have discovered that I can program something that I designed from scratched.  I set out to make this gem "somewhat" useful, insightful, and customizable, and I think I was "somewhat" successful.

That being said, I know there are plenty of bugs buried in the gem, and my next step would be to build some tests to catch them.  I would first try to hook up a more efficient method for data sourcing so that I'm not slamming these unsuspecting websites with hundreds of hits at a time.  There are also plenty of opportunities for moving some of my code into modules.

Bugs and all, I'm quite happy with this first implementation and I look forward to revisiting it when I get a chance and possibly building it out into something live and awesome.


















