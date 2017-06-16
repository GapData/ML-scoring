
# Machine Learning Scoring

This repo aims to compare the scoring speed of several open source machine learning
libraries. The focus will be on scoring provided via a REST API (via web requests).


## h2o.ai

Trained LR, RF (100 trees, depth 10), GBM (100 trees, depth 10) and NN (2 hidden layers, 200 neurons each) using h2o and exported Java scoring code. Built a prediction service using Steam. 

Scoring sequentially using Python via REST API. TODO: Parallelize the client (server is multithreaded AFAIK).

Round-trip time is about 2ms for all algos. This includes client request, network trip,
server prep and scoring itself. TODO: Get the breakdown. 

Detailed code and results [here](h2o).

It seems all algos are very fast, maybe scoring itself is <1ms for each of them (LR should be orders of magnitude faster than RF/GBM). TODO: Measure scoring time from Java.

TODO: Concurrency/throughput, some attempts [here](https://github.com/szilard/ML-scoring/tree/master/h2o#thoughput). 


## plumber

The `plumber` R package provides a REST API for an R function. There is a ~5ms overhead for using the framework 
(5.8ms measured via the Python client).  

### glmnet with plumber

Trained LR on same data with `glmnet` (actually using `glmnetUtils` that deals with categorical variables doing 
one-hot encoding under the hood and calling `glmnet`).

Code [here](plumber-glmnet).

Latency is ~25ms of which ~5ms should be plumber (see above) and 20ms should be scoring with `glmnetUtils` (but see below).

Further [inspection](https://github.com/szilard/ML-scoring/blob/master/plumber-glmnet/2a-glmnetUtils-scoring_speed.R#L18-L24) 
shows about 14ms doing the one-hot encoding in `glmnetUtils` and 1ms scoring with `glmnet`,
a total of 15ms. TODO: What's the remaining 5ms?

### gbm (the R package gbm) with plumber

Trained `gbm` 100 trees, depth 10. `gbm` deals directly with categorical values (no need for one-hot encoding).

Code [here](plumber-gbm).

Total round-trip only 8ms (6ms for plumber and I measured 2ms for gbm)













