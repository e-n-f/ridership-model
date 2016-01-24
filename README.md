ridership-model
---------------

For the record, these are my predictions for the daily ridership
of stations on the Silicon Valley BART extension, if the
7 stations currently planned or under construction are built.

Station | Predicted daily exits
------- | ---------------------
Warm Springs | 4877
Milpitas | 4649
Berryessa | 5032
Alum Rock | 4713
San Jose | 8415
Diridon | 5989
Santa Clara | 4984

The model predicts ridership of existing stations with a geometric standard deviation of 1.5,
meaning we can be 95% confident that the ridership will not be lower than
44% of this prediction or greater than 225% of it.

I consider it somewhat unlikely (1 standard deviation above my prediction) that the
three stations of the Berryessa Extension will have 23,000 riders per day
at opening [as predicted](http://www.vta.org/bart/faq).
I predict 4369 when Warm Springs alone opens, and 15,539 for the three stations together.
Maybe by 23,000 they mean both entries and exits, in which case I am optimistic.

The 55,000 [projected daily riders](http://vtaorgcontent.s3-us-west-1.amazonaws.com/Site_Content/BARTPhase2-ScopingPresentation-50212.pdf)
of the remaning 4 stations is more unlikely, 2 standard deviations above my prediction.
Again, maybe they really mean both entries and exits,
in which case it's only a little high.

If Livermore and eBART riders behave like other BART riders, I also predict the following:

Station | Predicted daily exits
------- | ---------------------
Railroad Ave | 5794
Antioch | 6464
Isabel Ave | 3419

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
  * Increase the probability empirically to the 0.98th power of the distance between the two stations.
    This adjustment used to seem meaningful but no longer does.
  * Scale the probability to match the actual station-to-station ridership counts: 1282600 times the 0.8th power of the probability.
  * Sum the estimated station-to-station ridership count for each station pair
  * For each station, scale it again to match the station totals by calculating 2.15 times the 0.792th power of the station total.
 
There are a lot of regression-to-the-mean problems along the way, but it's not too bad.
