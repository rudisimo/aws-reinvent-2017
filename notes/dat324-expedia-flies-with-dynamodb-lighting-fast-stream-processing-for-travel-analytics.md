# Expedia Flies with DynamoDB: Lighting Fast Stream Processing for Travel Analytics
> Brandon O'Brien  

## Notes

### Expedia

* Context: Real-Time Travel Analytics @ Expedia
    * Two-sided, multi-product, multi-brand global travel marketplace
    * Summary
        * Over 500k properties on Core OTA platforms
        * 1.5M online bookable HomeAway listings
        * 500+ airlines
        * 25K activities
    * Real-time user clickstream
        * Live view of demand patterns for suppliers
        * Live pricing statistics for shopper
        * Live view of market for internal business & operations
    * Reference Data
        * On-demand reference data needed to unlock richer functional capability
        * Conceptually simple: a key-value lookup and a hash-join
        * In practice, at scale: several constraints & trade-offs to consider
        * Reference domain layer: allow reuse of data in-situ across multiple apps
* Patters for Reference Data in Streaming Systems
    * Requirements
        * Performance: fast, random access, scalable throughput, availability
        * Data Management: durability, mutability, scalable storage
        * Wide System: Reusability, simplicity, operational overhead
    * In-Memory/In-Heap
        * Very fast, but storage is not scalable
        * Slows down streaming processor startup time
        * Updating reference data may be difficult
    * Streaming processor
        * Fast, scalable storage
        * May make streaming processor node deployment/management complex
        * Updating reference data may be difficult
    * Direct service call
        * Goes right to the source - up to date data
        * Durability & scalable storage
        * May or may not be able to handle read-load
    * Redis/ElastiCache
        * Fast & easily updatable
        * Shared data source - across ecosystem of streaming apps
        * Availability & durability story not great
    * Cassandra
        * Fast & highly available
        * Linear scalability for storage & throughput
        * Operational overhead can be high
    * RDB (MySQL)
        * Reduced operational overhead
        * Great durability, reasonable scalability
        * Shared data source
    * RDB + Cache
        * Increased read performance & throughput
        * Shared source - amortize data access across nodes
        * Streaming processor needs to talk to multiple data source, more moving pieces to manage
    * DynamoDB + DAX
        * Fast, available, scalable
        * Low maintenance & easy to update
        * Shared data source
* Streaming Data Systems Architecture Performance with DynamoDB + DAX
    * System & Data Setup
        * DAX
            * 1 x dax.r3.large (13g)
            * TTL: 5min
            * Eventually consistent reads
        * DynamoDB
            * Dataset cardinality: ~1M
            * Small items
        * Per item access times
            * DAX cache hit: <1ms
            * DAX cache miss: 11ms
            * DynamoDB: 10ms
        * Low volume
            * ~80k items/sec
            * Cache hit rate ~80%
            * CPU: 5%
            * DynamoDB RCU: 100
        * High volumen 
            * ~300k items/sec
            * Cache hit rate >99%
            * CPU: 80%
            * DynamoDB RCU: 500
    * Design Benefits
        * Balances Performance/Scalability/Availability/Durability/Mutability
        * In-situ reusability across streaming systems
        * Low operation overgeah
        * Extremely cost effective and performant - if data access pattern sufficiently uniform
    * Performance & Availability Tips
        * BatchGetItem on DAX: 10x-20x speed up. Batch size 25 vs 1
        * DynamoDB client (not DAX), not observed consistent performance improvements with keep-alive
        * Save bandwidth: For large objects, use projection to specific subset of fields you want to return to economize bandwidth
        * Partition dilution: Choose high cardinality partition keys and avoid unnecessary throughput scaling
        * DynamoDB/DAX client is threadsafe, but instantiation time is ~20ms
        * Spark - Catch ProvisionedThroughExceededException, use exponential backoff, pause streaming processor, don't update Kafka offsets, rely on Kafka consumer lag alerting
        * Spark - Singleton LRU cache (Guava) in Executor JVM's to store reference data, to reuse across tasks and RDD partitions
