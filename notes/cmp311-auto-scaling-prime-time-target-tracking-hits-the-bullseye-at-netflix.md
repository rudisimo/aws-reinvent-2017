# Auto Scaling Prime Time: Target Tracking Hits the Bullseye at Netflix
> Vadim Filanovsky  
> Anoop Kapoor  
> Tara Van Unen  

## Notes

### AWS

* AWS Customer survey 2016
    * Top Picks for Auto Scaling
        * Top Gear: Speedy set up
        * Super Size Me: More services
        * Sherlock: Mastering metrics
* Step Scaling Policies
    * Multiple conditions
    * Manual customization
    * Granular control and tuning
* Target Tracking
    * No conditions
    * Self-optimizing
    * Fastest scaling
* More Services
    * DynamoDB > Tables and global secondary indices > Provisioned capacity
    * Aurora > Cluster > Replicas
    * EC2 Spot Fleet > Spot Fleet Request > Sport Instances
    * ECS > Service > Tasks

### Netflix

* Chapter One: The need for Auto Scaling
    * Encoding
        * Perceptually optimal video encoding
        * Encoding jobs run during off-peak (reserved instances)
    * Recommendations
    * Other benefits of Auto Scaling
        * Red-black pushes
        * Regional failover
        * Curing cancer (Hack Day Project)
    * Auto Scaling != Random Scaling
* Chapter Two: Choosing the Metric
    * What metric to use?
        * Throughput per instance: RequestCountPerTarget
            * Pros: direct measure of work; intuitive
            * Cons: Drifts over time
        * Resource utilization per instance: Average CPUUtilization
            * Pros: Requires less adjustment
            * Cons: More oscillation/jitter
    * Auto Scaling on multiple metrics
        * Harder to reason about scaling behavior
        * Different metrics might contradict each other, causing oscillation
        * Typical Setup
            * Normal scale-up and scale-down on throughput
            * Emergency scale-up on CPU (aka "the hammer rule")
* Chapter Three: Setting the target
    * Tools for figuring out the target
        * curl, ab, siege, nghttp, jmeter, gatling...
    * Squeezing with live prod traffic (see image)
    * Understand failures
        * Measure: Throughput, CPU Utilization, Latency
* Chapter Four: Scaling policy setup
    * "No rush" scaling
        * Problem: scaling amounts too small, cooldown too long
        * Effect: Scaling lags behind the traffic, not enough capacity at peak
        * Remedy: Increase scaling amounts or migrate to target tracking
    * Twitchy scaling
        * Problem: Scale up policy is too aggressive
        * Effect: Unnecessary capacity churn
        * Remedy: Reduce scale up amount, increase the number of eval periods or migrate to target tracking
    * Should I stay or should I go
        * Problem: scale-up and scale-down thresholds are too close to each other
        * Effect: Constant capacity oscillation
        * Remedy: Move scale-up and scale-down thresholds farther apart or migrate to target tracking
    * Step vs target tracking setup
        * Target tracking uses a single policy to apply scaling
* Chapter Five: Traffic patterns 
    * Understand the traffic
        * What a difference a day makes
            * Weekday vs weekend traffics
                * ASG is at the lowest size
                * Aggressive traffic ramp-up
                * Can auto scaling keep up?
    * Experiment
        * Target tracking provides a smoother auto scaling curve
* Results
    * Productivity
    * Reliability
    * Cost
