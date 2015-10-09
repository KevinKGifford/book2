# NetApp

Brian McKean from NetApp presented to the class a data analysis problem he'd
like to get some help on.

Our team members:
* [William Farmer](http://github.com/willzfarmer)
* [Kevin Gifford](http://github.com/kevinkgifford)
* [Parker Illig](http://github.com/pail4944)
* [Robert Kendl](http://github.com/DomoYeti)
* [Andrew Krodinger](http://github.com/drewdinger)
* [John Raesly](http://github.com/jraesly)


# Overview

Listen to his presentation carefully. Write down the answers to the following
questions to show your team's understanding of the basics of the problem.

## What is the problem?

Problem statement: Given the ASUP gathered data, is the data any good?  I.e., is all of the data good?  Are there subsets of the data that is good?  Does the data need to be munged (cleaned)?  Can we detect what subsets of the data are bad?  How do we filter out the subsets of bad data?

Of the remaining good data, what observations can be made?

From a more global perspective: Is there enough data (good data, good+bad data) to distill any meaningful observations? 

A valuable contribution/output would be to identify what additional data is required (could be gathered) to improve the predictive analyses?


## Why is the problem important?

Given valid data, NetApp can make predictions regarding (1) when to replace hardware to provide maximum reliability; (2) Provide optimal (cost and service perspectives) maintenance for quality customer service; (3) Generate cost/benefit analyses related to disk quality/cost product and service offerings.  In short, utilize the data to provide high-value customer service to keep currrent customer base happy and provide opportunities for product and service growth which improves profitability and corporate sustainability.


## What dataset has been made available?

The ASUP daily system reports in a "NetApp.csv" file format.


## What specific questions are being raised?

See:
* "Q1: What is the problem"
* "Our team's specific questions from the Q/A session (below)


# Q/A session

We will run a Q/A session. Before the session, compile a list of questions you
want to ask. Then, teams will take turn to ask Brian follow up questions.
Each team gets to ask one question each time. Write down the questions your team
wanted to ask and the answers you received. If another team happens to ask the
same question, simply write down the answer you heard.

## Q1: What data is actually valid? *Answer:* Unknown.

## Q2: What is the highest priority to fix?  *Answer:* Why do the delta times vary so widely?  Is there a pattern?

## Q3: Clarify what the delta time is?  *Answer:* ASUP reports are on a daily basis - should be approximately the number of seconds in a day which is 86400.

## Q4: Is there any additional related data for the netapp data set?  *Answer:* No, this is all you get.

## Q5: For the ASUP timestamps - while these are in Unix epoch seconds, is there an associated timezone or GMT offest?  *Answer:* No  (follow-on questions learned that ASUP report systems utilize localtime() ).

## Q6: How are the different ASUP reporting systems time synced?  *Answer:* They are not, some ASUP systems use a time (NTP) server, some are set once at initial configuration, some are updated when serviced.

## Q7: Where are the ASUP report generating facilities located geographically (looking for time zone info)?  *Answer:*  Americas and European centric with some in Asia.

## Q8: Serial ID is a system serial number (entire system, not specific hardware components), can we identify bad systems vs. good systems?

# Approach

Based on the information you've obtained during the Q/A session, come up with
plan how your team will tackle this problem.

## How should the dataset be imported into Tableau?

Can import NetApp.csv directly into Tableau; consider cleaning data prior to import.

## How should the work be distributed among the team members?

Each of our team members will chose two specific questions to investigate.

* [William Farmer](http://github.com/willzfarmer): How are the delta times distributed by system?

* [Kevin Gifford](http://github.com/kevinkgifford): (1) Is there any temporal relationship (consistency or inconsistency) that can be observed in regards to "good" and "bad" data (e.g., are any of the issues of bad data related to timing in some fashion)?; (2) Is there a relationship between 'delta time' and 'Release'?

* [Parker Illig](http://github.com/pail4944): Is there a relationship between delta time and SW version?

* [Robert Kendl](http://github.com/DomoYeti):  Is there a relationship between delta time and FW version?

* [Andrew Krodinger](http://github.com/drewdinger): Is there any correlation between which controller sent the data and the median delta time?

* [John Raesly](http://github.com/jraesly):


## What types of charts or visualizations to use to support the answer?

Tableau visualizations: bar charts (delta time vs. Release, SW version, FW version), average/mean and standard deviation of delta time grouped by System, Release, SW version, FW version.


## What questions may be too complex for Tableau and may require custom scripts to be written?

1.  Can Tableau convert Unix epoch seconds into a standard date/time formats, e.g., "Mon Oct  5 19:43:32 MDT 2015"?

2.  Will the input data file size cause problems when importing into Tableau or attempting to push to github.com?

3.  Are the timestamps in localtime or gmtime?

4.  Will Tableau be able to properly filter the data (especially the delta time) properly for meaningful analysis and visualizations?

