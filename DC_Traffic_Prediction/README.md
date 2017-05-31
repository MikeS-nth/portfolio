# Predicting D.C. Traffic

Traffic in the District of Columbia is bad; that much is a given. But what if you could know ahead of time just how bad it was going to be?

I set out to try to answer that question based on an intuition that's evolved in 15+ years of living here: Perhaps the volume and intensity of political/governmental activity and events on a given day impacts that day's traffic.  

After all, politics and government remain the District's primary industry, with many adjacent and/or support industries (e.g., catering, private security, lobbying, etc.).  If the political calendar drives these other industries, perhaps its influence on the total number of people coming into the city can be correlated to traffic.

This was a proof-of-concept project, and I'm happy to report that the results suggest that it may indeed be possible to build a traffic forecast based on event data.

I am pleased to say the results are encouraging, and the models I built perform reasonably well at predicting high-level classes of traffic congestion.  This post includes an overview of the project, and the jupyter notebook with code is available [here](https://github.com/MikeS-nth/portfolio/blob/master/DC_Traffic_Prediction/Predicting_DC_Traffic_Mike_Sanders.ipynb):  

## Project Overview
Traffic in the District of Columbia is bad; that much is a given.  But what if you could know ahead of time just *how* bad it was going to be--*tomorrow*?

Mapping and directions apps like Google Maps, Waze, etc. already do a pretty good job of showing where the traffic is bad right now.  They also estimate travel time in traffic based on whether or not you are traveling during rush hour.

But as anyone who lives or works in the District will tell you, there are those days when the traffic is really bad... as well as some when it's surprisingly not so terrible.  What if we could predict those patterns in advance, so that you could get a sense of tomorrow morning's traffic the night before--just like a weather report.

If that were possible, what could we use to make that prediction?

In the years I've lived here, I've often wondered if there was any link between politics/government (still the city's primary industry) and traffic.  The intuition is twofold:
1. The severity of traffic is related to the total number of drivers coming into the city at roughly the same time
2. When there is an uptick in political/governmental activity, there is an increase in the aggregate number of people who come into the city.  Not just legislators, staffers, and high-level officials--there's a very large ecosystem of adjacent roles and industries--such as lobbying, catering, security, event planning, etc.--that have a role in these events and activities.

In this project, I set out create a proof of concept of the idea that, by monitoring upcoming political/government events (and any other very large events), we can adjust the probability of how bad *tomorrow's* traffic will be.

## Approach

Traffic (and transportation generally) in a major metropolitan area is a complex and highly dynamic system.  The objective is like a weather report for traffic--and I believe there is something deeper to that analogy.  Weather is highly complex, driven by the interaction of many natural processes, which are themselves complex. While I suspect traffic may not have the exact same level of complexity, it is also impacted by many diverse factors.  

With this in mind, it seemed especially important to be very deliberate about containing the scope.  This meant gathering data and selecting features with the following criteria:
- Is it logically and intuitively credible that it could impact traffic?  I'm not looking at stock market data, because how could that possibly be relevant?
- Is it "collectable" ahead of the date I'm trying to predict?  This means there need to be online sources that share date and time informaiton for upcoming events of interest.  It also means that data on crashes, Metro fires, etc. don't make sense because their impact is on the same day I'm trying to predict.
    - *A side note on Metro SafeTrack:  I considered adding that as a feature, which would have been easy to do.  I chose not to because SafeTrack was in effect during the entire period observed, so it would not have added any information.  (Think of it as being "priced in" by drivers already.)*
- Does it reasonably align with the hypothesis, by which I mean, is it political/governmental in nature, or is it some other large event (politically oriented or otherwise) that could impact the daily traffic volume.

Further, I decided to focus my analysis on *morning traffic only*.  There are numerous reasons for this, but the most compelling is that, in my experience at least, evening rush hour traffic seems to follow wholly different rules than morning traffic.  If it is predictable, I suspect it has completely different predictors.

## Data Collected

This analysis covers the time period of April 12 through May 19, 2017 (excluding weekends).  This leads to a relatively small sample size of 27 weekdays (one day's data was lost).  This was necessary given the time constraints of the current project, but it is obviously smaller than I'd like.  However, this is a living project, and I intend to maintain the data collection and retrain the model periodically to improve it.

#### Events

Informed by the scoping considerations above, I collected the following data that I thought might have some impact on traffic.  For each day:
- Was the US Senate in session before 11 a.m.
- Was the US House of Representatives in session before 11 a.m.
- How many Senate committee hearings began before 11 a.m.
- How many House committee hearings began before 11 a.m.
- How many events at the Walter Washington Convention Center began before 11 a.m.
- How many events at the Ronald Reagan Building began before 11 a.m.

I had hoped to gather similar information about White House / Presidential activities.  However, the current administration has broken with precedent and no longer publishes the President's public schedule online.

#### Traffic
To collect traffic information, I utilized the [Google Maps Distance Matrix API](https://developers.google.com/maps/documentation/distance-matrix/).  This API returns distance, duration, and duration in traffic for a given route.  I compiled a list of 24 major traffic arteries within the District and gathered duration data from the API between 7 a.m. and 10 a.m. in 10-minute intervals.

The blue lines on the satellite image below depict the routes that were tracked.  All routes were defined as "outside-in" in order to measure "with traffic" commuting times.  That is, the origin was the point furthest from the city center, and the destination was closest to the city center for a given route.  

![Map of roads where traffic data was collected](https://michaeljsanders.com/img/routes_smaller.jpg)

## Results Summary

After numerous iterations of regression modeling and classification modeling, I came to the following results:
Using classification algorithms, the events data seems to provide a reasonably reliable predictor of whether a given day's traffic will be:
- Above or below the median
- Unusually bad
- Unusually less bad


|Prediction |Precision Score |Interpretation|
|:---|:--:|:---|
|Tomorrow's traffic will be ABOVE the median  |.67 | 67% chance the prediction is correct |
|Tomorrow's traffic will be BELOW the median |.83 | 83% chance the prediction is correct  |
|Tomorrow's traffic will be VERY BAD  |.86 |86% chance the prediction is correct |
|Tomorrow's traffic will NOT be VERY BAD  |.50 |50% chance the prediction is correct |
|Tomorrow's traffic will be relatively LESS BAD    |.88 |88% chance the prediction is correct |
|Tomorrow's traffic will NOT be relatively LESS BAD  |1.0 |100% chance the prediction is correct*

* *Obviously 100% is unlikely, and it is probably due to the small sample size.  However, what matters is that the model is tuned to be "suspicious" that the traffic will be less bad.  A bad prediction on this category could mean someone (like me) doesn't leave the house early enough and arrives late.*

Another side note:  I had hoped to be able to predict the *amount* of traffic delay in time (regression modeling), but it was unsuccessful.  However, given the small sample size, I don't believe that the results of this exercise disprove the ability to predict traffic times with this data.  I will test this again once I've collected enough more data.

## Project Next Steps

I consider this to be a living project and plan to continue developing and refining it.  In particular, I plan to continue gathering data to build a larger, more significant dataset to model against.  I'll use this data to retrain the current models, as well as explore whether the additional data allows for greater results using different methods (maybe even regression will work).

If you're interested in seeing the code and more graphs, please check out the [jupyter notebook]((https://michaeljsanders.com/img/routes_smaller.jpg)!
