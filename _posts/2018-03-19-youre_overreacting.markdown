---
layout: post
title:      "overReacting"
date:       2018-03-19 21:55:25 -0400
permalink:  youre_overreacting
---


The final project!  My swan song!  Complete and in the bag!

Who am I kidding?  It's not even close to complete.  But the bulk of the architecture is there and ready for an assessment.

I actually started this project back in the "OO Ruby" days (feels like forever ago) when I built a little gem called "Priorities".  In that gem you'd wrecklessly scrape your way through the web to find a city that matched your perfect little priorities ("low crime, good schools, high incomes, & only people who agree with my politics).

Since that project I dreamed EVERY NIGHT of getting to the final final project so I could expand on that idea and turn it into something real and awesome.  And so I present to you....

**Perfect City Search**... .com (I bought the domain).

 
Before I begin writing about this app, I'd like to thank Maximilian Schwarzmüller at Udemy for the excellent course, [React 16: The complete Guide (including React Router and Redux)](https://www.udemy.com/react-the-complete-guide-incl-redux/learn/v4/overview).  It really filled in the gaps for me in my understanding of React and Redux and helped me push this project forward.


PerfectCitySearch has an API with over 25,000 cities that you can "filter" through to find the ideal city for you.  For the demo site I have about 5 filters working (median household income, median home prices, median age, region, population), but there are many more in the works.  I'm getting the bulk of my data from the US census bureau, wikipedia, and google places( although the caveat with google places is that they only allow 2,500 API hits a day, so I'm having to run my script in small batches).   I've also applied for several API's that will hopefully allow me to get loads and loads of interesting data.

Before I even touched React, I spent about 3 days writing scripts in good ol' Ruby to grab data and build the database and then I created a Rails API.  I really love Ruby.

Then, I wanted to keep all the querying on the server side, so I created a bunch of scope methods that would be my "filters" and then returned them as JSON. I started out simple.:

City.rb

```
    scope :by_population, -> (from, to) { where("population >= ? AND population <= ?", from, to)}
    scope :by_age, -> (from, to) { where("median_age >= ? AND median_age <= ?", from, to)}
    scope :by_home_price, -> (from, to) { where("median_property_value >= ? AND median_property_value <= ?", from, to)}
    scope :by_region, -> (region) { where(region: region)}

```

Cities_Controller.rb

```
def index
        @cities = City.where(nil)
        @cities = @cities.by_region(params[:region]) if params[:region].present?
        @cities = @cities.by_population(params[:pop_from], params[:pop_to]) if params[:pop_from].present?
        @cities = @cities.by_age(params[:age_from], params[:age_to]) if params[:age_from].present?
        @cities = @cities.by_home_price(params[:home_price_from], params[:home_price_to]) if params[:home_price_from].present?
        render json: @cities, status: 200
    end

```

In the index controller, I first grab all the cities as my starting "batch" that I want to filter through.  And then I filter through that batch with my scope methods if the pertinent parameter exists.

So now, if I wanted to grab all cities on the Pacific Coast where the population is between 75,000 and 400,000, in my redux action I fetch the following url:

```
http://localhost:3000/cities?[region]=pacific_coast&[pop_from]=75000&[pop_to]=400000

```

And THAT was the very beginning feature I needed for getting this project up-and-running. I've since expanded on the Cities index controller by allowing you to create different starting batches based on other paramaters (popular cities, your own saved cities), ordering results by popularity, more filters, and pagination.

With a solid way to grab the filtered data from the server complete, I just needed to build my front end filter components to allow the user to choose their paramters.  I kept them components simple and presentational, with the form values being the actual string I'd append to my url for my server fetch:

PopulationFilter.js

```
const PopulationFilter = (props) => {
    return (
    <div className="filter-div">
        <label htmlFor="PopulationFilter"><img src={require('../../assets/images/skyline.png')} className="stat-icon-sm"/> Population:</label>
        <br></br>
        <select value={props.value} defaultValue={props.filterHolder} id= "PopulationFilter" onChange={(event) => props.onFilterChange(event)}>
                <option value="">Deactivate</option>
                <option value="[pop_from]=400000&[pop_to]=10000000">Big City (400K+)</option>
                <option value="[pop_from]=75000&[pop_to]=400000">Medium City (50K - 400K)</option>
                <option value="[pop_from]=5000&[pop_to]=50000">Small City (5K - 50K)</option>
                <option value="[pop_from]=50&[pop_to]=5000">Small Town (50 - 5K)</option>
          </select>
    </div>
    )
}

export default PopulationFilter;

```

... and then I was off to the races!

Obviously a lot more went in to this app, including getting React & Redux to play nicely with Devise authentication,  dynamically rendering my filter components and moving them between "active" and "inactive" states, creating front-end pagination to work with my server-side pagination (from scratch!), creating a "heart it" system to allow the user to save cities, and more! 

This app is my most ambitious project yet and I'm quite proud of how far it's come. Now check back with me in a month and hopefully I'll have it live with loads more data, a dozen more filters, a search box, zillow listings, and a happy user base.  

Thanks!























