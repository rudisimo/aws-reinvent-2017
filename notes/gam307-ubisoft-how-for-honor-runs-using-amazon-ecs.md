# Ubisoft: How For Honor Runs Using Amazon ECS
> Louis-Michel Gélinas  
> Ralf Mueller  

## Notes

* Trail Blazing
    * Fail Fast! Success Consistently!
    * Development Ease
    * Automation
    * Managed Infrastructure 
* Beginning
    * Limited cloud experience
    * Desire to use cloud infrastructure
    * Buy-in from team
    * Limited support from internal partners
    * Small DevOps team
    * No CD for console development
* Prototype
    * Play framework not good enough
    * Elastic Beanstalk too managed 
* Planning
    * 1.5 years before launch
    * Non-mission critical services
        * Faction War
        * Player Information Enrichment
    * Immutable Docker Images
    * Minimize Development Resources
        * Run everything locally
        * Namespace databases
    * Multiple UAT environments
    * Scale out of Production
* Initial Integration
    * Manual infrastructure creation and management
    * ECS task and service management using Python scripts
    * Retrospective
        * Manual infrastructure creation and management
            * Depends on documentation
            * Fear driven opposition to change
            * Manual tweaks untracked
        * ECS task and service management using Python scripts
            * Scripted parts depend on manual setup
            * Hard to orchestrate (no rollback)
* Last Mile
    * Monitoring and alerting with DataDog
    * Logs and metrics
    * Load tests
    * Track key KPIs
* From Trail to Autobahn
    * ECS boot sequence fragility
    * Environments
        * UAT
            * 100s of ECS services in 2-5 clusters
        * Prod
            * 10s of services in 2 clusters
            * 3 static environments (PS4, XBOX, etc)
    * Lambda + SNS for cluster draining
        * [AWS Blog post](https://aws.amazon.com/blogs/compute/how-to-automate-container-instance-draining-in-amazon-ecs)
    * Automate CloudFormation stacks
        * Store templates in VCS
        * Implement CI template updating
        * Benefit from stack updates
        * Promote changes from DEV to PROD 
* Lessons Learned
    * Everything manual is a risk
    * Break-event point of automation changes over time
    * Validate all changes in noncritical but identical setups
    * Service containers are hard to manage (even with placement constraints)
    * Surprising gaps in offerings: Lambdas can duct tape a lot of features
