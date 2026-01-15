#machine-learning 

> [!tldr] 
> Cross Validation allows us to compare different machine learning methods and get a sense of which method will work the best.
> A popular use-case for cross validation - Hyper Parameter Tuning

**What cross validation does?**
The dataset is split into multiple subsets, and the model is trained and tested on these subsets in a systematic way. This allows the evaluation of the model's performance across different data splits, providing a more reliable estimate of its accuracy.
### Common Types of Cross Validation

**K Fold Cross Validation**
- The dataset is divided into _k_ equal-sized subsets (or folds).
- The model is trained on _k-1_ folds and tested on the remaining fold.
- This process is repeated _k_ times, with each fold used as the test set once. The average performance across all folds is calculated.

![[Cross Validation Explained.png|650]]

**Leave-One-Out Cross-Validation (LOOCV)**:
- A special case of K-fold where _k = n_ (number of data points).
- Each data point is used as the test set once while the remaining points are used for training.