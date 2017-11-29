# ElastiCache Deep Dive: Best Practices and Usage Patterns
> Michael Labib

## Notes

### AWS ElastiCache: Redis

* ElastiCache overview
    * High-performance
    * Fully managed
    * Hardened by Amazon
    * Redis 3.2.10
        * Ridiculously fast (<1ms commands)
        * Persistence
        * Highly available
        * Atomic operations
        * Powerful (~200 command)
        * Utility data structures 
        * In-memory data structure server
        * Simple
        * Lua scripting
        * Geospatial queries
        * Pub/Sub
        * Encryption in-transit
        * Encryption at-rest
        * Redis AUTH
    * Optimized swap memory
    * Dynamic write throttling
    * Smoother failovers
    * Cluster Mode
        * Failover: 15-30 sec
        * Failover risk: writes affected, reads available
        * Performance: scales with cluster size
        * Max connections: primaries 975,000, replicas 4,875,000
        * Storage: 6+ TB
        * Cost: Smaller nodes but more $$
        * Automatic client-size sharding
            * 16,384 hash slots per cluster (CRC16(key) mod 16384)
* Scaling your cluster with online re-sharding
        * Resizing
        * Backup and restore (old)
            * Create snapshot
            * Copy snapshot
            * Create replication group from snapshot
        * Zero-downtime online re-sharding (new)
            * Modify replication group with node count
        * CloudWatch + SNS + Lambda for auto resizing 
* Common usage patterns
    * Session management
    * IOT
    * Social media (sentiment analysis)
    * Database caching
    * Streaming data analytics (filtering/aggregation)
        * Kinesis > Lambda (> ElastiCache > Lambda) > Kinesis
    * Standalone database
    * APIs
    * Pub/Sub
    * Leaderboards
* Best practices
    * Cluster sizing
        * Storage
            * Recommended: memory needed + 25% reserved memory + 10% growth (optional)
            * Optimize using eviction policies and TTLs
            * Scale up or out before reaching max-memory using CloudWatch alarms
            * Use memory optimized nodes for cost efficiency (R4 support)
        * Performance
            * Benchmark operations using Redis Benchmark tool
                * READIIOPS: add replicas
                * WRITEIOPS: add shards (scale out)
                * Network IO: use network optimized instances and scale out
            * Use pipelining for bulk read/writes
            * Consider Big(o) time complexity for data structure commands
        * Cluster isolation (app sharing key space)
            * Identify what kind of isolation is needed base on the workload and environment
    * Redis max-memory policies
        * noeviction: return errors when memory limit has been reached
        * allkeys-lru: evict keys trying to remove the LRU keys first
        * volatile-lru: evict keys trying to remove the LRU keys first, but only among keys that have an expiration
    * ElastiCache CloudWatch metrics to monitor
        * CPU:
            * Memcached: up to 90%
            * Redis: divide by cores
        * SwapUsage: low
        * CacheMisses/CacheHit Ration: low/stable
        * Evictions: near zero
        * CurrConnections: stable
        * Maxclients: 65000
        * Databases: 16 for non-clustered mode
        * Reserved Memory: 25%
        * Maxmemory-policy
    * Caching Tips
        * Understand the frequency of change of underlying data
        * Set appropriate TTLs on keys that match that frequency
        * Choose appropriate eviction policies that are aligned with application requirements
        * Isolate your cluster by purpose
        * Maintain cache freshness with write-throughs
        * Monitor Cache HIT/MISS ratio and alarm on poor performance
        * Use failover API to test application resiliency
