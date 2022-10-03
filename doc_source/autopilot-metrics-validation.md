# Metrics<a name="autopilot-metrics-validation"></a>

This guide shows metrics  that you can use to measure machine learning model performance\. Amazon SageMaker Autopilot produces metrics that measure the predictive quality of machine learning model candidates\. The metrics calculated for candidates are specified using an array of [MetricDatum](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_MetricDatum.html) types\. 

The following list contains the names of the metrics that are currently available to measure model performance within Autopilot\.

**`Accuracy`**  
 The ratio of the number of correctly classified items to the total number of \(correctly and incorrectly\) classified items\. It is used for both binary and multiclass classification\. Accuracy measures how close the predicted class values are to the actual values\. Values for accuracy metrics vary between zero \(0\) and one \(1\)\. A value of 1 indicates perfect accuracy, and 0 indicates perfect inaccuracy\.

**`AUC`**  
 The area under the curve \(AUC\) metric is used to compare and evaluate binary classification by algorithms that return probabilities, such as logistic regression\. To map the probabilities into classifications, these are compared against a threshold value\.   
The relevant curve is the receiver operating characteristic curve \(ROC curve\)\. The ROC curve plots the true positive rate \(TPR\) of predictions \(or recall\) against the false positive rate \(FPR\) as a function of the threshold value, above which a prediction is considered positive\. Increasing the threshold results in fewer false positives, but more false negatives\.   
AUC is the area under this ROC curve\. Therefore, AUC provides an aggregated measure of the model performance across all possible classification thresholds\. AUC scores vary between 0 and 1\. A score of 1 indicates perfect accuracy, and a score of one half \(0\.5\) indicates that the prediction is not better than a random classifier\. 

**`BalancedAccuracy`**  
`BalancedAccuracy` is a metric that measures the ratio of accurate predictions to all predictions\. This ratio is calculated after normalizing true positives \(TP\) and true negatives \(TN\) by the total number of positive \(P\) and negative \(N\) values\. It is used in both binary and multiclass classification and is defined as follows: 0\.5\*\(\(TP/P\)\+\(TN/N\)\), with values ranging from 0 to 1\. `BalancedAccuracy` gives a better measure of accuracy when the number of positives or negatives differ greatly from each other in an imbalanced dataset\. For example, when only 1% of email is spam\. 

**`F1`**  
The `F1` score is the harmonic mean of the precision and recall, defined as follows: F1 = 2 \* \(precision \* recall\) / \(precision \+ recall\)\. It is used for binary classification into classes traditionally referred to as positive and negative\. Predictions are said to be true when they match their actual \(correct\) class, and false when they do not\.   
Precision is the ratio of the true positive predictions to all positive predictions, and it includes the false positives in a dataset\. Precision measures the quality of the prediction when it predicts the positive class\.   
Recall \(or sensitivity\) is the ratio of the true positive predictions to all actual positive instances\. Recall measures how completely a model predicts the actual class members in a dataset\.   
F1 scores vary between 0 and 1\. A score of 1 indicates the best possible performance, and 0 indicates the worst\.

**`F1macro`**  
The `F1macro` score applies F1 scoring to multiclass classification problems\. It does this by calculating the precision and recall, and then taking their harmonic mean to calculate the F1 score for each class\. Lastly, the F1macro averages the individual scores to obtain the `F1macro` score\. `F1macro` scores vary between 0 and 1\. A score of 1 indicates the best possible performance, and 0 indicates the worst\.

**`LogLoss`**  
Log loss, also known as cross\-entropy loss, is a metric used to evaluate the quality of the probability outputs, rather than the outputs themselves\. It is used in both binary and multiclass classification, neural nets, and is also the cost function for logistic regression\. Log loss is an important metric to indicate when a model makes incorrect predictions with high probabilities\. Values range from 0 to infinity\. A value of 0 represents a model that perfectly predicts the data\.

