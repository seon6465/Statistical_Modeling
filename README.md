# Stock Prediction

## Introduction


The data we are going to use involves prices of different stocks (Microsoft, Apple and Tesla). The important factor about these stock data sets is the price of stock for different days. All of the datasets include a Date (day), Open (initial price when market opens), High (highest price during the day), Low (lowest price during the day), Close (closing price when market closes), Adj.close (final price of a stock at the end of the day when banks unload their positions and adjusted price count for this) and volume (number of stocks sold in one day). The datasets are interesting because they give us different information about stock prices during the day. Stocks are time sensitive which means the price is constantly changing throughout the day before the market closes. Determining the future value of a company’s stock is important because it could yield a significant amount of profit. One way to accomplish this is to use a linear regression model. 


## Method


For this project, we split the models into 3 month intervals for accuracy. Specifically, the smaller the input date range, the more accurate we will be since we are working on a smaller time scale (smaller noise).

The ANOVA test compares the predictive power of our AIC-selected models the predictive power of the full model. It is important to avoid models with too many variables because it may result in overfitting. For this project, we did not take out any outliers or influential points because these points indicate future movements in the market (outliers are common in trading). To select our model we followed a standard practice method. After we had segmented our data, we then went to create a full model for every segment or split. With this full model in hand, we applied a backwards AIC to the full model for each split. This is important, as it crafts a custom model for each split of data we have. This means that we can correctly fit our model depending on market conditions. For example, in times of high volume trading, where the price was moved simply by amount of trades, and not signals such as news, we can see that the step AIC included volume and price. But in more news oriented situations, the open and high price were included, as they represent the current state of the stock, and are more influential to the future value. With a model in hand for each spilt, we can then perform validation, and predictions with our model. 

Our goal is to create a model that can predict the stock price for the following day. If you look at the dataset of MSFT_stock (Microsoft) and AAPL_stock (Apple), there’s a column called “Adj.close” which stands for Adjusted Close (this is the final stock price at the end of the day). Our model predicts the final stock for the following day and in order to achieve this, we shifted up the Adj.close by one day because we want to predict the stock price for the following day. To be specific, if Adj.close is $350 on the date 2018-01-03, we would place $350 on the date 2018-01-02 in the new column, Target, and vice versa. 


## Results


The anova tests for MSFT all have pvalue greater than 0.05. If we made the rejection level alpha = 0.05, we'd end up not rejecting the null hypothesis. Instead, we choose to pick 2 models with the lowest pvalues. For instance with MSFT, we should choose Target ~ Low + Adj.Close and Target ~ Adj.Close because they have the lowest pvalues (0.5114 and 0.5175 respectively). We do this because our data has high noise so getting exact anova value is difficult. Same as before, for AAPL, we choose 2 models with the lowest pvalue which are Target ~ High + Low + Close + Adj.Close + Volume and Target ~ Open + Low + Adj.Close (pvalues 0.2174 and 0.483 respectively). Same as before, for TSLA, we choose 2 models with the lowest pvalue which are Target ~ Open + High + Close + Volume and Target ~ Close + Volume (pvalues 0.4858 and 0.6389 respectively).

In addition, looking at the fitted vs residuals for MSFT, AAPL and TSLA, none of the diagnostic plots seem to violate normality. This means that during that time, the market was behaving in a linear way. In the future, we could use this information to decide whether or not our model should be used to trading. Looking at the Normal Q-Q for MSFT, AAPL and TSLA, Normal Q-Q 1 MSFT, Normal Q-Q 7 AAPL and Normal Q-Q 7 TSLA seem to violate normality. This means that during that period, the data points were not normally distributed. However, it's really hard to judge constant variance and normality by looking at these diagnostic plots. Therefore, we performed tests that could give us the pvalue and set an appropriate significant level for rejection.

Looking at the Shapiro.Wilko test:

1. For MSFT at alpha level 0.05, 1st, 5th and 7th model does not violate normality. 
2. For AAPL, the pvalues are larger than the previous one, so we'll use a larger signifigance level. If we use alpla = 0.2, the 1st, 5th and 6th models do not violate normality.
3. For TSLA, if we use signifigance level 0.05, the 1st, 3rd and 4th model do not violate normality. 
