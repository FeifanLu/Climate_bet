# Climate Bet : Statistical View of Global Warming
## Background: 
Many believe that climate change is one of the biggest threats to humanity and that massive changesmmust be made in how we live to revert it and reduce its impact.

Simultaneously, there are also many skeptics, who either do not believe in the threat or do not see a need to do much about it.

A fascinating statistical debate between the supporters and skeptics of global warming is the so-called "Climate Bet" (http://www.theclimatebet.com/). The bet's nature is that Mr. Armstrong, a statistician, claims that the model predicting a constant temperature will be more accurate than the model that predicts warming. The 10-year bet was offered in 2007 by Mr. Armstrong to Mr. Gore, a global warming advocate, who declined.

I dived into this debate and looked at the statistical evidence for global warming – a primarily scientific, fact-based view on the situation.

## Overview:
* Obtain the data from [NASA](https://data.giss.nasa.gov/gistemp/)and [UK MET Office](https://crudata.uea.ac.uk/cru/data/temperature/). Mr.Armstrong used NASA data for his analysis. Mr.Gore's team used UK MET data for their analysis.
* Apply time series analysis and train six models on NASA data and seven models on UK MET data.
* Utilize the rolling window cross-validation to find the best model for each dataset.
* Train the two best models on the pre-2007 data and use them to make predictions. Then make the naïve model forecast of constant temperature per [Mr. Armstrong's method](http://www.kestencgreen.com/G&A-Skyfall.pdf)  and compare the three models using the actual temperatures for 2007-2017 (a period of the bet).
* Repeat the same analyses starting in the year 1999 with 10- and 20-year time-intervals.

## Code and Resources Used 
**R Version:** 4.03 

**Packages:** forecast, fpp, ggplot2, astsa 

**External tools:** Tableau 

**Data Source:** https://data.giss.nasa.gov/gistemp/, https://sites.uea.ac.uk/cru/

**Reading Material:** http://www.theclimatebet.com/, http://www.kestencgreen.com/G&ASkyfall.pdf
https://repository.upenn.edu/cgi/viewcontent.cgiarticle=1161&context=marketing_papers, https://www.carbonbrief.org/explainer-how-do-scientists-measure-global-temperature

**Modelling Reference:** https://otexts.com/fpp2/

## Data Cleaning
Both datasets present global temperature anomalies: the difference between the temperature for the month and the average monthly temperature for the baseline 30-year peroid. 

NASA dataset is from 1880 to 2020 and the baseline is 1951-1980. 
UK MET dataset is from 1850 tp 2020 and the baseline is 1961-1990. 
The best estimate for average global temperature for the baseline 30-year period of 1951 through 1980 is [14°C](https://earthobservatory.nasa.gov/world-of-change/global-temperatures). Therefore, 14 degrees Celsius was added as a constant to both datasets to reversely transform the data from anomalies to actual measured values of temperatures. 

As I noticed, the baseline for NASA dataset is 1951-1980(30 years) and for UK MET dataset is 1961-1990(30 years). Though the two datasets have baselines close by, it is essential to note small differences while comparing the two datasets. The baseline of UK MET data set is 0.056041667 degrees Celsius lower compared with NASA dataset.
Except for the difference of baseline peroid, these two datasets have different measruing ways as well. Some other reasons such as differnt locations, time points for measuring can also potentially casue variations of the measured values. Based on these factors, 0.056041667 is not significant and I decided not to consider it for the simplicity of the analysis. 



## Model Building
I followed a four-step approach in this part. First, explore the time series data visually by graphs, including patterns, changes over times and remainder term. Secondly, check stationarity and differencing, since time series with trends, seasonality are not stationary and will affect model’s prediction. Third, based on step 1 and step 2’s analysis build models.  Fourth, compare the overall performances of the candidate models using both cross validation method and accuracy test to identify the best performing model. 
   
<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/Modelling_Process.png" width="500" lengt="600">

### EDA ###
<p float="left">
<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/EDA_NASA.png" width="500"> 
<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/EDA_UK.png" width="500">
</p>

From the above, I can infer that during the late 19th century, the temperature change was negative with no apparent trend. However, in the early 20th century around approximately 1910, the temperature increase trend is noticeable. This could be due to the economic growth and industrial revolutions around the world that had a noticeable global warming impact starting from 20th century. While the impact of industrial revolution on climate is not the focus of this project, without any analysis and just by visualizing the above graphs we could infer this point.


<p float="left">
<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/NASA.png" width="500"> 
<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/NASA2.png" width="500">
</p>

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/UK.png" width="500">



