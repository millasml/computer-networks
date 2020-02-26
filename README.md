# Predicting Computer Network Down Times
decided to start some analysis on this data set to check out if I could predict network down times. Got kinda interested in networks because I have been taking a computer networking course this semester, and I thoroughly enjoy it!

dataset:    https://www.kaggle.com/crawford/computer-network-traffic

## A Quick Summary of The Dataset
The dataset covers 10 local workstation IPs over a three month period. Half of these local IPs were compromised at some point during this period and became members of various botnets. The dataset includes the flow of each IPs with various ASNs over this time period

we know that there are 5 events of "odd" activity or suspicions. They are:
* IP 1 on 08-24
* IP 3 on 09-26
* IP 4 on 09-18
* IP 5 on 09-04
* IP 6 on 09-26

## First Steps - Exploratory Data Analysis
Do check out the EDA jupyter notebook for a more detailed breakdown, and to check out the plots I created to come to these conclusions. 

First, some quick analysis brought these observations:
* Anomalies usually arises when there is a sudden stop in flow, or a sudden spike in flow (IP 4)
* On the day that there were two anomalies, they were caused by the same ASN

However, we want to detect anomalies before they happen. To do this, we have to look at the trend of flows from a few days before. These were a few observations that we got:
* Before the anomaly happens, it is easy to identify a few ASNs that register a sudden spike in flow or a sudden drop in flow. 

And just some general takeaways:
* Each IP registers very different average flows. Hence, to detect sudden spikes or drops, it would make more sense to use percentage differences, rather than 

## The Decision to Not Use ML
Firstly, a dataset of 10 IPs and 3 months is really small. This dataset would not be sufficient to make a reasonable ML model. 
Secondly, there are only 5 anomalies detected on this dataset. With only 5 anomalies, the dataset is too imbalanced for any reasonable ML model to work well on.
Lastly, while we know the IPs and dates the anomalies arises, we don't know the exact time the machine was compromised, and we also do not know exactly which ASNs caused the anomalies for sure. With this, the data set isn't exactly precise enough for a ML model to train on.

## The Logic
The logic that I have developed works like this:
1. Keep track of the flows for the different ASNs of the different IPs each day.
2. If a new ASN joins, mark it as a possible threat. Record the IPs that register flows from this ASN too.
3. If any IP registers a drop/ spike in flow that exceeds 100% of the previous day, or a flow of zero, report it, along with the main ASNs that contribute to this flow.
4. If any new ASN suddenly registers a huge spike in flows, report it too.

What we get is a list of possible threats each day, identifying which IP and corresponding ASNs it could be

## Evaluation of The Logic
This model's goal is to identify the anomalies before the network goes down. As such, it does report a few false positives.
The main merit of this model is that it is able to identify possible threatening ASNs along with the compromised IP, hence narrowing down the search efforts by a lot.

## Conclusions
A simple model made usind day-over-day differences was created, based on some simple EDA. 
This dataset was fun to work with, but I would have really preferred if we could get a large set of IPs and a longer time frame. It being a time series data, I would love to try some LSTM, ARIMA or Regression Trees on this set and see how it performs. 