# Process Mining: Analyzing the helpdesk process log of an Italian company

This repository contains a data analytics project that utilizes process mining methodology to analyse the real-life event log of a helpdesk process of an Italian company. The dataset contains order requests spaning 4 years from 13th January 2010 to 3rd January 2014.

# Project Overview
The goal of this project is to utilize process mining techniques to:
1. Discover the actual helpdesk process process.
2. Identify inefficiencies and bottlenecks in the process.
3. Generate insights to improve process efficiency.

# Methodology
1. Data Collection
2. Data Quality Check
3. Data Transformation
4. Process Discovery
5. Timing Analysis
6. Users Analysis
7. Suggestions for process improvement

# Technologies Used
1. Python
2. Pandas
3. Graphviz library
4. Jupyter Notebook
5. Microsoft PowerBI

# Data collection
From the initial review of the dataset, it contained 4,580 cases, 14 activities and 21,348 events. The <b>Case ID</b> column was used as the case ID. The <b>Activity</b> column was used as the activities and contained the following 14 activities:
1. Assign seriousness
2. Closed
3. Create SW anomaly
4. DUPLICATE
5. Insert ticket
6. INVALID
7. Require upgrade
8. Resolve SW anomaly
9. Resolve ticket
10. RESOLVED
11. Schedule intervention
12. Take in charge ticket
13. VERIFIED
14. Wait

The dataset contains other metadata which includes:

* Case ID: the case identifier
* Activity: the activity name
* Resource: the resource who performed the action
* Complete Timestamp: the timestamp of the event. Format: YYYY/MM/DD hh:mm:ss.sss
* Variant: case variant
* Variant index: case variant in integer format
* seriousness: a seriousness level for the ticket
* customer: name of the customer
* product: name of the product
* responsible_section: name of the responsible section
* seriousness_2: a sub-seriousness level
* service_level: level of the service
* service_type: type of the service
* support_section: name of the support section
* workgroup: name of the workgroup

# Data quality check and transformation
The event log was reviewed to ensure the data contained were okay to use for process mining. The following was discovered:
1. Python did not automatically identify the date column. This was adjusted.
2. To analyse the end-to-end process, it was important to use completed cases. From initial analysis, we found out that the dataset has 6 different start/end activities. It was assumed that this was due to the way the data was extracted from the system. The dataset was extracted for events from 13th January 2010 to 13th January 2014, without considering if these cases starts or ends on these dates. The analysis carried out indicated that there are two posible start events (Assign seriousness abd Insert ticket) and one end event (Closed). The eventlog was then filtered for cases which meets this criteria. After this, we were left with 4,479 completed cases starting from 13th January 2010 to 3rd January 2014.
3. Since the event log was gotten online, no process owner was contacted to gain further understanding of the process and other columns in the dataset. Therefore, if there are manual events in this process, it is not highlighted here.

