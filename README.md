# Momentum Trading with Random Forests

## __Concept:__  
  - Predict futures prices in the near-term (1-10 seconds).  
  - Without knowing what causes the market to move the theory is that the model should be able to detect upward or downward market pressure.
  - Use the predictions to build a trading strategy.

## __Background:__
  1. Partnership:
    - I partnered with a high frequency trading firm that provided data for this project.
    - The firm specializes in low latency trading of futures and options.

  2. Data:
    - The data for the project includes information on a nano-second scale.
    - Every trade made and every bid or offer placed on the exchange is included in the dataset.
    - Additionally, there is information as to whether the trade/bid/offer was made from the buy side or the sell side.

## __Research Set-Up:__
  1. Data Set-Up:
    - In order for the random forest model to work, I needed to take the non-uniform time steps of the raw financial data and aggregate it into uniform time steps without losing information.
    - Once the data was in uniform time steps, I created numerous momentum indicators using weighted average prices, distance from the weighted average price and weighted volatility.
    - Then I created additional features in the form of lagged time states. This took the total number of features in my model up significantly.
  2. Model Set-Up:
    - Once the data was aggregated and features created it was time to set up training/testing splits, which were done using significant amounts of training data relative to testing data.
    - I ran the model iteratively, over the course of a given day: training on 5000 seconds on data and predicting on the next 120 seconds.  
    - I set up three different models to make predictions 1 second, 5 seconds and 10 seconds out.
    - This clearly took significant computing power, so I used numerous EC2 machines in order to run my models.
    - In total, I was running 1.1K different random forest models for a single day of trading.

## __Strategy:__
  1. Once the predicted prices were in from the 3 different model time horizons,   I designed the trading logic and wrote a strategy script.
  2. The strategy script ‘traded’ based on predictions from the models and enabled me to calculate profit and loss from the trading strategy.  
  3. Via the strategy script, I was able to tune how frequently my strategy traded, volume and the duration of holding period among other things.  

## __Predictive Power:__
  1. The model did a very good job of predicting the short term directional moves of the market.  This can be seen through the random forest’s ability to predict the next second’s price.
  2. If you were able to trade at the last traded price, then this model would be highly profitable. To quantify, the model had an average score of 79 and this would lead to trading profits of ~$150K if you could only trade/hold a maximum of 5 futures at a given time.  
  3. Unfortunately, you can only assume that you are able to trade at the best bid or offer price, which is typically one tick worse on both entry and exit into the position.  When this much more stringent assumption is used, the strategy is slightly unprofitable.  
  4. Since the model is relatively good at predicting the moves in price, I believe that investing additional time into the strategy logic would be very beneficial.   
