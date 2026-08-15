Software Testing
CSC13003
Performance Testing

Content
• What is Performance Testing?
• Why to do Performance Testing?
• Types of Performance Testing
• How to do Performance Testing?
Department of Software Engineering 2

Content
• What is Performance Testing?
• Why to do Performance Testing?
• Types of Performance Testing
• How to do Performance Testing?
Department of Software Engineering 3

What is Performance Testing?
Performance testing is a type of non-functional testing
that ensures software applications to perform properly
under their expected workload.
Department of Software Engineering 4

What is Performance Testing?
• The main goal is
• not to find bugs
• to eliminate performance bottlenecks
• The focus is on
• speed – whether the application responds quickly
• scalability – the maximum user load the application can handle
• stability – if the application is stable under varying loads
Department of Software Engineering 5

What is Performance Testing?
• Common Performance Problems
• Long load time – long initial time to start an application
• Poor response time – delayed output response to an input
• Poor scalability – does not support a large enough number of users
• Bottlenecks – obstacles that degrade overall system performance
Department of Software Engineering 6

What is Performance Testing?
• Example Performance Test Cases
• Verify response time is not more than 4 secs when 1000 users access the website
simultaneously
• Verify response time of the Application Under Load is within an acceptable range
when the network connectivity is slow
• Check the maximum number of users that the application can handle before it
crashes
• Check database execution time when 500 records are read/written simultaneously
• Check CPU and memory usage of the application and the database server under
peak load conditions
• Verify the response time of the application under low, normal, moderate, and
heavy load conditions
Department of Software Engineering 7

What is Performance Testing?
• Performance Testing Metrics
Department of Software Engineering 8

What is Performance Testing?
• Performance Testing Metrics
• CPU utilization – percentage of CPU capacity utilized
• Memory utilization – utilization of the primary memory
• Response times – time between sending request and receiving response
• Average load time – time to complete the loading process
• Throughput – the number of transactions can be handled in a second
Department of Software Engineering 9

What is Performance Testing?
• Performance Testing Metrics
• Average latency/Wait time – the time spent by a request in a queue before
getting processed
• Bandwidth – the volume of data transferred per second
• Requests per second – the number of requests handled per second
• Error rate – the percentage of requests resulting in errors
• Transactions Passed/Failed – the percentage of passed/failed transactions
Department of Software Engineering 10

Content
• What is Performance Testing?
• Why to do Performance Testing?
• Types of Performance Testing
• How to do Performance Testing?
Department of Software Engineering 11

Why to do Performance Testing?
• Most users click away after 8 seconds of delay
• $4.4 billion business revenue loss due to poor
web applications performance
• Aberdeen found that inadequate performance
could impact revenue by up to 9%
• Business performance begins to suffer at 5.1
seconds of delay in response times of web
applications and 3.9 for critical applications
Department of Software Engineering 12

Why to do Performance Testing?
| Only a                                         | 5-minute downtime                          | of Google.com | (19-Aug-13) is  |
| ---------------------------------------------- | ------------------------------------------ | ------------- | --------------- |
| estimated to cost the search giant as much as  |                                            |               | $545,000.       |
| It’s estimated that companies lost sales worth |                                            |               | $1100 per       |
| second                                         | due to a recent Amazon Web Service Outage. |               |                 |
Department of Software Engineering 13

Department of Software Engineering 14

Why to do Performance Testing?
• Help ensure the software
• meet the expected levels of service
• provide a positive user experience
• Highlight improvements relative to speed, stability, and scalability
• Absence of testing might suffer from different types of problems
that lead to a damaged brand reputation
Department of Software Engineering 15

Content
• What is Performance Testing?
• Why to do Performance Testing?
• Types of Performance Testing
• How to do Performance Testing?
Department of Software Engineering 16

Types of Performance Testing
Department of Software Engineering 17

Types of Performance Testing
Load testing • It checks the product’s ability to
perform under anticipated user
Endurance testing loads
• The objective is to identify
Stress testing performance congestion before the
software product is launched in
Volume testing market
Spike testing
Scalability testing
Department of Software Engineering 18

Types of Performance Testing
Load testing • It is performed to ensure the
software can handle the expected
Endurance testing load over a long period of time
Stress testing
Volume testing
Spike testing
Scalability testing
Department of Software Engineering 19

Types of Performance Testing
Load testing • It involves testing a product under
extreme workloads to see whether
Endurance testing it handles high traffic or not
• The objective is to identify the
Stress testing breaking point of a software
product
Volume testing
Spike testing
Scalability testing
Department of Software Engineering 20

