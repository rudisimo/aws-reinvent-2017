# Measuring the Internet in Real Time
> Jorge Vasquez  

## Notes

### CloudFront

* Abstract
    * Speed of light in fiber
        * 100ms RTT from here to Sao Paulo
        * TCP + TLS takes three RTTs, so 300ms before any data is sent
    * Bandwidth
        * Regional ISPs
        * Bandwidth X delay product
* Collecting RTT data
    * CloudFront POP > Kinesis Stream > S3
* Design Patterns
    * Use Kinesis to stream data from many endpoints to a central location
    * Use KCL to consume stream in a scalable fashion
    * Intermediary aggregated results in Amazon S3
    * Use CloudWatch metrics to store health data
    * Use multiple nested feedback loops
    * Precompute data in AWS regions
