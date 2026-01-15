#inngrid-rms #machine-learning 

Before learning about GB Trees, I needed to learn about Random Forest and Decision Trees. So here it is,
### Decision Tree
>[!info] Definition
> A decision tree is a **supervised model that is used for regression + classification**
> It works like a inverted tree (or simply like a binary tree), where the entire dataset lies at the root node.
> At each node, a decision is made with 2 outcomes (true/false). Based on this, the dataset is split into smaller subsets.
> Each leaf node represents a classification label or predicted value in regression

##### Significance of Splitting
Splitting is driving force in decision tree. The dataset is divided into subsets based on a feature that maximizes *information gain* or minimizes variance. Since splitting is of such importance, you need to grasp that -
> In decision trees, you are predicting 'Y' based on different input parameters 'X'. Here 'X's are the values to the splitting criteria at each non-leaf node

*What is Information Gain?* - The value of X that helps us make a decision on Y. For example if you are classifying whether it will rain today or not, then the chance of (w/o any other information):
	Raining  = 0.5 
	Not raining = 0.5
However, now if you know that the day is sunny, suddenly the chances of (w/information gain):
	Raining ~ 0.2
	Not raining ~ 0.8

#### Decision Tree Splitting Criteria
The splitting criteria aim to create the purest possible child nodes at each step.

##### 1. Entropy and Information Gain (useful in Classification Trees)
Entropy is measure of randomness and is measured as 
$$
		H(S)=−∑pi * ​log_2^​pi​
$$
where pi​ is the probability of class i in the dataset S

Entropy = 0 : The data split is pure (all elements belong to same class)
Entropy = 1 : The data split is impure (equal number of classes / random split)

Information Gain here will be measured as the *reduction in entropy after data split*
$$
IG=H(S)−∑(S∣Si​) * ​H(Si​)
$$
here, H(S) is the entropy before the split.
Si​ are the subsets created by splitting on a feature.
Si/S​ is the proportion of samples in each subset.

> The feature with maximum information gain is selected for splitting

Example, If a dataset contains 100 samples with 50 "Yes" and 50 "No", entropy is **1 (maximum disorder)**.
##### 2 Gini Impurity (useful in Classification Trees, but faster >>> Entropy)
Gini impurity is a lot simpler than calculating entropy
$$
Gini=1−∑pi^2​
$$

Gini = 0 : The data split is pure
Gini  = 0.5 : The data split is impure
> The feature with the lowest Gini impurity is selected for splitting.

Example, If a dataset contains 80% "Yes" and 20% "No", the Gini impurity is: 1 - (0.8^2 + 0.2^2) = 0.32

##### 3. Variance Reduction (useful in Regression Trees)
Here we calculate variance as:
$$
Var(S)=1/N​∑(yi​−yˉ​)^2
$$
where yi​ is each target value and yˉ​ is the mean.

And variance reduction is calculated as:
$$
Var(S)−∑(S∣Si) * ​Var(Si​)
$$

> The feature that results in the highest variance reduction is chosen for splitting.

### Random Forest

##### Model Ensembling 
- answers the question whether more ML models > less ML models in your situation
- it helps to construct more ML models that<span style="color:rgb(216, 203, 79)"> increase test accuracy and decrease cost storing & training those model</span> 
- **Random forest is a popular ensembling model for decision trees**

> [!info] Random Forest in Model Ensembling
> The idea is simple - we train a bunch of decision trees (hence the name "forest") and each tree produces a vote/result
In case of classification, each tree will product a class label and the label which is in majority will be the final output
In case of regression, different statistical methods can be applied to produce the final output from all individual outputs (you may take arithmetic mean/harmonic mean of all distinct values) 

##### Uncorrelatedness 
It is important that the trees in random forest should be different from each other i.e., they should be "uncorrelated"
Hence, the word "random" in random forest
This is achieved in random forests by providing a sample of training data (and not the entire training data). In decision tree, the entire training data is provided to the root node; but here only (say  25%) random data is provided to each tree to achieve uncorrelatedness
This method is called "*Bootstrap Aggregation (or in short `B-agg-ing`)*"
Another approach to achieve uncorrelatedness is *"Feature Randomness"*. In decision tree, you split the dataset based on all input 'X's and then select the one which results in purest sub-branch (or max. info. gain).
But here you can ignore the info. gain and use different splits for different trees.
A variant of this approach is to change the number of input "X"s to each tree. For example, to determine whether it will rain today or not, if input X = {today's temperature, humidity, wind} then say to tree-1 you work/split only on {temperature, humidity} and in tree-2 only on {wind, humidity}

