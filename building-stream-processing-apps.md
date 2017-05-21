# Building Stream Processing Applications
* presenters: Amit Ramesh, Qui Nguyen
* Yelp, ads team
* Description:

    > Do you have a stream of data that you would like to process in real time? There are many components with Python APIs that you can put together to build a stream processing application. We will go through some common design patterns, tradeoffs and available components / frameworks for designing such systems. We will solve an example problem during the presentation to make these points concrete. Much of what will be presented is based on experience gained from building production pipelines for the real-time processing of ad streams at Yelp. This talk will cover topics such as consistency, availability, idempotency, scalability, etc.

# Plan
1. Why stream processing?
2. Putting an application together.
    1. Example problem
    2. Components and data operations
3. Design principles and tradeoffs
    1. Horizontal scalability
    2. Handling failures
        * Idempotency
        * Consistency versus availability

## Why Stream Processing?
* Data bombarding at us all times, from many places, in many forms.
* Want to extract meaningful data out of these, e.g. "average value in the last minute".

### Data Processing Options
* Batch
    * Was the most natural way of handling events when they were in logs.
    * Finite chunk of data.
    * Operations defined over the entire input.
* Stream
    * Enabled by frameworks like Kafka.
    * Data coming in all the time, continuously processing it.

### Why stream processing over batch?
* Lower latency on results
* Most data is unbounded, so streaming model is more flexible.
* Rolling windows on data.

### Yelp's Evolution
* Tracking ads & clicks on ads
* Reports
* "Nightly jobs" to run over logs, produce reports
    * logs + python + mrjob + hadoop

* App for business users to view metrics of campaigns
* Logs + Python + Spark Streaming

## Putting an application together

### Example: ad campaign metrics
* Events associated w/ ads in many parts of system
* Logs for when ads are served, when they're viewed, and when they're clicked
* Want to combine ads from different parts of the system and get metrics over time

### Components and data operations
* Source of streaming data
* Stream processing engine
* Data sink, e.g. another stream or a storage database
* kafka --> Spark Streaming --> {kafka, cassandra}

### Types of operations
1. Ingestion
2. Stateless transforms: on single events
    * Filtering
    * Projection
3. Stateful transforms: on windows of events
4. Keyed stateful transforms
    * (missing)
5. Publishing

#### Ingestion
* Pull streaming data in for processing
* If kafka is source, need kafka-specific driver to translate data into the pipeline

    from pyspark.streaming.kafka import KafkaUtils

    ad_stream = KafkaUtils.createDirectStream(
        streaming_context,
        topics=['ad_events']
        ...(missing)

#### Operation 2: Stateless Transforms
* Filtering operation- condition where you drop events or let them pass through
    * e.g. a bot blacklist filter
* Projection
    * (missing)

#### Operation 3: Stateful transforms
* Not concerned w/ one event at a time, but a window of events
* Sliding windows with overlaps or tumbling windows
* Aggregation- summing values

    aggregated_stream = event_stream.reduceByWindow(
        func=operator.add,
        windowLength...
        (missing)

#### Operation 4: Keyed stateful transforms
* Group events by key (shuffle) within each window before transform
* Windowing --> Shuffle --> Transform
* e.g. aggregate views by campaign_id

    code (missing)

* e.g. Can also be on more than one stream, e.g. join by id
    * window --> shuffle --> join
    * e.g. a consolidated view of everything that's happening

    code (missing)

    * define windows for the two views, join is in the context of those windows

#### Operation 5: Publishing
* Transform to sink with driver

### Putting it all Together
(missing)
* Three streams: Ad, View, Click (each read --> filter --> project)
* Joining stage to bring them together
* Transformations to:
    * Sum by campaign
* Write to sink (database)

## Horizontal scalability
* Design such that data can be broken into discrete chunks and redistributed
* Only need to throw in more machines or resources; design should remain unchanged
* Why?
    * Data will grow over time
    * May have cyclical peaks and troughs- save some $ during troughs
* How?
    * Random partitioning- Divvy up into multiple partitions
        * First three stages are stateless transforms, so they can scale well
    * Key partitioning- Use hashing to bucket things
        * Watch out for hot spots / data skew
        * e.g. may have two different campaigns which are performing very differently

### Summary
* Random partitioning for stateless transforms
* Keyed partitioning for keyed transforms
* (missing)

## Handling Failures

### Idempotency
* An operation that can be applied more than once with the same result
* Need so that you can reason about retries
    * Transforms like filters, projections, etc have no side effects
    * (missing)
* Some writes (those with unique keys) are idempotent
* Increment writes are **not** idempotent
    * Work around by versioning to only apply operations once

### Idempotency in streaming pipelines
* Both in output to data sink and in local state
* Any event can be reprocessed
* Some frameworks guarantee "once-only" guarantees
    * They do this by keeping track of what they already processed
    * Important to understand how they're guaranteeing this

### Consistency vs Availability
* Tehre is always a tradeoff between these two failures.
* Consistency- every read sees a current view of the data.
    * A consistent solution is to reject writes when they can't be synced to the cluster.
    * Not very available.
* Availability- capacity to serve requests.

* Some systems make a specific choice along these lines.
* Others let you be more flexible, for example Cassandra lets you configure # of replicas that must be respond before a write is accepted.
* Usually you want to prioritize availability so your application doesn't shut down in case your sink or source becomes slightly unavailable.

* May need to introduce retries to your application so you can handle failure.

## Conclusion
* Stream processing: data processing with operations on events or windows of events
* Horizontal scalability, as data will grow and change over time
* Handle failure appropriately
    * Keep operations idempotent (missing)

