# Designing for the Future: Building a Flexible Event-based Analytics Architecture for Games
> Brent Nash  

## Notes

* Analytics
    * Measure, Understand, Improve
    * Engagement, Monetization, Data-driven
    * Question > Hypothesis > Experiment > Analysis > Conclusion ^
* Flexibility
    * Ambiguous requirements
    * Evolving tech landscape
    * Changing requirements
    * The "awesome prototype conundrum"
* Architecture
    * Telemetry events
        * Unique
        * Point-in-time
        * Self-describing
        * Redundant
    * Format
        * Bandwidth constraints
        * Downstream support
        * Extensibility
        * Example event
            * event: version, id, timestamp, type 
            * app: name, version
            * client: id
            * level: id
            * position: x
            * position: y
    * Data sources
        * Server side events (trusted (mostly))
        * Game client events (untrusted)
            * Engagement
            * Performance (client)
            * Gameplay (local)
    * Workflow
        * Ingest
            * Decouple processing (schema-less)
            * Using Kinesis Stream
        * Architecture
            * Stream Consumers
                * Cold Data Store
                    * Medium: S3
                    * Retention: years
                    * Access: slow
                    * Turn-around: n/a
                    * Structure: semi
                    * Duplicates: ok
                    * Process
                        * Validate
                        * Sanitize
                        * Enrich
                * Warm Data Store
                    * Medium: Redshift 
                    * Retention: months
                    * Access: fast
                    * Turnaround: 1 hour
                    * Structure: yes
                    * Duplicates: no
                    * Process
                        * Dedupe
                * Hot Data Store
                    * Medium: ElasticSearch
                    * Retention: days
                    * Access: very fast
                    * Turnaround: 5 minutes
                    * Structure: yes
                    * Duplicates: no
                    * Process
                        * Dedupe
        * Analyze
            * Custom queries
                * Redshift, Kibana
            * Heatmap tool
                * Built with Python (pg8000, pandas, matplotlib.pyplot)
            * Tableau
* Evolution
    * Crash Reporter > SNS Crash Topic
    * Publish to Kinesis Stream via Lambda
    * New Products
        * Kinesis Analytics
        * Redshift Spectrum
        * QuickSight
        * Athena
* Takeaways
    * Assume things are going to change
    * Abstract and decouple wherever possible
    * Bias toward fan-out (no data dead ends)
    * Run experiments, be agile, learn, and be curious
    * Links
        * [Amazon Game Studios](https://games.amazon.com)
        * [AWS for Gaming](https://aws.amazon.com/gaming/)