**`MAE`**  
The mean absolute error \(MAE\) is a measure of how different the predicted and actual values are, when they're averaged over all values\. MAE is commonly used in regression analysis to understand model prediction error\. If there is linear regression, MAE represents the average distance from a predicted line to the actual value\. MAE is defined as the sum of absolute errors divided by the number of observations\. Values range from 0 to infinity, with smaller numbers indicating a better model fit to the data\.

**`MSE`**  
The mean squared error \(MSE\) is the average of the squared differences between the predicted and actual values\. It is used for regression\. MSE values are always positive\. The better a model is at predicting the actual values, the smaller the MSE value is

**`Precision`**  
Precision measures how well an algorithm predicts the true positives \(TP\) out of all of the positives that it identifies\. It is defined as follows: Precision = TP/\(TP\+FP\), with values ranging from zero \(0\) to one \(1\), and is used in binary classification\. Precision is an important metric when the cost of a false positive is high\. For example, the cost of a false positive is very high if an airplane safety system is falsely deemed safe to fly\. A false positive \(FP\) reflects a positive prediction that is actually negative in the data\.

**`PrecisionMacro`**  
The precision macro computes precision for multiclass classification problems\. It does this by calculating precision for each class and averaging scores to obtain precision for several classes\. `PrecisionMacro` scores range from zero \(0\) to one \(1\)\. Higher scores reflect the model's ability to predict true positives \(TP\) out of all of the positives that it identifies, averaged across multiple classes\.

**`R2`**  
R2, also known as the coefficient of determination, is used in regression to quantify how much a model can explain the variance of a dependent variable\. Values range from one \(1\) to negative one \(\-1\)\. Higher numbers indicate a higher fraction of explained variability\. `R2` values close to zero \(0\) indicate that very little of the dependent variable can be explained by the model\. Negative values indicate a poor fit and that the model is outperformed by a constant function\. For linear regression, this is a horizontal line\.

**`Recall`**  
Recall measures how well an algorithm correctly predicts all of the true positives \(TP\) in a dataset\. A true positive is a positive prediction that is also an actual positive value in the data\. Recall is defined as follows: Recall = TP/\(TP\+FN\), with values ranging from 0 to 1\. Higher scores reflect a better ability of the model to predict true positives \(TP\) in the data, and is used in binary classification\.   
Recall is important when testing for cancer because it's used to find all of the true positives\. A false positive \(FP\) reflects a positive prediction that is actually negative in the data\. It is often insufficient to measure only recall, because predicting every output as a true positive will yield a perfect recall score\.

**`RecallMacro`**  
The RecallMacro computes recall for multiclass classification problems by calculating recall for each class and averaging scores to obtain recall for several classes\. RecallMacro scores range from 0 to 1\. Higher scores reflect the model's ability to predict true positives \(TP\) in a dataset\. Whereas, a true positive reflects a positive prediction that is also an actual positive value in the data\. It is often insufficient to measure only recall, because predicting every output as a true positive will yield a perfect recall score\.

**`RMSE`**  
Root mean squared error \(RMSE\) measures the square root of the squared difference between predicted and actual values, and it's averaged over all values\. It is used in regression analysis to understand model prediction error\. It's an important metric to indicate the presence of large model errors and outliers\. Values range from zero \(0\) to infinity, with smaller numbers indicating a better model fit to the data\. RMSE is dependent on scale, and should not be used to compare datasets of different sizes\.

Metrics that are automatically calculated for a candidate model are determined by the type of problem being addressed\.
+ Regression: `MAE`, `MSE`, `R2`, `RMSE`
+ Binary classification: `Accuracy`, `AUC`, `BalancedAccuracy`, `F1`, `LogLoss`, `Precision`, `Recall`
+ Multiclass classification: `Accuracy`, `BalancedAccuracy`, `F1macro`, `LogLoss`, `PrecisionMacro`, `RecallMacro`