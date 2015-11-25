ridership-model
---------------

For the record, these are my predictions for the daily ridership
of stations on the Silicon Valley BART extension, if the
7 stations currently planned or under construction are built.

Station | Predicted daily exits
------- | ---------------------
Warm Springs | 4218
Milpitas | 4184
Berryessa | 4310
Alum Rock | 3937
San Jose | 7043
Diridon | 4693
Santa Clara | 4323

The model predicts ridership of existing stations with a geometric standard deviation of 1.49,
meaning we can be 95% confident that the ridership will not be lower than
45% of this prediction or greater than 222% of it.

I consider it fairly unlikely (1.5 standard deviations out) that the
three stations of the Berryessa Extension will have 23,000 riders per day
at opening [as predicted](http://www.vta.org/bart/faq).
I predict 3760 when Warm Springs alone opens, and 12,932 for the three stations together.
Maybe by 23,000 they mean both entries and exits, which wouldn't be far off.

The 55,000 [projected daily riders](http://vtaorgcontent.s3-us-west-1.amazonaws.com/Site_Content/BARTPhase2-ScopingPresentation-50212.pdf)
of the remaning 4 stations is slightly more unlikely, 2.6 standard deviations away.
Again, maybe they really mean both entries and exits,
in which case it's only a little high.

If Livermore and eBART riders behave like other BART riders, I also predict the following:

Station | Predicted daily exits
------- | ---------------------
Railroad Ave | 5051
Antioch | 5654
Isabel Ave | 2934

How does it work?
-----------------

The prediction comes from matching [LEHD commute flows](http://lehd.ces.census.gov/data/#lodes)
to existing [BART station-to-station ridership counts](http://www.bart.gov/about/reports/ridership)
with probabilities from the [BART station profile study](http://www.bart.gov/about/reports/profile).

The stages are:

  * Given all known Bay Area commutes, calculate the distance from their
    origins and destinations to the nearest BART stations or hypothetical future stations.
  * Calculate a probability that this trip would be taken on BART based on the product of the
    lognormal curves from the Station Profile Study: 1260 feet (times or divided by 2.8) from work
    and 5682 feet (times or divided by 3.35) from home.
  * Reduce each probability empirically by the 1.8th root of the distance from the station.
  * Increase the probability empirically by the 0.7th power of the distance between the two stations.
  * Scale the probability to match the actual station-to-station ridership counts: 15814600 times the 0.8th power of the probability.
  * Sum the estimated station-to-station ridership count for each station pair
  * For each station, scale it again to match the station totals by calculating 2.7 times the 0.766th power of the station total.
 
There are a lot of regression-to-the-mean problems along the way, but it's not too bad.
