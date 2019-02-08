Credit Card Fraud Detection System
=============================
Real time fraud detection is becoming more and more important in today's digital world. Depending on a set of static rules arent enough in effective and proactively stopping fraud even before it occurs. Dynamic fraud patterns and analytics are important to make this possible. A very important aspect to this is identifying fraud patterns with temporal nature. Consider the example of a transaction, that happens in the US and a transaction that happens in the UK. Both of these events are discrete events which do not reveal any fraud behavior. But consider, them happening within a specific timeframe of lets say 5 mins. This shows a fraud behavior. This demo is aimed at providing a similar solution. The CEP engine looks to see if there were more than a specified number of transaction in a given time frame to identify potential fraudulent transactions.




Architecture
=============================
This demo demonstrates a complete end to end solution for real time fraud detection. The core logic of this demo is derived off the blog 

https://developers.redhat.com/blog/2018/07/26/detecting-credit-card-fraud-with-red-hat-decision-manager-7/

The CEP rule engine inspects transactions in a time window to identify potential fraudulent transactions. The event stream is created using kafka. This simulates the real-life events based on customer usage behavior. Once the fraudulent transactions are identified, it sends out the information so that a fraud investigation case can be created and acted upon. 


![alt text](https://github.com/jbossdemocentral/rhpam7-credit-card-fraud-detection/blob/master/docs/demo-images/demo_arch.png)

Software
=============================

This demo runs on OpenShift, and therefore requires an available OpenShift runtime. This can be a OpenShift Container Platform instance, a Minishift instance, etc. The only requirement is that there are enough resource available to run the 4 OpenShift pods that this demo consists off.


Installation
=============================
This demo can be installed on Red Hat OpenShift in various ways. We'll explain the different options provided.

All installation options require an oc client installation that is connected to a running OpenShift instance. More information on OpenShift and how to setup a local OpenShift development environment based on the Red Hat Container Development Kit can be found [here](https://developers.redhat.com/products/cdk/overview/).

Automated installation
=============================

This installation option will install the demo in OpenShift using a single script, after which the demo project needs to be manually imported.

Download and unzip or clone this repo.

Run the init-openshift.sh (Linux/macOS) or init-openshift.ps1 (Windows) file. This will create a new project and application in OpenShift and deploy the demo.

Login to your OpenShift console. For a local OpenShift installation this is usually: https://{host}:8443/console


Scripted installation
=============================

This installation option will install the demo in OpenShift using the provided provision.sh (Linux/macOS) or provision.ps1 (Windows) script, which gives the user a bit more control how to provision to OpenShift.

Download and unzip. or clone this repo.

In the demo directory, go to ./support/openshift. In that directory you will find the provision.sh (Linux/macOS) and provision.ps1 (Windows) script.

Run ./provision.sh -h (Linux/macOS) or ./provision.ps1 -h (Windows) to inspect the installation options.

To provision the demo, with the OpenShift ImageStreams in the project's namespace, run ./provision.sh setup rhpam7-case-mgmt --with-imagestreams (Linux/macOS) or ./provision.sh -command setup -demo rhpam7-case-mgmt -with-imagestreams (Windows)

---
**NOTE**

The with-imagestreams parameter installs the Process Automation Manager 7 image streams and templates into the project namespace instead of the openshift namespace (for which you need admin rights). If you already have the required image-streams and templates installed in your OpenShift environment in the openshift namespace, you can omit the with-imagestreams from the setup command.

---


A second useful option is the --pv-capacity (Linux/macOS)/ -pv-capacity (Windows) option, which allows you to set the capacity of the Persistent Volume used by the Business Central component. This is for example required when installing this demo in OpenShift Online, as the Persistent Volume Claim needs to be set to 1Gi instead of the default 512Mi. So, to install this demo in OpenShift Online, you can use the following command: ./provision.sh setup rhpam7-oih --pv-capacity 1Gi --with-imagestreams (Linux/macOS) or ./provision.ps1 -command setup -demo rhpam7-oih -pv-capacity 1Gi -with-imagestreams (Windows).

After provisioning, follow the instructions from above "Automated installation", starting at step 3.

To delete an already provisioned demo, run ./provision.sh delete rhpam7-oih (Linux/macOS) or ./provision.ps1 -command delete -demo rhpam7-oih (Windows).

Demo Walkthrough
=============================

Events are created using a simple python event emitter. The event processor uses vertx kafka consumer to read the kafka stream and invoke the decision engine for rule evaluation, it then invokes a blocking call for invoking the case creation process using a remote call out to the case management engine.

A full walkthrough script of the demo can be found [here](https://docs.google.com/document/d/1GhRoBTIA2CuYmdqXgJxQGcp3HJuZmypUQouuKNcrsWk).

The case management process is a simple 3 milestone process as seen below:


![alt text](https://github.com/jbossdemocentral/rhpam7-credit-card-fraud-detection/blob/master/docs/demo-images/case_mgt.png)

CEP Usecase
=============================
When a credit-card transaction enters the system, fetch the context of that transaction from a datastore, where the context is defined as an {x} number of previous transactions of the same credit card. When, within the last 15 minutes of the current transaction, there were three or more additional transactions with the same card, and of those transactions, at least two were within 10 seconds of each other, raise a ‘potential fraud’ alert.

Enhancements
=============================

- The event processor can be made to run within a spark context if there needs to be a requirement to scale. This can also be used in case of need to use training models in the decision making process.
- The Case Management process is a simple 3 milestone process, this can be extended to fit real-life usecases. Also, for the scope of the demo, the various steps in the process are simple script tasks. This would be replaced by service tasks where necessary.

Project Sources
=============================
https://github.com/snandakumar87/decisionManagerCreditCardFraud
https://github.com/snandakumar87/eventEmitterCreditTransactions
https://github.com/snandakumar87/FraudCaseManagementWorkflow


