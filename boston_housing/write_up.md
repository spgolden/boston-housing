# 1. Statistical Analysis and Exploration

There are 506 observations in the data set. For row, there are 13 features. The minimum price for a house is 5.0 (presumably in log space) and the maximum price is 50. The mean price is 22.53 and the median price is 21.2. The standard deviation is 9.19.

# 2. Evaluating Model Performance
I chose to use Median Absolute Error as the performance metric. Even though Mean Square Error is commonly used, it can be sensitive to outliers (beacuse it is an average of the error squared). Medians on the other hand, are robust to large outliers and will not be as sensitive to outliers.

It's important to split the data into training and testing so that we don't overfit our model. While we can fit our model on the training set, we can get a (hopefully) unbiased estimate of the predictive power of our model on a new data set. This is useful for picking between models or paramaters to feed a model. If we were to just use the training set, we would not really know how the function would deal with new data. 

Grid search is important because it conducts an exhaustive search across all of the possible input parameters and measures the model's performance using a specified error metric. After fitting a model with each combination, it can return the set of parameters that minimize (or maximize) the loss function. With grid search, we should use cross validation. Sklearn's implementation of GridSearchCV uses a method called k-fold, by default (with 3 folds). In k-fold, the data is split into k groups. For the first round, in a 3-fold cross validation, folds 2 and 3 will be used to fit the model while fold 1 serves as the test set to get a measure of prediction error. Then, we rotate to fit the model again, this time on sections 3 and 1, while fold 2 is the test set. Finally, the model is fit on folds 1 and 2 while we get a prediciton error from fold 3. These errors can then be aggregated into a total error measure. This approach is useful because the model gets fit to every data point at least once. In a traditional setting, the model can be sensitive to which observations get put into the one time training and test split.

# 3. Analyzing Model Performance
Interestingly, across all depths, as the size of the training set increases, the test error goes down (to a point) and the training error goes up. The fact that training error goes up is probably due to the fact that it is much more difficult to perfectly fit a model to hundreds of data points than it is to fit a line through a handful of points. When the model is fully trained (at a max depth of 10), it appears to have reached a point of high variance and overfitting because the training error goes to zero and the test error starts going up after falling. The fact that the test error is much larger than than the testing error also suggests a degree of overfitting.

_Max Depth = 10_
![image](../learning10.png =450x)

Looking at a Max Depth of 1, it appears that the model is underfitting the data. The fact that the test error never really decreases as the number of training cases increases, suggests that the model is not really picking any information up from the new data, presumably because it is underfitting the data.

_Max Depth = 1_
![image](../learning10.png =450x)

_Model Complexity_
![image](../model_complexity.png =450x)

Looking at the model complexity graph, the training error falls sharply as the model's complexity increases. On the other hand, the test error hovers around the same level even as the complexity of the model increases, consistent with overfitting starting as the model becomes over complex at the expense of increased variance. Based on the model complexity graph, I'd recommend using a max_depth of about 5. This decision is based on the "elbow" of in the test error at about this point. Without adding more complexity, we can get about the same test performance and a depth of 5 as 10. In this case, I think the more parsimonious model wins. 

# 4. Model Prediction
The Grid Search seems to select a max depth of 4 most often, but also selects other values occasionally - all the way to 10. These changes are due to which observations get put into the three fold validation. The predictions for the new house's value range from 19.99 to 21.63, depending on the max depth of the tree. The max depth Grid Search chooses (4) is about what we eyeballed in part 2, which is a good sign. The predicted value is also not out of line with the prediction data - well within one standard deviation of the mean.