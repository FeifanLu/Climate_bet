# Climate Bet : Statistical View of Global Warming
## Background: 
Many believe that climate change is one of the biggest threats to humanity and that massive changesmmust be made in how we live to revert it and reduce its impact.

Simultaneously, there are also many skeptics, who either do not believe in the threat or do not see a need to do much about it.

A fascinating statistical debate between the supporters and skeptics of global warming is the so-called "Climate Bet" (http://www.theclimatebet.com/). The bet's nature is that Mr. Armstrong, a statistician, claims that the model predicting a constant temperature will be more accurate than the model that predicts warming. The 10-year bet was offered in 2007 by Mr. Armstrong to Mr. Gore, a global warming advocate, who declined.

I dived into this debate and looked at the statistical evidence for global warming – a primarily scientific, fact-based view on the situation.

## Overview:
* Obtain the data from [NASA](https://data.giss.nasa.gov/gistemp/)and [UK MET Office](https://sites.uea.ac.uk/cru/). Mr.Armstrong used NASA data for his analysis. Mr.Gore's team used UK MET data for their analysis.
* Apply time series analysis and train six models on NASA data and seven models on UK MET data.
* Utilize the rolling window cross-validation to find the best model for each dataset.
* Train the two best models on the pre-2007 data and use them to make predictions. Then make the naïve model forecast of constant temperature per Mr. Armstrong  and compare the three models using the actual temperatures for 2007-2017 (a period of the bet).
* Repeat the same analyses starting in the year 1999 with 10- and 20-year time-intervals.

## Code and Resources Used 
**R Version:** 4.03 
**Packages:** forecast, fpp, ggplot2, astsa 
**Data Source:** (https://data.giss.nasa.gov/gistemp/), (https://sites.uea.ac.uk/cru/)
**Reading Material:** (http://www.theclimatebet.com/), (http://www.kestencgreen.com/G&A-Skyfall.pdf), (https://repository.upenn.edu/cgi/viewcontent.cgiarticle=1161&context=marketing_papers)
**Modelling Reference:** (https://otexts.com/fpp2/)
