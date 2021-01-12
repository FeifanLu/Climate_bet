# Climate Bet : Statistical View of Global Warming
## Background: 
Many believe that climate change is one of the biggest threats to humanity and that massive changes mmust be made in how we live to revert it and reduce its impact.

Simultaneously, there are also many skeptics, who either do not believe in the threat or do not see a need to do much about it.

A fascinating statistical debate between the supporters and skeptics of global warming is the so-called "Climate Bet" (http://www.theclimatebet.com/). The bet's nature is that Mr. Armstrong, a statistician, claims that the model predicting a constant temperature will be more accurate than the model that predicts warming. The 10-year bet was offered in 2007 by Mr. Armstrong to Mr. Gore, a global warming advocate, who declined.

I dived into this debate and looked at the statistical evidence for global warming – a primarily scientific, fact-based view on the situation.

## Overview:
* Obtain the data from [NASA](https://data.giss.nasa.gov/gistemp/) and [UK MET Office](https://crudata.uea.ac.uk/cru/data/temperature/). Mr.Armstrong used NASA data for his analysis. Mr.Gore's team used UK MET data for their analysis.
* Apply time series analysis and train six models on NASA data and seven models on UK MET data.
* Utilize the rolling window cross-validation to find the best model for each dataset.
* Train the two best models on the pre-2007 data and use them to make predictions. Then make the naïve model forecast of constant temperature per [Mr. Armstrong's method](http://www.kestencgreen.com/G&A-Skyfall.pdf)  and compare the three models using the actual temperatures for 2007-2017 (a period of the bet).
* Repeat the same analyses starting in the year 1999 with 10- and 20-year time-intervals.

## Code and Resources Used:
**R Version:** 4.03 

**Packages:** forecast, fpp, ggplot2, astsa 

**External tools:** Tableau 

**Data Source:** https://data.giss.nasa.gov/gistemp/, https://sites.uea.ac.uk/cru/

**Reading Material:** http://www.theclimatebet.com/, http://www.kestencgreen.com/G&ASkyfall.pdf
https://repository.upenn.edu/cgi/viewcontent.cgiarticle=1161&context=marketing_papers, https://www.carbonbrief.org/explainer-how-do-scientists-measure-global-temperature

**Modelling Reference:** https://otexts.com/fpp2/

## Data Cleaning:
Both datasets present global temperature anomalies: the difference between the temperature for the month and the average monthly temperature for the baseline 30-year peroid. 

NASA dataset is from 1880 to 2020 and the baseline is 1951-1980. 
UK MET dataset is from 1850 to 2020 and the baseline is 1961-1990. 
The best estimate for average global temperature for the baseline 30-year period of 1951 through 1980 is [14°C](https://earthobservatory.nasa.gov/world-of-change/global-temperatures). Therefore, 14 degrees Celsius was added as a constant to both datasets to reversely transform the data from anomalies to actual measured values of temperatures. 

As I noticed, the baseline for NASA dataset is 1951-1980(30 years) and for UK MET dataset is 1961-1990(30 years). Though the two datasets have baselines close by, it is essential to note small differences while comparing the two datasets. The baseline of UK MET data set is 0.056041667 degrees Celsius lower compared with NASA dataset.
[Except for the difference of baseline peroid, some other reasons such as differnt measuring techniques, locations, time points can also potentially casue variations of the measured values.](https://www.carbonbrief.org/explainer-how-do-scientists-measure-global-temperature) Based on these factors, 0.056041667 is not significant and I decided not to consider it for the simplicity of the analysis. The plot below shows these two datasets are inherently differnet.

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/Data_difference.png" width="500">


## Model Building:
I followed a four-step approach in this part. First, explore the time series data visually by graphs, including patterns, changes over times and remainder term. Secondly, check stationarity and differencing, since time series with trends, seasonality are not stationary and will affect model’s prediction. Third, based on step 1 and step 2’s analysis build models.  Fourth, compare the overall performances of the candidate models using both cross validation method and accuracy test to identify the best performing model. 
   
<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/Modelling_Process.png" width="500" length="600">

### EDA ###
<p float="left">
<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/EDA_NASA.png" width="500"> 
<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/EDA_UK.png" width="500">
</p>

From the above, during the late 19th century, the temperature change was negative with no apparent trend. However, in the early 20th century around approximately 1930, the temperature increasing trend became noticeable. This could be due to the economic growth and industrial revolutions around the world that had a noticeable global warming impact starting from 20th century. While the impact of industrial revolution on climate is not the focus of this project, without any analysis and just by visualizing the above graphs I could infer this point.

#### NASA ####
For NASA’s data set, from the plot below, there is a clear and increasing trend starts from 1970s (Figure 2). About the seasonality (Figure 3), I couldn’t visually  see a strong seasonal pattern. From the data STL decomposition plot (Figure 4), we could see that the reminder component shown in the bottom panel is what is left over when the seasonal and trend-cycle components have been subtracted from the data. 

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/NASA.png" width="800"> 

The slow decrease in the ACF (Figure 5) as the lags increase is due to the trend. The plot also shows an indicative of a need for differencing. From the PACF plot (Figure 6), it seems like the autoregressive term is 4 (p = 4 for Arima model)

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/NASA2.png" width="800">


#### UK MET ####
For UK's data set, similar trend was observed. From the ACF and PACF plots, I knew that I needed to use differencing to stabilize the mean of our time series data. The ACF plot identifies the non-stationary of the UK data.

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/UK.png" width="1000">

### Stationarity ###

I used the ndiffs() function to check appropriate number of differences. It returned the diff = 1 on both datasets. After differencing, I further performed the KPSS test - which is unit root test to find whether differencing is required and find out on data stationarity. The test statistic on NASA dataset is 0.0227, which is below the 1 pct critical value 0.739 and this means data is stationary after differencing. It gave test statistic of 0.0079 on UK MET datset, which is below the 1 pct critical value 0.739 and this means data is stationary as well. 

### Build Model ###
#### Time Series CV on NASA dataset ####

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/NASA_model.png" width="500">

#### Time Series CV on UK MET dataset ####

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/UK_model.png" width="500">

### Model Selection ###
Based on my above analysis using NASA’s dataset, the [ARIMA (4, 1, 3) (0, 1, 2)[12]](https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/NASA_final_model.png) model is recognized to have the best overall performance. The model predicts with moderate confidence that the global temperature will likely continue to rise by 2 degrees Celsius, and possibly even by 4 degrees by 2100.
Based on our analysis using Met Office’s dataset, the [ARIMA (2, 1, 2) (2, 0, 0)[12]](https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/UK_final_model.png) model is the optimal model. However, the model predicts with 90% confidence that the global temperature will likely continue to rise by 1 degrees Celsius by 2100.

## Compare with the naïve model:
How did Armstrong get a forecast of 0.159?

[The actual challenge is between 01 Jan 2008 and 31 Dec 2017 – 10 years.](https://www.theclimatebet.com/2007/06/)

Excerpts as below:

“Starting at the beginning of 2008, one-year ahead forecasts then two-year ahead forecasts, and so on up to ten-year-ahead forecasts of annual “mean temperature” will be made annually for each weather station for each of the next ten years. Forecasts must be submitted by the end of the first working day in January. Each calendar year would end on December 31.”
[The forecast of 0.159 is based on another dataset UAH.](https://www.nsstc.uah.edu/data/msu/v6.0/tlt/uahncdc_lt_6.0.txt)
The graph below shows the comparison between Armstrong and Al Gore forecast from climate challenge

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/Naive_model.png" width="500" length="600">

To compare the overall performance the two 10-year predictions from 2007-2017 and 1999-2018 and the one 20-year prediction from 1999-2018 are included in the table below. A score of 1 and 0 for winning and losing was considered and totaled. This is consolidated in table below

<img src="https://github.com/FeifanLu/Climate_bet/blob/main/Climate_bet/Result.png" width="800" length="600">

Note: To be consistent with Armstrong’s research  paper (Kesten C. Green, 2009), I chooses the MAE as our major measurement in the model evaluation.

## Final Conclusions:
The benchmark model in this case is Armstrong Naïve Model

•	Naïve model outperforms my best models five times in six comparisons.

•	Naïve model’s performances are stable at two different datasets. Nevertheless, the MAE increases with the increasing forecast horizon. 

•	My best model for NASA dataset works as expected, but the best model for UK dataset works awkwardly. Especially for the 1999.1 – 2018.12 prediction on UK MET dataset, that model yields a two times MAE compared with the benchmark model.

•	Only the benchmark model’s prediction on NASA dataset, 1999.1 – 2008.12, has the Theil’s U number smaller than 1, which means only this model’s forecasting technique is better than guessing. 

Overall, the benchmark model (naïve model) beats my models. The only victory of my models happens at the 2007-2017 prediction on NASA data. The reasons could be:

1.	NASA data is not inherently flawed compared with the UK MET data, which allows models perform as they should be. According to Mr. Armstrong’s papers, the UK MET has some flaws. These flaws damage the data quality, which could cause models produce inconsistent and unrealistic prediction.

      a.	Not fully disclosed
   
      b. Not global and not produced by comprehensive coverage by satellites
   
      c.	Not avoid local effects such as encroachment by buildings, tarmac, and other heat sources
   
      d.	Not avoid inconsistent measurement.
    

2.	There is an upward momentum in 2007-2017 and our model can capture that one, which is a disadvantage for the naïve model.

3.	Over longer policy-relevant periods, annual global mean temperatures are highly stable and naïve model would beat our models all the time. However, in short-term forecasting, my model can get lucky because there could be a relatively consistent momentum or trend to be captured. 

Since there are myriad reasons of climate change known and unknown to mankind, it is incorrect to assume the upward momentum of global temperature of recent decades as a consistent trend and extrapolate this trend to the future. Maybe it is just an upward part of a long-term cycle and the global temperature will eventually fall.  It also could be costly to make and execute policies based on this un-tested assumption. 

Overall, accurately predicting the global absolute temperature is almost impossible due to several reasons not limited to measurement errors, unknown factors influencing weather, mystery natural phenomenon unknown to mankind.  












