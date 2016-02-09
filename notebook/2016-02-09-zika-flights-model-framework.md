---
title: "Framework for Zika Travel Model"
author: "Noam Ross"
date: 2016-02-09
notebook: zika-virus
---

# Basic Framework

**Dependent variable:**

Time from start of Zika Outbreak (May 2015), to date of *first autocthonous
infection in a country*.

-   As [reported to WHO](http://www.who.int/csr/don/archive/year/2015/en/) or
    found in news via [HealthMap](http://www.healthmap.org/en/), whichever
    is earlier.

**Independent Variables to test**

1.  Connectivity measures from FLIRT (to be tested separately to determine
    best predictor)

    -   Aggregate total estimated passengers from all Brazil Airports to all
        airports in each destination country from 1 May 2015 to each countries'
        first infection.
    -   As above, but aggregate estimated passengers from Brazil plus other
        infected countries starting at their infection dates.
    -   As both of the above, but limiting to only single-leg connections.

    The above will test (a) the predictive power of air travel, (b) whether
    infections are primarily driven by the Brazil source population or whether
    other source populations are important, and (c) the additional predictive
    power provided by the FLIRT simulator.

2.  Health spending per capita, as a measure of suveillance effort, to account
    for different amounts of time from arrival to detection

# Requirements for queries to the FLIRT service

In query, provide:

-   List of origin airports
-   List of destination airports (optional)
-   Date range (start, end)
-   Simulation parameters
    -   Number of passengers to simulate
    -   Maximum number of legs (optional, default to maximum in the
        distribution used)
    -   Simulator version (default to latest)

Return:

-   For each origin airport
-   Total number of departure seats/passengers in each period
-   Row of each destination airport with nonzero passengers estimated in the
    time period, with
    -   Number of simulated arrivals at destination
    -   Filtered by optional destination airports
-   Metadata
    -   Copy of query submitted
    -   Timestamp, simulator version

## Notes

Ideally the passengers simulated should be distributed amongst origin airports
according to their total outgoing flights, rather than an equal number simulated
from each origin airport.

It would be nice to be able to provide alternatives to defaults simulation
parameters for sensitivity testing:

-   Flight leg probability distribution
-   Layover window (is this maximum hours or mean of a decay distribution?)

## Questions

Will it work to have a single query over an extended date range? Or should the
simulation be run on a short date range, because that simulation is based on
aggregate flights in that time period? (In this case, I'd run a query/simulation
each for periods of a week or a month, and sum the results).
