# Airbnb Seattle Price prediction
Price prediction of Airbnb listings in Seattle.

## dataset
The dataset can be found at [Inside Airbnb](http://insideairbnb.com/get-the-data.html).
There are 3 files of listings data in this repository.

listings.csv: main file with various features of listings at the time of scraping.

calendar.csv: price and availability data of the coming entire year. 

reviews.csv: the detailed review data. 

In this project, I combined calendar data to the listings data and analyzed the seasonal pattern of prices. I did not use reviews.csv for this project.
It is most of the time clear from column names what they indicate.

## Algorithm
1. Random Forest regression
2. XGBoost regression
3. Multilayer perceptron

I also tried using blending technique by simple average of the 3 models.

## Result
I measured the performance of the models with 3 metrics.

MAE: Mean Absolute Error, MAPE: Mean Absolute Percentage Error, R2: R-squared value

### Random forest
#### on training data:
MAE:  1.64,
MAPE:  0.017,
R2:  0.996
#### on test data:
MAE:  2.63,
MAPE:  0.044,
R2:  0.976

### XGBoost
#### on training data:
MAE:  2.28,
MAPE:  0.032,
R2:  0.990
#### on test data:
MAE:  2.90,
MAPE:  0.054,
R2:  0.973

### Multilayer Perceptron
#### on training data:
MAE:  1.73,
MAPE:  0.017,
R2:  0.991
#### on test data:
MAE:  2.71,
MAPE:  0.046,
R2:  0.971

### Blending
#### on training data:
MAE:  1.97,
MAPE:  0.023,
R2:  0.991
#### on test data:
MAE:  2.72,
MAPE:  0.047,
R2:  0.974

## Summary & Conclusion
In this project, I analyzed Airbnb's open data in Seattle by exploratory methods and subsequently machine learning models. I started with combining listings' detail data and yearly price data. I only used the average monthly price. Threfore the dataset contains at most 12 different entries for the same listing. I took a lot of care when engineering categorical data. I even generated some new features from datetime information. During and after data cleaning & engineering, I discussed about various relations among important features using visualization and correation analysis. The plot of the monthly price shows seasonal tendency that in the summer (June~August) listings are priced higher. Analysis about amenities could be especially informative. According to correlation analysis and feature importance by random forest and XGBoost, items like elevator, pool, cable TV influences price most among various kind of amenities. Only luxurious houses/apartment have elevators or pools so this makes a lot of sense. Also I found that hosts are putting higher price when they offer many amenities. One of the most important categorical features was room type, which is either entire offer, private room, or a bed in shared room. This definitely helped my models to perform well in the highest price range. Downtown and Queen Anne were 2 most expensive areas in Seattle.

I trained random forest regression, XGBoost regression, and multilayer perceptron to predict monthly average prices. Random forest was trained with mean squared error, XGBoost was trained with log squared error, and multilayer perceptron was trained with mean absolute percentage error as loss function. Random forest shows significantly good performance obviously because of MSE penalizing error of big values more. On the other hand other 2 models seemingly work more consistently in lower price range. Although random forest performs very well in general in terms of metrics, the difference of its performance for training data and test data is relatively big. In this regard, XGBoost was the best to perform similarly to training and test data. However its downside is that it predicts some listings' price as 0 or negative. In this sense multilayer perceptron do not have these defects, but it looks slightly underfitting compared to the other 2. Lastly I also used blending technique by siply taking the average of all 3 models' predictions. It compromises all 3 models' downsides but the improvement was not significant enough to say we should take all these computational costs. Overall considering both the performance and computational cost, I would conclude that random forest was the best model for this dataset.

Although the performance in terms of metrics was quite good, one should take extra care to interpret them. Since there are at maximum 12 samples with the same identity with only month data being different, there is no surprise good predictive performance is achieved. It should be suspected that as a result the test set contains very similar data as the training set. Also since there are a lot of extremely high price and the inital MSE is very high, by its definition R2 could easily be very high. Another thing I was not satisfied with was the dataset itself. For price of accommodations, room size should be a very important factor but this dataset does not contain these information. If this is available as a predictive feature, even better performance will be achieved. 
