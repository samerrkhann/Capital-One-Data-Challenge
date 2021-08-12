# Capital-One-Data-Challenge

## Problem Statement
I'm consulting for a real estate company that has a niche in purchasing properties to rent out short-term as part of their business model specifically within New York City. The real estate company has already concluded that two bedroom properties are the most profitable; however, they do not know which zip codes are the best to invest in.

The real estate company has engaged my firm to build out a data product and provide conclusions to help them understand which zip codes would generate the most profit on short term rentals within New York City. I looked at publicly available data from Zillow and AirBnB:

Cost data: Zillow provides us an estimate of value for two-bedroom properties

Revenue data: AirBnB is the medium through which the investor plans to lease out their investment property. Fortunately for you, we are able to see how much properties in certain neighborhoods rent out for in New York City

US County Level Socio-Economic Data - Gotten from USA.county.data package in R created by Emil W. Kirkegaard link

US Zip Code Data - Gotten from the zipcode package, it contains a database of city, state, latitude, and longitude information for U.S. ZIP codes from the CivicSpace Database (August 2004) augmented by Daniel Coven link

## Assumptions

No significant difference in housing prices in the same neighborhood
The investor will pay for the property in cash (i.e. no mortgage/interest rate will need to be accounted for).
The time value of money discount rate is 0% (i.e. $1 today is worth the same 100 years from now).
All properties and all square feet within each locale can be assumed to be homogeneous (i.e. a 1000 square foot property in a locale such as Bronx or Manhattan generates twice the revenue and costs twice as much as any other 500 square foot property within that same locale.)
Occupancy rate of 75% annually
50% of guests review the hosts/listings

## Metrics Created:

### Annual_Rent:

This is annual rent collected from a property in Airbnb assuming 75% occupancy using rent price per night from Zillow data. This value is assumed to be fixed.

### Payback_Period:

This is the number of years required to earn back the initial cost of investment from annual rental income from property assuming 75% occupancy throughout the year. Occupancy: 0.75*365 (Assumed occupancy of all properties in Airbnb)

### Rate:

So, we all are aware that properties value appreciate/depreciate depending upon the neighborhood and locality. In the Zillow data we have the average housing prices for a zip code for as far back as 1996 but these columns have lot of missing values. However, we do have complete data from 06-2007. The difference between in prices in 06-2007 and 06-2017 divided by 10 has been taken as an average rate by which a property rises in that zip code. Although a naïve approach, it still captures the essence of appreciation/depreciation of property by zip codes over the years and would be useful in adding a location specific factor in Return on Investment calculations. Roi_without: This metric is return on investment in 10 years assuming rent appreciates at a constant rate without including the equity value of the property.

It is calculated as follows:

(Annual_Rent(1+Rate/100) ^10)/Current Price of the property

Roi_10: This metric takes into account the return on investment through rental

as well as equity appreciation i.e. the increase in the price of the house is also taken into account to calculate roi. This has been calculated for 10 years as well. It is is calculated as:

A= (Annual Rent(1+Rate/100) ^10)/Price of the property {Rent amount in 10 years}

B= (Price of Property(1+Rate/100) ^10)-Price Of property {Increase in cost of property in 10 Years}

Roi_10= (A+B)/Price of Property

## Usefulness of these metrics:

Payback_Period: This tells us how fast we can recover investments from our property. This may be one of the conservative estimates to evaluate expected profits from a property.

Roi_without : This metric is a good metric to assess profitability from a property. Because of variable rates of appreciation for each zipcode this metric gives a good estimate how location of zipcode determines ROI of a property

Roi_10 : This metric would be useful to assess overall value of investment keeping in mind the increase in value of asset and also opens the value analysis in case rental Property decides to sell the current asset after 10 years. It is expected that Roi_without would be very similar in results in ROI_10 as it has high equity appreciation has same rate of increase as rate of rentals but ROI_10 . But some interesting insights can be drawn from differences in behavior of these two metrics for zipcodes.

## Data Quality and Processing Key Points:
select columns of interests in our analysis

Handling of missing values in the Data and doing necessary imputations.

Subset the data further as per needs for visualization

## About Datasets:
### Airbnb Data:
The biggest concern for our analysis was imputations for missing zipcodes. Once we have data for zipcodes the city and state can easily be inferred from zipcode value. We used pygeocoder a library built on google

maps api to impute missing zipcodes by using latitudes and longitudes. Luckily no missing values on lat and long. After imputation only 2 properties still had missing values as their coordinates were not too specific for api to give a zipcode. They were ignored and removed. Once we imputed zipcode a quality check was done on zipcodes to ensure proper string length and then a range of zipcodes between 10000 and 20000 were selected as properties in NY. Next we noticed 22 missing values for bedrooms which were removed from the dataset as their was no way to estimate the number of bedrooms. finally we subset the 2 bedrooms properties in NY CITY. This data set was then aggregated to be used for decision point 1 .

### Zillow Data :
This dataset was filtered for properties in NY as no missing values were present on city column. Then we selected the columns to be used for analysis. We took the column 2016-06 as current selling price of the property. In this filtered data set we did not find any missing values. However missing values were present in certain price years, but we did not use them in our analysis. While calculating rate, the farthest two columns were considered which had complete data in their columns.
