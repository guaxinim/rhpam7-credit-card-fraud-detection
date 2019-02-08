Credit Card Fraud Detection System
=============================
Real time fraud detection is becoming more and more important in today's digital world. Depending on a set of static rules arent enough in effective and proactively stopping fraud even before it occurs. Dynamic fraud patterns and analytics are important to make this possible. A very important aspect to this is identifying fraud patterns with temporal nature. Consider the example of a transaction, that happens in the US and a transaction that happens in the UK. Both of these events are discrete events which do not reveal any fraud behavior. But consider, them happening within a specific timeframe of lets say 5 mins. This shows a fraud behavior. This demo is aimed at providing a similar solution. The CEP engine looks to see if there were more than a specified number of transaction in a given time frame to identify potential fraudulent transactions.




Architecture
=============================
This demo demonstrates a complete end to end solution for real time fraud detection. The core logic of this demo is derived off the blog https://developers.redhat.com/blog/2018/07/26/detecting-credit-card-fraud-with-red-hat-decision-manager-7/
The CEP rule engine inspects transactions in a time window to identify potential fraudulent transactions. The event stream is created using kafka. This simulates the real-life events based on customer usage behavior. Once the fraudulent transactions are identified, it sends out the information so that a fraud investigation case can be created and acted upon. 


![alt text](https://github.com/jbossdemocentral/rhpam7-credit-card-fraud-detection/blob/master/docs/demo-images/demo_arch.png)