Types of Performance Testing
Load testing • In volume testing large number of
data is saved in a database and the
Endurance testing overall software system’s behavior
is observed
Stress testing • The objective is to check product’s
performance under varying
Volume testing database volumes
Spike testing
Scalability testing
Department of Software Engineering 21

Types of Performance Testing
Load testing • It tests the product’s reaction to
sudden large spikes in the load
Endurance testing generated by users
Stress testing
Volume testing
Spike testing
Scalability testing
Department of Software Engineering 22

Types of Performance Testing
Load testing • In scalability testing, software
application’s effectiveness is
Endurance testing determined in scaling up to support
an increase in user load
Stress testing • It helps in planning capacity
addition to your software system
Volume testing
Spike testing
Scalability testing
Department of Software Engineering 23

Content
• What is Performance Testing?
• Why to do Performance Testing?
• Types of Performance Testing
• How to do Performance Testing?
Department of Software Engineering 24

How to do Performance Testing?
Department of Software Engineering 25

How to do Performance Testing?
|                                    | Determine    |             | Configure  |             |           |              |
| ---------------------------------- | ------------ | ----------- | ---------- | ----------- | --------- | ------------ |
| Identify test                      |              | Plan and    |            | Implement   |           | Analyze and  |
|                                    | performance  |             | test       |             | Run tests |              |
| environment                        |              | design      |            | test design |           | retest       |
|                                    | criteria     | environment |            |             |           |              |
| Step 1 – Identify test environment |              |             |            |             |           |              |
• Identify the testing environment and know what testing tools are available
at your disposal
• Understand the details of all the hardware, software and different network
configurations ahead of time
Department of Software Engineering 26

How to do Performance Testing?
|                                         | Determine    |             | Configure  |             |           |              |
| --------------------------------------- | ------------ | ----------- | ---------- | ----------- | --------- | ------------ |
| Identify test                           |              | Plan and    |            | Implement   |           | Analyze and  |
|                                         | performance  |             | test       |             | Run tests |              |
| environment                             |              | design      |            | test design |           | retest       |
|                                         | criteria     | environment |            |             |           |              |
| Step 2 – Determine performance criteria |              |             |            |             |           |              |
• Identify the general performance metrics
• Identify the performance success criteria
Department of Software Engineering 27

How to do Performance Testing?
|                          | Determine    |             | Configure  |             |           |              |
| ------------------------ | ------------ | ----------- | ---------- | ----------- | --------- | ------------ |
| Identify test            |              | Plan and    |            | Implement   |           | Analyze and  |
|                          | performance  |             | test       |             | Run tests |              |
| environment              |              | design      |            | test design |           | retest       |
|                          | criteria     | environment |            |             |           |              |
| Step 3 – Plan and design |              |             |            |             |           |              |
• Identify key scenarios by considering
• user variability
• test data
• plan performance
• Simulate a variety of use cases
• Outline what metrics will be gathered
Department of Software Engineering 28

How to do Performance Testing?
|                                     | Determine    |             | Configure  |             |           |              |
| ----------------------------------- | ------------ | ----------- | ---------- | ----------- | --------- | ------------ |
| Identify test                       |              | Plan and    |            | Implement   |           | Analyze and  |
|                                     | performance  |             | test       |             | Run tests |              |
| environment                         |              | design      |            | test design |           | retest       |
|                                     | criteria     | environment |            |             |           |              |
| Step 4 – Configure test environment |              |             |            |             |           |              |
• Arrange all the necessary testing tools and monitoring resources
| Step 5 – Implement test design |     |     |     |     |     |     |
| ------------------------------ | --- | --- | --- | --- | --- | --- |
• Design performance tests according to performance criteria and metrics
Department of Software Engineering 29

How to do Performance Testing?
|                    | Determine    |             | Configure  |             |           |              |
| ------------------ | ------------ | ----------- | ---------- | ----------- | --------- | ------------ |
| Identify test      |              | Plan and    |            | Implement   |           | Analyze and  |
|                    | performance  |             | test       |             | Run tests |              |
| environment        |              | design      |            | test design |           | retest       |
|                    | criteria     | environment |            |             |           |              |
| Step 6 – Run tests |              |             |            |             |           |              |
• Execute and monitor the performance tests
Step 7- Analyze and retest
• Analyze the finding and fine tune the test again to see an increase or
decrease in performance
• Run the tests again using the same or different parameters
Department of Software Engineering 30
