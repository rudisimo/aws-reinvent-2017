# Building Serverless Video Workflows
> Tom Nightingale  
> Usma Shakeel  


## Notes

* Media Streaming Workflow
    * Ingest
        * S3 Transfer Acceleration
        * Storage Gateway
        * Import/Export
    * Store
        * Glacier
        * Elemental MediaStore
    * Process
        * Elemental MediaConvert
        * Elemental MediaLive
        * Elemental MediaPackage
        * Machine Learning
        * Rekognition
        * CloudSearch
    * Deliver
        * Elemental MediaTailor
* Elemental MediaConvert
    * Access to professional-grade video features and quality
    * No software or hardware infrastructure to manage
    * Automatically scales in response to variations in incoming video volume
    * Ability to manage capacity to control order in which jobs are processed
    * API interface to integrate via Lambda/Serverless functions
    * Basics
        * Job: primary unit of work
        * Output preset: settings to create a single output
        * Job template: commonly used settings
        * Queue: job queue
* AWS Answers
    * Best practices
    * Prescriptive design patterns
    * Ready-made solutions
    * Strategic guidance
* VOD Solution
    * S3 > Ingest Step > Process Step > MediaConvert > Publish Step > S3
* Solutions
    * Version 1 (MVP)
        * Single input, multiple outputs
        * 7 encoding profiles
        * Encoding decision
        * CloudFront for HLS
        * SNS notifications
        * Capture workflow details in DynamoDB
    * Version 2
        * Decoupling: Step outputs metadata instead of updating DynamoDB directly
        * Error Handling: SNS notification to Lambda error handler
        * Metadata: Use metadata file for ingest job specification
        * MediaConvert: Use API for encoding job specification