# Output and Visualisations
## Data overview
![alt text](https://github.com/nkwachiabel/Process-Mining-Helpdesk-of-an-Italian-coy/blob/main/Images/Data%20Overview.jpg?raw=true)

- This page gives an overview of the dataset. At the top, it shows the number of tickets, activities, events, number of users and number of process variants. In the observation period from 13th January 2010 to 3rd January 2014, 14 activities were identified and 193 variants.
- The area chart at the top right shows the count of activities for each year. Looking at the chart, there was period in September 2012 where there was a peak. This was on 1st September 2012. It indicates that a lot of tickets were closed on that day (a batch operation).
- The event graph shows the events from the most occuring to the least occuring. We can see that <i>Take in charge ticket</i>, <i>Assign seriousness</i>, <i>Resolve ticket</i> and <i>Closed</i> were the most occuring events.
- The variants table shows the variants and the number of cases per variant. We can see that the top 5 variants accounts for 78.65% of the total variants.
- The pie chart shows the number of tickets by products. Majority of the tickets were open for Product Value 1, followed by Product Value 3 and Product Value 2. The three products accounts for 86.61% of all tickets logged. This shows that there are more issues with this products than the other products the company has.
-  Finally, the number of tickets by customer shows how many times a customer logs an issue. Note that the same customer may log issues for the different products used. For example, customer Value 22 has logged over 200 tickets across 10 products. There is also a possibility that the customer is not knowledgeable enough or logging different tickets for different issues.

## Process discovery based on event log data
![alt text](https://github.com/nkwachiabel/Process-Mining-Helpdesk-of-an-Italian-coy/blob/main/Images/Process%20discovery.jpg?raw=true)

This page helps in analysing the actual process based on the filtered event log. It contains various filters including variants, activities, first activity and connections. In understanding the process, different analysis were carried out;

* <b>Recurrence rate</b>: This rate looks at how many times the same customer calls for the same product. We get this number and convert this to a percentage of the total tickets in the eventlog. This shows that of all the tickets raised, approximately 12% of them were for the same product and from the same customer. This can be as a result of various reasons (i) lack of knowledge on the part of the agents to appropriately resolve the customer's issue (ii) the user manual is not descriptive enough to help users solve their issues. Note that this assumes that the customer is calling for the same issue on the same product which might not be the case all the time. A limitation to this analysis is that the dataset did not give more information about the reason for the call because this would be more useful to this analysis.
* <b>First Call Resolution Rate</b>: This is basically the opposite of the recurrence rate. This looks at those tickets that were resolved on the first call. 
* <b>Escalation rate</b>: This rate looks at the amount of cases which requires escalation. Only 2% of all tickets raised requires escalation. While the escalation rate is low, it is surprising that the recurrence rate is high. This reinforces the fact that the low level agents should be trained on when to escalate issues to reduce the recurrence rate.
* <b>Connections table</b>: This table shows all the possible preceding and succeeding activities in the dataset. Here, there are 52 unique connections between 14 activities. This also gives us a glance of where there are movements between activities which should not have happened and how many times they happen. For example, we can see that
    - <i>Resolve ticket</i> was followed by <i>Closed</i> 4,479 times i.e., in all tickets. We can also see that
    - <i>Assign seriousness</i> was followed by <i>Assign seriousness</i> 438 times,
    - <i>Resolve ticket</i> was followed by <i>Resolve ticket</i> 246 times,
    - <i>Wait</i> was followed by <i>Wait</i> 101 times,
    - <i>Take in charge ticket</i> was followed by <i>Take in charge ticket</i> 96 times
    - <i>Closed</i> was followed by <i>Closed</i> 14 times
This shows a possible improvement area in the software to avoid reassigning the same activities to the same ticket multiple times.

* <b>Variant analysis</b>: The variant analysis starts by identifying the trace of activities in sequential order that each order request follows. After this, similar traces were grouped into process variants. This analysis shows that there are 193 different variants. That is 193 different process a ticket follows from start to finish.
  * Out of the 193 variants, 111 variants occured once, 22 variants occured twice and 11 variants occured thrice. Given that we do not have only one variant, this does not mean that a lot of variants are wrong. A ticket may follow various process to be resolved, hence the various variants. However, a lot of variants means that there is a need for a more standardized process.
  * The most occuring variant (Variant 1) occurs in 2,366 cases, followed by Variant 2 in 552 cases, and Variant 3 in 228 cases.
  * As shown in the Data Overview page, The first five variants accounts for 3,523 cases which accounts for approximately 78.65% of all cases.
  
* <b>Process flow visualisation</b>: The process flow graph shows the process flow from the start to finish. The process starts from either <i>Assign Seriousness</i> or <i>Insert ticket</i> and ends with <i>Closed</i> i.e., when a ticket is closed. The process flow diagram here focuses on the top 5 variants highlighting the medium days between activities. The Graphviz library was used to automatically generate a visual process model based on the event log data. As indicated on the diagram, the activity where the most time is spent is moving from <i>Resolve ticket</i> to <i>Closed</i> (30 days). It is also evident that keeping tickets on wait extends the time to resolve a ticket and this may lead to breaches in SLAs.

* <b>Events transition matrix</b>: The aim of this transition matrix shows how the cases moves from one event to another. The row shows the starting event, while the column shows the preceeding events, while the numbers indicates how many times this was done. This gives an alternate view of the connections. For example, we can see clearly here that <i>Closed</i> was followed by <i>Closed</i> 14 times like mentioned above.

The following were noted:

* <b>Sequence-related issues</b>:
 - Goods Issue to Credit Check Denied: There are 40 cases where the credit check was denied after the Goods Issue activity. This indicates a lack of control in the system as goods could be issued to customers who are not credit worthy.
 - Goods Issue to Delivery Block Set: There are 15 cases where the Goods Issue activity was followed by Delivery Block Set activity. This happened a total of 13 times.
 - Delivery Block Set to Create Delivery: There are 134 cases where despite the Delivery Block Set activity was immediately followed by Create Delivery.
 - Delivery Block Set to Goods Issue: This occures in 4 cases.

* <b>Repeated activities</b>: There were several repeated activities. They include the following amongst others:
 - Create delivery
 - Goods issue
 - Change scheduled date
 - Pro forma invoice

* <b>Connections</b>: The connection column below shows the connections between events i.e., from the preceeding activity to the next activity. From this, there are 349 possible connections.

### Unwanted activities
![alt text](https://github.com/nkwachiabel/Process-Mining-Order-to-cash/blob/main/Images/Unwanted%20activities.jpg?raw=true)

Unwanted activities: Looking at the variants above, we can see that there are about 6 major activities in the top 5 variants. They include:
 - Create Sales Order Item
 - Create Invoice
 - Create Delivery
 - Goods Issue
 - Pro forma invoice
 - Create Quotation

The remaining activities are considered Unwanted activities.

From the above, there are 22 unwanted activities which occured 64,509 times in 23,177 orders. 35% of cases includes unwanted activities. The unwanted activities by business area shows that unwanted activities occurs more in Business Area 7000. Of the unwanted activities, the top three includes <i>Change Scheduled date</i> occurs the most (47.66%), followed by <i>Remove Reason for Rejection</i> (17.72%) and <i>Credit Check Release</i>. In addition to this, the median duration of cases with these unwanted activities is 9 days, compared to 5 days with cases without these activities. This indicates another area of Value improvement.


### Changes
![alt text](https://github.com/nkwachiabel/Process-Mining-Order-to-cash/blob/main/Images/Changes.jpg?raw=true)

There are changes which occur in the eventlog. This changes causes delays to the order requests. There are 11 change activities, 35,426 change events and they occur in 14,775 orders. The most frequent change activity is <i>Change Scheduled date</i> which occurs approx. 87%. The medium order without the changes is 5 days compared to 12 days for those cases with change activities. This indicates that these changes cause delays in fulfilling the order requests and these changes should be minimised to ensure a high customer satisfaction rate.

### Process benchmarking
![alt text](https://github.com/nkwachiabel/Process-Mining-Order-to-cash/blob/main/Images/Process%20Benchmarking.jpg?raw=true)

The process benchmarking page helps to compare different process variants using various filters; Business Area, Division, Changes. It also contains some metrics such as the median order fulfilment time, and how many orders in the selected variant. 

For example, in the above we can see that while the median order fulfilment time in Variant 2 is 0 days, the median order fulfilment time is 7 days. It can further be stated that cases that starts with <i>Create Delivery</i> activity are completed faster than cases that starts with <i>Create Sales Order Item</i>. A possible explanation can be that for cases that starts with <i>Create Delivery</i> activity, the requested item is available and can be issued immediately, while for the case that starts with <i>Create Sales Order Item</i>, it takes a median of 3 days for the goods to be ready for delivery. Additionally, 2 days delay is introduced between the <i>Pro forma invoice</i> and <i>Create Invoice</i> activity. This may be because of manual approval process inherent between these activities.

## Timing analysis
![alt text](https://github.com/nkwachiabel/Process-Mining-Order-to-cash/blob/main/Images/Timing%20analysis.jpg?raw=true)

This analysis was done to analyse the bottlenecks in the process relating to timing. To know how long a process should last, we used the median duration of the cases as the performance indicator. In the overall order, the median duration was 5 days. About 49% of cases did not meet this time target. When looking at the median time of the indivudual activities, Invoice cancellation takes a median of 12 days. For the transition between events, the connections that takes more time is between Creqte Quotation and Delivery Blocks. They take above 120 days. Apparently, cases which starts with <i>Create Quotation</i> takes more time than other cases. The median case duration for cases which starts with <i>Create Quotation</i> is 69 days, <i>Create Delivery</i> 1 day and <i>Create Sales Order Item</i> is 7 days. This may be as a result of delay in response from the customer when presented with quotes.

## Order details
![alt text](https://github.com/nkwachiabel/Process-Mining-Order-to-cash/blob/main/Images/Order%20details.jpg?raw=true)

This page shows information relating to a particular case by using the filter at the top right of the screen.

## Open orders
![alt text](https://github.com/nkwachiabel/Process-Mining-Order-to-cash/blob/main/Images/Open%20orders.jpg?raw=true)

This page shows information relating to incomplete order requests. There are 16,524 open order requests based on the dataset which span across various business areas. From the dashboard above, we can see that the last activity of majority of the open orders is <i>Pro forma invoice</i>. This shows that most of the orders have been executed and awaiting the <i>Create Invoice</i> activity to be completed. This dashboard can be connected to the SAP system and refreshed at intervals to keep track of these open orders at certain points. Due to the introduction of Power Automate in Microsoft PowerBI, a notification can be triggered when a case gets to a particular activity, or a reminder can be sent to the responsible individual if an order has stayed in a particular activity after some days. This will prompt users to complete their tasks.

# Process improvement
Based on the analysis, areas for improvement were identified such as:
* <b>Process redesign</b>: From the process discovery, it can be seen that some controls should be put in place. There are cases where blocks were set but Delivery still occured and goods were issued. Additional controls should be put in place to avoid this.
* Real-time dashboard: A real-time dashboard should be made available for those open cases to be monitored. This dashboard should include key metrics to ensure that they are not deviating from the expected process and reminders should be sent to process owners to ensure compliance. This would help in reducing processing times and improved overall process efficiency.

# Limitation
No process owner was reached out to confirm the validity of the expected process

# Repository structure
* 'Data/': Contains the data used for analysis
* 'Notebook/': Jupyter notebook detailing the data cleaning, process discovery and analysis
* 'Output/': Includes the PowerBI output and a PDF file

# Contributions
Contributions to this repository are welcome! If you encounter any issues or have suggestions for improvement, please open an issue or submit a pull request.

# Contact
For any questions or inquiries, please contact nkwachiabel@gmail.com
