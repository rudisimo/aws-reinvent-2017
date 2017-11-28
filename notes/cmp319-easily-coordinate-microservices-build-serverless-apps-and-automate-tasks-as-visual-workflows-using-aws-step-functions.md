# Easily Coordinate Microservices, Build Serveless Apps, and Automate Tasks as Visual Workflows using AWS Step Functions
> Andy Katz  
> Scott Triglia  

## Notes

### AWS Step Functions
> [Resources](https://aws.amazon.com/step-functions/reinvent/)

* Benefits
    * Productivity
        * Quickly create apps by easily connection distributed components
    * Agility
        * Diagnose and debug problems faster
    * Resilience
        * Ensure availability at scale, with fully-managed operations
* Build workflows using seven state types
    * Task
    * Choice
    * Fail
    * Parallel
* Long running tasks (up to a year)
* Monitor from the console, CLI or API
* Use cases
    * Automate daily, weekly, monthly tasks
        * home24: Coordinating daily imports of marketing data
        * theguardian: ETL for subscription fulfillment
        * AWS Blog Posts
            * Weekly EBS snapshot management
            * Monthly Amazon S3 bucket sync ([link](https://aws.amazon.com/blogs/compute/synchronizing-amazon-s3-buckets-using-aws-step-functions/))
            * Monthly Amazon S3 bucket sync ([link](https://aws.amazon.com/blogs/compute/automate-your-it-operations-using-aws-step-functions-and-amazon-cloudwatch-events/))
    * Coordinate components of distributed applications
        * skycatch: A scalable image processing platform for drone data
        * yelp: subscription billing
    * Build microservices
        * EllieMae: Self-service performance testing and infrastructure management
        * outsystems: Delivering a better customer experience while reducing costs

### Yelp
> [Blog Post](https://engineeringblog.yelp.com/2017/11/breaking-down-the-monolith-with-aws-step-functions.html)

* Subscription Billing
    * Monthly invoicing for advertising accounts
    * Business critical, happens at scale (>100k accounts)
    * Older code, evolved over ~10 years
* Benefits
    * Concurrency: easily modify parallelism factor with concurrent executions
    * Enhanced observability: Explicitly represent and handle failure nodes
    * Flexible timeouts
    * Performance gains: internal parallelism
