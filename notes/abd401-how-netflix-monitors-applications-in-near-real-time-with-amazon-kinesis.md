# How Netflix Monitors Applications in Near Real-Time with Amazon Kinesis
> Roy Ben-Alta  
> John Bennett  

## Notes

### Amazon Kinesis

* Session Topics
    * Kinesis at Amazon
        * AWS Metering
        * Amazon S3 Events
        * Amazon CloudWatch logs
        * Amazon.com's catalog
    * Batch processing
        * Hourly server logs
        * Weekly or monthly bills
        * Daily website clickstream
        * Daily fraud reports
    * Stream Processing
        * Real time metrics
        * Real time spending alerts/caps
        * Real time clickstream analysis
        * Real time detection
    * Real-time Analytics
        * Kinesis Video Streams
            * Capture, process, and store video streams for analytics
        * Kinesis Data Stream
            * Build custom applications that analyze data streams
        * Kinesis Data Firehose
            * Load data streams into AWS data stores
        * Kinesis Data Analytics
            * Analyze data streams with SQL
    * Use cases
        * Operation intelligence
        * Security intelligence and event management
        * Application performance and monitoring
        * Business analytics
    * [CloudTrail Event Logs](http://amzn.to/2ApHXKr)

### Netflix

* What is wrong with the network?
    * It's not the network!
* Why is the network so slow?
    * Faraway places are far away!
* My service can't connect to its dependencies.
    * Distributes systems are hard.
* Netflix is big
    * 104 million
    * 190 counties
    * 100s of microservices
    * 1,000s of deployments
    * > 100,000 instances
* Challenges
    * No access to the underlying network
    * Large traffic volume
        * Billions of flows per day
        * Gigabytes of logs per second
    * Dynamic environment
        * Logs are limited
        * IP addresses are randomly assigned
        * IP metadata varies over time, unpredictable
* Goal
    * Develop a new data source for network analytics
    * Enable ad-hoc OLAP-style queries
    * Add observability to network
* Solutions
    * AWS VPC Flow Logs
        * Good: wide coverage of network traffic
        * Good: consolidated
        * Good: core info (IP-to-IP
        * Bad: 10 minute capture window
        * Ugly: stateless
    * Dredge
        * Enriched and aggregated multi-dimensional network traffic data
        * Strong AWS Integration
            * Enables experiment
            * Load streaming data differently
            * Batch with Kinesis Firehose
                * Store in S3, Process with Lambda
            * Stream with Kinesis Streams
        * Identifies high-latency network flows
    * Salp
        * Distributed tracing
        * Similar to Google's Dapper
        * Naive sampling
        * JVM-centric
    * 
* Kinesis Streams
    * Cross-Account Log Sharing
        * Multiple accounts per region > Single region Kinesis Stream
        * Handles event data at scale
    * Total Cost of Ownership
        * Very little operational overhead
        * No overhead for Amazon Kinesis Firehose
    * Limitations
        * Per-shard limit
            * Increase shard count or fan out to other streams
        * No log compaction
            * Up to 7 day max retention
            * Manual snapshots
            * Not ideal for changelog joins
* Stream Processing Patterns
    * Batch
        * VpcFlowLogEvent > process > EnrichedVpcFlowLogEvent
            * Delay: 24 hours
            * Bounded, fix sized input
            * Measured by throughput
        * Limitations
            * Remote DB: RTT, parallel queries could overload
            * Local cache: Depends of distribution of data, how to handle invalidation
            * Local DB: More effective, less contention and not network RTT
    * Batch + DB
        * VpcFlowLogEvent > Storage > Batch processor (< MetadataChangeEvent) > EnrichedVpcFlowLogEvent
    * Stream + DB
        * VpcFlowLogEvent > Stream processor (< MetadataChangeEvent) > EnrichedVpcFlowLogEvent
            * Delay: 7 minutes
            * Unbounded input as events happen
            * Measured by how far consumer is behind
            * Limitations (same as Batch)
    * Stream + DB + Cache
        * VpcFlowLogEvent > Stream processor (< Cache < MetadataChangeEvent) > EnrichedVpcFlowLogEvent
    * Stream + Cache + Stream
        * Derived Data
            * Database indexes, caches, materialized views
            * Transformed form source of truth
            * Optimized for read queries, improve performance
            * Built from a changelog of events
        * Change Data Capture
            * Log-based message broker to send change events
            * Expose changelog stream as first class citizem
            * Consume and join streams instead of querying the DB
            * Alternative view to query efficiently
                * Update when data changes
                * Removes network RTT, resource contention
                * Pre-computed cache
* Results
    * 7 million network flows
        * Enriched per second
    * 5 minutes
        * Average delay from network flow occurrence
    * 1 Kinesis Stream
* Security Use Cases
    * User network dependencies to audit security groups
    * Only source of logs for Security Group rejected flows
    * Reports communication with public internet
        * Threat detection, port scanning, etc
        * AWS resources (instances, load balancers) with increased exposure
    * Risk profiles
* Future
    * VPC Flow Logs give us a 10,000 ft view
    * More detail and context
        * Kernel-level metrics
    * Dynamic sampling rates
    * Minimize variability
        * Coordination
