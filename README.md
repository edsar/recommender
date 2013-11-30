Random Recommender Method
===========
# load recommenderlab
> library("recommenderlab")

# load data
> data(Jester5k)

# Data Properties
> Jester5k

# 5000 x 100 rating matrix of class ‘realRatingMatrix’ with 362106 ratings.
# Query registry for recommendation methods 
> recommenderRegistry$get_entries(dataType = "realRatingMatrix")

$IBCF_realRatingMatrix
Recommender method: IBCF
Description: Recommender based on item-based collaborative filtering (real data).

$POPULAR_realRatingMatrix
Recommender method: POPULAR
Description: Recommender based on item popularity (real data).

$RANDOM_realRatingMatrix
Recommender method: RANDOM
Description: Produce random recommendations (real ratings).

$UBCF_realRatingMatrix
Recommender method: UBCF
Description: Recommender based on user-based collaborative filtering (real data).

# Create IBCF Recommender System 
> r <- Recommender(Jester5k, method = "RANDOM")

# Describe Recommender
> r
Recommender of type ‘RANDOM’ for ‘ratingMatrix’ 
learned using 5000 users.

# Get recommendation systems
> names(getModel(r))
[1] "range"

# Get top-N model
> getModel(r)$topN
NULL

# Create top-5 list from IBCF recommender system for users 1001 and 1002
> recom <- predict(r, Jester5k[1001:1002,], n=5)

# Describe recommendations
> recom
Recommendations as ‘topNList’ with n = 5 for 2 users. 

# Show recommended items
> as(recom, "list") 

#[[1]]
[1] "j83" "j75" "j37" "j96" "j22"

#[[2]]
[1] "j99" "j30" "j94" "j76" "j83"

# Evaluate recommender  
# Create evaluation scheme
# 4-fold cross-validation        
> scheme <- evaluationScheme(Jester5k, method="cross", k=4, given=3, 
+                            goodRating=5) 

#Describe scheme
> scheme

# Evaluation scheme with 3 items given
  Method: ‘cross-validation’ with 4 run(s).
  Good ratings: >=5.000000
  Data set: 5000 x 100 rating matrix of class ‘realRatingMatrix’ with 362106 ratings.

# Evaluate recommender method IBCF
# Evaluate top-3 list
> results <- evaluate(scheme, method="RANDOM", n=c(1,3,5,10,15,20)) 
# RANDOM run 
	 1  [0.006sec/1.744sec] 
	 2  [0.006sec/1.874sec] 
	 3  [0.008sec/1.789sec] 
	 4  [0.007sec/1.697sec] 
	 
# Display results
> results
# Evaluation results for 4 runs using method ‘RANDOM’.

# View confusion matrix for first fold
> getConfusionMatrix(results)[[1]] 
    
n        TP      FP      FN      TN PP     recall precision        FPR        TPR
  1  0.1848  0.8152 17.1656 78.8344  1 0.01065105 0.1848000 0.01023483 0.01065105
  3  0.5464  2.4536 16.8040 77.1960  3 0.03149207 0.1821333 0.03080493 0.03149207
  5  0.8920  4.1080 16.4584 75.5416  5 0.05141092 0.1784000 0.05157590 0.05141092
  10 1.8040  8.1960 15.5464 71.4536 10 0.10397455 0.1804000 0.10290071 0.10397455
  15 2.6744 12.3256 14.6760 67.3240 15 0.15414054 0.1782933 0.15474780 0.15414054
  20 3.5760 16.4240 13.7744 63.2256 20 0.20610476 0.1788000 0.20620317 0.20610476
  
# Average results for 4-fold cross-validation
> avg(results) 
    
n        TP      FP      FN      TN PP     recall precision        FPR        TPR
  1  0.1922  0.8078 17.6240 78.3760  1 0.01078638   0.19220 0.01020150 0.01078638
  3  0.5610  2.4390 17.2552 76.7448  3 0.03148779   0.18700 0.03080172 0.03148779
  5  0.9334  4.0666 16.8828 75.1172  5 0.05238309   0.18668 0.05135608 0.05238309
  10 1.8454  8.1546 15.9708 71.0292 10 0.10358198   0.18454 0.10298328 0.10358198
  15 2.7510 12.2490 15.0652 66.9348 15 0.15440694   0.18340 0.15469058 0.15440694
  20 3.6870 16.3130 14.1292 62.8708 20 0.20693633   0.18435 0.20601384 0.20693633
  
# ROC curve plot
> plot(results, annotate=TRUE)

# Precision-recall plot
> plot(results, "prec/rec", annotate=TRUE)
