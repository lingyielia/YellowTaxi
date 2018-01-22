# Predicting Pickup Density of Trips to Airports in New York City

![figure1](figure/Figure1.PNG)
New York City is host to a vast and complicated transportation system. The approximately 54% of the city’s residents who do not own personal automobiles are reliant on public transportation. These public-transportation aficionados support New York City’s Taxi and Limousine Commission’s (TLC) nearly 15,000 yellow and green taxi cab drivers by taking approximately 200 million taxi trips annually. Yellow taxis of NYC have what is called a “yellow zone,” where they are capable of serving customers, which is primarily through the Financial District up to Midtown Manhattan. In addition, they are capable of both picking up and dropping off passengers at airports in the NYC metropolitan area.

The primary research interest of this study concerns the pickup density of what are ultimately airport dropoffs in NYC and aims to address the following question: given a location in space and a time span, to what extent can the number of New York City yellow taxi trips’ ultimate destinations being one of the area’s three major airports (LaGuardia, JFK, Newark Liberty) be predicted? With this prediction, how may the NYC TLC gain insight into strategic deployment of yellow taxis throughout the city?

## The Data
Data utilized covered the period from January to June of 2015. As both spatial and temporal information are required, the data are limited to pre-2016, where geographic coordinates of pickup and dropoff are provided. Due to the limitation of computation capacity, data covered half year. All trips whose ultimate destinations were one the three major airports, i.e. JFK, LaGuardia, and Newark Liberty International Airport, were extracted from the original dataset for our study. Data sources included the following:

__TLC Trip Record Data__. Null values and outliers that were beyond reasonable locations and time ranges for a normal taxi trip were excluded. Features including year, month, hour, day of week, day of year, and trip duration are created from it.

__Ride-Sharing Data__.  Uber trip data are not open-sourced on its website. Because NYC has categorized Uber and Lyft as black cars and recorded them in the FHV dataset [1], the hourly FHV trip amounts were employed in this study.

__Weather Data__. the National Oceanic and Atmospheric Administration (NOAA) provides weather data in the form of daily summaries, including temperature highs and lows, precipitation amounts and snow depth, and weather type (i.e. fog). Their Storm Events database, which has 5 storm events in records in 2015, were also incorporated in the data.

A taxi zone shapefile was provided by the NYC TLC (Figure 1a). It separates the city into 263 zones, including the Newark airport zone in New Jersey. Location zones in this format were relatively large. For the purposes of this study, taxi pickup locations were geohashed (Figure 1b) with a precision of 5, meaning that each latitude and longitude coordinate in the data set were encoded in 30 bits with an error of 610.812 m [2]. In Figure 1b., there are roughly 700 geohash areas. While the taxi zone shapefile provided by the TLC have 263 polygons, the geohash gave higher resolution and more flexibility. The geohash encoding can be set into a higher resolution, but it will also make the dataset larger.

![zone](figure/zone.PNG)

## Method
Ensemble learning methods were employed to predict airport dropoff pickup density. A principal concern was selecting the most useful predictive features. To that end, features to initially consider include pickup location, number of passengers, fare and/or tip, proximity to building type, time of day, and day of week. Acknowledging successful pickup density prediction performed by Daulton et al [3], a random forest (RF) regressor was explored in order to decrease variance while utilizing the small bias of decision trees.

Data were split into training and testing sets with an 80%-20% split. The primary features included in the data frame to be analyzed included a pickup location from latitude and longitude coordinates as well as weather event data at certain times. A grid of parameters was defined over which cross-validation was performed to determine the optimal number of decision trees to include in the random forest. This optimized random forest was fit to the data and run on the test data, and R2  values were calculated for the test sample. From there, the predicted number of airport dropoffs originating in each geohashed region could be determined and compared to the actual number for that region for varying periods of time and weather conditions.

## Result
The random forest regressor was able to predict both relative density and absolute pickup counts reasonably well for each geohashed area. The overall prediction density is visualized in Figure 2a, where it can be observed that pickup density with an airport destination tends to decrease as one moves away from Manhattan. Interestingly, the model predicts a significantly visually less dense area of pickups to the east of the Lincoln Tunnel, near Penn Station, than the actual pickup density (Figure 2b).

![result1](figure/result1.PNG)

This is possibly indicative of people tending to be dropped off at Penn Station for its transportation services, as opposed to traveling to an airport via taxi from Penn Station. A strong predictor was competition presence (Figure 4), which likely dips in airport-destination pickups around major areas of public transportation. Density of trips on the weekend (Figure 3a, b) is disproportionately high in Manhattan’s Financial District and Midtown regions, with similarly high density from people traveling from one airport to another. In both overall and weekend cases, the model overestimates the number of pickups, resulting in relatively darker density maps.

![result2](figure/result2.PNG)

![feature](figure/featurerank.PNG)

For the entire range of data spanning six months, the testing sample R2  calculated as a result of the random forest predictions was 0.56, indicating that the model explains approximately 56% of the variability of the data around the mean. Given the extent of the data size and relatively small number of predictive features, confidence is increased in the resulting goodness of fit. Considering several other subsets of the data set, data filtered to include only weekend pickups, including Friday, Saturday, and Sunday, resulted in an R2 of 0.53. Data filtered to include only bad weather events where precipitation was significant resulted in a calculated R2 value that was similar at 0.54. Root-mean squared error was somewhat high at 1.82, indicative of the majority of predictions (2⁄3) lying within two orders of magnitude from the actual pickup value.

The applications of a yellow cab pickup density regressor, insofar as they result in airport destinations, are many. The data can be subsampled to predict values at a given time or day. Because weather data were utilized in the model design, it would be possible for the NYC TLC to build a forecasting application that can predict any change in airport destination density in specific regions, given upcoming weather forecasts and the type and time of future day being measured. From there, airport-related congestion may be predicted and relieved by selective allocation of taxi cabs intending to take customers to the city’s three major airports. As the FHV amount indicated a strong factor related to the yellow cab. For further study, the impact of rapid growth in Uber and Lyft on yellow cab pickup density and the whole transportation system would be interesting.

Team member:
[Emily](https://github.com/ekh331), [Lingyi](https://github.com/lingyielia)

References:
- [1] [For-Hire Vehicle Transportation Study (2016).](http://www1.nyc.gov/assets/operations/downloads/pdf/For-Hire-Vehicle-Transportation-Study.pdf)
- [2] [Geohash representation details.](https://github.com/tammoippen/geohash-hilbert)
- [3] [The Yellow Taxicab: an NYC Icon (2015).](http://sdaulton.github.io/TaxiPrediction/)
