Random Recommender Method
===========
# load recommenderlab
library("recommenderlab")
data(Jester5k)
# Data Properties
Jester5k
# Query registry for recommendation methods 
recommenderRegistry$get_entries(dataType = "realRatingMatrix")
# Create IBCF Recommender System 
r <- Recommender(Jester5k, method = "RANDOM")
# Describe Recommender
r
# Get recommendation systems
names(getModel(r))
# Get top-N model
getModel(r)$topN
# Create top-5 list from IBCF recommender system for users 1001 and 1002
recom <- predict(r, Jester5k[1001:1002,], n=5)
# Describe recommendations
recom
# Show recommended items
as(recom, "list")   
