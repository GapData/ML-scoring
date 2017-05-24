
## Machine Learning Scoring

This repo aims to compare the scoring speed of several open source machine learning
libraries. The focus will be on scoring provided via a REST API (via web requests).


### h2o.ai

Trained LR, RF, GBM and NN using h2o and exported Java scoring code. Built a prediction service using Steam. 

Scoring sequentially using Python via REST API. TODO: Parallelize the client (server is multithreaded AFAIK).

Round-trip time is about 2ms for all algos. This includes client request, network trip,
server prep and scoring itself. TODO: Get the breakdown. 

It seems all algos are very fast, maybe scoring itself is <1ms for each of them (LR should be orders of magnitude faster than RF/GBM). TODO: Measure scoring time from Java.
