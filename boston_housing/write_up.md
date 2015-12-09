# 1. Statistical Analysis and Exploration

There are 506 observations in the data set. For row, there are 13 features. The minimum price for a house is 5.0 (presumably in log space) and the maximum price is 50. The mean price is 22.53 and the median price is 21.2. The standard deviation is 9.19.

# 2. Evaluating Model Performance
I chose to use Mean Squared Error as the performance metric. This metric is commonly used in models with numeric outcomes. One of the downsides of this metric is that it gives disproportionate weight to the errors on outliers. However, due to the ease of use and interpretation I've decided to use it here. Other possible measures include:  median absolute error, r-squared, or mean absolute error.

It's important to split the data into training and testing so that we don't overfit our model. If we give the model too much data, it is likely to start fitting a function to all of the noise, instead of a more generalizable function built on a subset. Furthermore, we can use the test data to choose the best parameter (max depth) to minimize the error. If we didn't do this on a different data set, we might choose a less optimal parameter.

Grid search is important because it conducts an exhaustive search across all of the possible input parameters and measures the model's performance using a specified error metric. After fitting a model with each combination, it can return the set of parameters that minimize (or maximize) the loss function. With grid search, we should use cross validation. Cross validation essentially splits up our training set further into a training and testing set. This is useful for the same reasons it's useful in a larger context - optimizing parameters on a separate set of data.

# 3. Analyzing Model Performance
Interestingly, across all depths, as the size of the training set increases, the test error goes down (to a point) and the training error goes up. The fact that training error goes up is probably due to the fact that it is much more difficult to perfectly fit a model to hundreds of data points than it is to fit a line through a handful of points. When the model is fully trained (at a max depth of 10), it appears to have reached a point of high variance and overfitting because the training error goes to zero and the test error starts going up after falling.

![image](../model_complexity.png =450x)

Based on the model complexity graph, I'd recommend using a max_depth of about 5. This decision is based on the "elbow" of in the test error at about this point. Without adding more complexity, we can get about the same test performance and a depth of 5 as 10. In this case, I think the more parsimonious model wins.

# 4. Model Prediction
The Grid Search seems to select a max depth of 4 most often, but also selects other values occasionally - all the way to 10. These changes are due to which observations get put into the three fold validation. The predictions for the new house's value range from 19.99 to 21.63, depending on the max depth of the tree. The max depth Grid Search chooses (4) is about what we eyeballed in part 2, which is a good sign. The predicted value is also not out of line with the prediction data - well within one standard deviation of the mean.